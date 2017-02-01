---
date: 2016-03-09T00:11:02+01:00
title: REST API Endpoints
weight: 10
---

## API

Nsproxy exposes an api for easy access to data and creating DNS records.  The following is the specification for endpoints and their protocols.  

## Endpoints

**Get description**
----
  Returns registered id's content

* **URL**  
  /:registered_id
* **Method**  
  `GET`
* **Sample Call**  
  ```bash
  curl localhost:8080/unixvoid
  ```


**Register DNS Entry**
----
  Register a host with the cluster manager

* **URL**  
  /
* **Method**  
  `POST`
* **URL Params**  
  **Required:**  
  `hostname` : hostname of the box  
  `cluster`  : cluster the host is associated with  
  `port`     : port to health check on  
  **Optional**  
  `ip` : (optional) the ip the box is on, you only need to set this if the client is behind a firewall  
  `weight` : (optional) the weight of the box (for load balancing). This field defaults to '1'. This is how many load balance hits the box will receive in a row before continuing to the next live host 
* **Sample Call**  
  ```bash
  curl -d hostname=$1 -d cluster=smartos -d port=7700 192.168.2.201:8080
  ```


**Add Loadbalancer Entry**
----
  Register a DNS entry

* **URL**  
  /dns  
* **Method**  
  `POST`  
* **URL Params**  
  **Required:**  
  `dnstype` : the type of request being made: supported {a, aaaa, cname}  
  `domain`  : domain name, this will be fully qualified by nsproxy if it is not already  
  `value`   : the entry value {ip4 address for 'a', ipv6 address for 'aaaa', aname for 'aname'}  
* **Sample Call**  
  ```bash
  curl -d dnstype=CNAME -d domain=bitnuke.io -d value=turbo.lb.bitnuke.io localhost:8080/dns
  ```


**Remove DNS Entry**
----
  Remove a DNS entry from the nameserver

* **URL**  
  /dns/rm  
* **Method**  
  `POST`  
* **URL Params**  
  **Required:**  
  `domain` : the domain to remove entry for  
  **Optional:**  
  `dnstype` : (optional) dns request to remove {a, aaaa, cname}. If not set, will remove all three from the db for the domain  
* **Sample Call**  
```bash
  curl -d dnstype=a -d domain=bitnuke.io localhost:8080/dns/rm
```


**Get Cluster Metadata**
----
  Get metadata for a particular cluster (returns registred hosts)

* **URL**  
  /clusterspec  
* **Method**  
  `POST`  
* **URL Params**  
  **Required:**  
  `cluster` : cluster name to get hosts for  
* **Sample Call**  
```bash
  curl -d cluster=smartos localhost:8080/clusterspec
```


**Get Host Metadata**
----
  Get metadata for a particular host (in a cluster)

* **URL**  
  /hostspec  
* **Method**  
  `POST`  
* **URL Params**  
  **Required:**  
  `cluster` : cluster name that the host belongs to  
  `host` : hostname to get the ip of  
* **Sample Call**  
```bash
  curl -d cluster=smartos -d host=test1 localhost:8080/hostspec
```


**Get DNS entry metadata**
----
  Get information about a particular DNS entry (ie: A, AAAA, CNAME.. data)

* **URL**  
  /dnstype  
* **Method**  
  `POST`  
* **URL Params**  
  **Required:**  
  `dnstype` : the dnstype of the entry, default is 'A' if not set  
  `domain` : domain name of the entry to return  
* **Sample Call**  
```bash
  curl -d dnstype=cname -d domain=google.com localhost:8080/dnsspec
```


**Get Live Hosts**
----
  Get a list of all live hosts (in the format <cluster>:<host>)

* **URL**  
  /hosts  
* **Method**  
  `GET`  
* **Sample Call**  
```bash
  curl localhost:8080/hosts
```


**Get Live Clusters**
----
  Get a list of all live clusters

* **URL**  
  /clusters  
* **Method**  
  `GET`  
* **Sample Call**  
```bash
  curl localhost:8080/clusters
```


**Get DNS Entries**
----
  Get a list of all DNS Entries

* **URL**  
  /dns  
* **Method**  
  `GET`  
* **Sample Call**  
```bash
  curl localhost:8080/dns
```
