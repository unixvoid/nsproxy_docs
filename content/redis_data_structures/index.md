---
date: 2016-03-09T00:11:02+01:00
title: Redis Data Structures with nsproxy
weight: 10
---

This is designed to help the user understand what is going on inside of redis at a low level.  The following are redis keys (with examples) and what they store.

- `cluster:<cluster_name>:<host_name>` This is a low level entry that has a hostname's ip
  - type: redis key
    - content: host ip
    - example `cluster:coreos:nginx` 192.168.2.2
- `port:<cluster_name>:<host_name>` This is a low level entry that has a hostname's port
  - type: redis key
    - content: host port
    - example `port:coreos:nginx` 443
- `index:cluster:<cluster_name>` This is a medium level entry that contains the elements (hosts) that are in a cluster
  - type: redis set
  - content: cluster hosts
  - example `index:cluster:coreos` {nginx, cApp, configServer}
- `list:cluster:<cluster_name>` This is a clone of the previous, used to hold order or load balanced hosts. This element stays in order to obey load balancer algoritms
  - type: redis unordered set (list)
  - content: cluster hosts
  - example `list:cluster:coreos` {nginx, cApp, configServer}
- `state:cluster:<cluster_name>` This is a statefile for any draining host. When a host begins draining it adds itself the the state entry so a single box is not entered twice.
  - type: redis unordered set (list)
  - content: draining cluster ip:port
  - example `state:cluster:coreos` {192.168.1.9:8080, 192.168.1.8:4410}
- `index:master` This a persistent index that hold a list of all clusters (this persists across nsproxy reboots)
  - type: redis set
  - content: clusters
  - example `index:master` {coreos, neatCluster, ps2_cluster}
- `index:live` This is volitile entry that gets diffed against `index:master` and only contains hosts that have live listeners
  - type: redis set
  - content: clusters
  - example `index:master` {coreos, neatCluster, ps2_cluster}
- `dns:<dns_type>:<domain>` This is the standard dns entry  
  - type: redis set  
  - content: a, aaaa, cname entry  
  - example `dns:a:unixvoid.com.` 192.168.1.80  
- `weight:<cluster_name>:<host_name>` This the entry that holds the master weight of the box for load balancing 
  - type: redis key  
  - content: the weight the box (defaults to 1)  
  - example `weight:testapp:4146b6ec7810` 5  
- `cweight:<cluster_name>:<host_name>` This the entry that holds the current weight of the box for load balancing. This field is volitile 
  - type: redis key  
  - content: the current weight the box  
  - example `cweight:testapp:4146b6ec7810` 3  
