---
date: 2016-03-09T00:11:02+01:00
title: Configuring nsproxy
weight: 10
---

Nsproxy uses **gcfg** (INI-style config files for go structs).

```
[server]
	port				= 53
	loglevel			= "cluster"

[clustermanager]
	useclustermanager	= true
	port				= 8080
	pingfreq			= 4
	clientpingtype		= "port"
	connectiondrain		= 10

[dns]
	ttl					= 60

[upstreamdns]
	server				= "8.8.8.8:53"

[redis]
	host				= "localhost:6379"
	password			= ""
```

The config uses some pretty sane defaults but the following fields are configurable:

- `[server]`
  - `port:`  the port the main DNS server listens on.  
  - `loglevel:`  the verbosity of logs. acceptable fields are 'info', 'cluster', 'debug', and 'error'.  
- `[clustermanager]`
  - `useclustermanager:`  whether or not to use the cluster manager. acceptable fields are 'true' and 'false'  
  - `port:`  the port that cluster manager will listen on (this is what port clients use to check in)  
  - `pingfeq:`  the ammout of time in between health checks (in seconds)  
  - `clientpingtype:`  the type of ping to be used. acceptable fields are 'icmp' and 'port'.  If port is set, client must send 'port' field in cluster registration.  
  - `connectiondrain:`  the time in seconds for the connection to drain. this is a grace period between a bad health check and actually removing the host from rotation. set to 0 if this is uneeded.  
- `[dns]`
  - `ttl:`  the default time to live (in seconds) for dns entries  
- `[upstreamdns]`
  - `server:`  the dns server and port that nsproxy uses if it cannot find a match in the local database  
- `[redis]`  
  - `host:`  this is the ip and port that the redis backend is running on  
  - `password:`  password to the redis database if one exists
