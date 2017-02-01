---
date: 2016-03-09T00:11:02+01:00
title: Nsproxy Basic Usage
weight: 10
---

The following usage implies the default config file is being used.  

- On boot nsproxy will bind to two ports:  
  - `53` is used as the regular dns server.  This will act the same as any other dns server and allows for custom dns entries to be used.  
  - `8080` is used as the cluster manager.  Clients should post to this port to bind with the dns server.  
- DNS entries can be added in a similar fashion to registering a host.  A POST on the same port that the clustermanager is running on in the following format will add an entry to the dns server.  
  - `/dns` dnstype= domain= value=  
    - example `curl -d dnstype=a domain=unixvoid.com value=192.168.1.80 localhost:8080/dns`  
  - `/dns/rm` dnstype= domain= will remove a dns entry (and all for a domain if 'dnstype' not set)  
    - example `curl -d dnstype=a domain=unixvoid.com localhost:8080/dns/rm`  
  - `dns:<dns_type>:<fqdn>` and the content being a valid A, AAAA, or CNAME entry.  
- Here are some examples on what typical redis entries would look like.  
  - entry: `dns:a:unixvoid.com.` content: `67.3.192.22`  
  - entry: `dns:aaaa:unixvoid.com.` content: `::1`  
  - entry: `dns:cname:unixvoid.com.` content: `customlb.cname.`  
- To register a client with the cluster manager, the client will send a form
    (`application/x-www-form-urlencoded`) to nsproxy with the following data.
    - `hostname`:  the hostname of the box  
    - `cluster`:  the intended cluster to join.  
    - `ip`(optional):  ip (usefull when client is behind proxy/loadbalancer).  
    - `port`(optional): port to tcp health check on, must be set if
      clientpingtype = port.  
  - Both of these fields are required.  
- A regular client registration looks like this:  
    `curl -d hostname=nginx -d cluster=coreos unixvoid.com:8080`  This will add the host `nginx` to the cluster `coreos`.  These names are arbitrary and can be anything.  
