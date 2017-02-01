---
date: 2016-03-08T21:07:13+01:00
title: DNS based load balancer
type: index
weight: 0
---

## Nsproxy documentation

[![Build Status (Travis)](https://travis-ci.org/unixvoid/nsproxy.svg?branch=develop)](https://travis-ci.org/unixvoid/nsproxy)  

Nsproxy is a DNS proxy and cluster manager written in go.  This project acts as
a normal DNS server (in addition to the cluster managment) and allows the use of
custom DNS entries.  Currently nsproxy fully supports A, AAAA, and CNAME
entries.

![Banner](img/banner.png)


## Quick start

There are 3 main ways to run nsproxy:  

1. **Docker**:  
  we have nsproxy pre-packaged over on the [dockerhub](https://hub.docker.com/r/unixvoid/nsproxy/), go grab the latest and run: 
  ```bash
  docker run -d -p 8080:8080 -p 53:53 unixvoid/nsproxy
  ```

2. **ACI/rkt**:  
  we have public rkt images hosted on the site! check them out [here](https://cryo.unixvoid.com/bin/rkt/nsproxy/) or go give us a fetch for 64bit machines!
  ```bash
  rkt fetch unixvoid.com/nsproxy
  ```
  This image can be run with rkt or you can
  grab our handy [service file](https://github.com/unixvoid/cryodns/blob/master/deps/cryodns.service)

3. **From Source**:  
  Are we not compiled for your architecture? Wanna hack on the source?  Lets bulid and deploy:  
  ```bash
  make dependencies
  make run
  ```  

  If you want to build a docker use: `make docker`  
  If you want to build an ACI use: `make aci`


## Acknowledgements

Big shoutout to the guys over at miekg, this project makes use of their [dns library](https://github.com/miekg/dns) for go.
