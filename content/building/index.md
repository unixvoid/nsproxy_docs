---
date: 2016-03-09T00:11:02+01:00
title: Building nsproxy
weight: 10
---

This project requires golang to be installed with the dependencies in place.

- To pull the dependencies on your box simply issue `make deps` to do all the `go get`s for you.  
- make will accept the following commands:  
  - `make nsproxy` will dynamically build nsproxy
  - `make stat` will statically build nsproxy
  - `make stage` will build and stage all files for preperation of building a
     container
  - `make link` will link the project to your GOPATH, this way if a pkg changes it will be updated in your GOPATH as well
  - `make deps` will do all the required `go get`s for you
  - `make populate` will populate the local DNS server with entries for testing
  - `make testhealthcheck` will run a health check docker against a local version of nsproxy
  - `make rmhealthcheck` will remove the docker containers made by `testhealthcheck`
  - `make test` will test against the entries added by `make populate`
  - `make docker` will build a docker image.  You can specify a non default docker name by editing the `IMAGE_NAME` field of the Makefile  
  - `make install` will install the compiled nsproxy in /usr/bin/
  - `make clean` will clean the project of all tmp directories and binaries
