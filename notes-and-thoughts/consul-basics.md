# Consul Tutorials
## Katacode -  Basics
* https://www.katacoda.com/courses/consul/basics-of-consul

Main functions:
* Service Registration & Discovery
* Health Check
* Key-Value Store

What is it?
Consul is distributed system where agent nodes communicate with server nodes:
* Servers are responsible to maintain the cluster' state
* An agent is responsible to perform health checks of the node it's running on and also of the services running on that node
* Static binary written in Go

### Exercise
Goal:
To set up a functioning dev cluster using Consul in Development mode

* Step 1: Getting Started
  * ```consul agent -dev -client=0.0.0.0```
* Step 2: Checking cluster members
  * ```consul members```
  * Outputs member nodes with hostname, ip address, status, type, build, protocol, dc and segment.
* Get list of cluster members
  * ```curl localhost:8500/v1/catalog/nodes```
  * /catalog allows for register, deregister & list of nodes, listens on `:8500`
* Step 3: Register a Service
  * Start an example webservice
    * `docker run --name nginx -d -p 80:80 nginx`
  * Define this service for Consul in web.json
  * use `agent/service/register` endpoint and provide content of `web.json`
  * `curl -X PUT --data-binary @web.json http://localhost:8500/v1/agent/service/register`
  * Verify service is registered
    * `curl http://localhost:8500/v1/agent/services`
    * Outputs service information such as  ID, Service, Tags, Address, Port etc.
* Step 4: Use DNS Embedded Server
  * Consul provides a DNS server at `:8600` we can use to get ip of registered service
  * `dig @127.0.0.1 -p 8600 web.service.consul`
    * Retrieving fully qualified name of service
  * `dig @127.0.0.1 -p 8600 web.service.consul SRV`
    * Also brings up the service's port
* Step 5: Health Checks
  * Consul agent will periodically check service health according to it's service definition, using http, tcp or a script and flags service as unhealthy if it cannot connect
  * Alternatively, a service can define it's TTL and periodically send it's health to the consul Agent
    * The service is flagged as unhealthy if TTL is not met
  * Deregister service
    * `curl http://localhost:8500/v1/agent/service/deregister/web`
  * Recreate the service with a defined health check (adds a check block to the json) & register with
    * `curl -X PUT --data-binary @web-health.json http://localhost:8500/v1/agent/service/register`
  * Stop container to see what happens with healthcheck
    * `docker container stop nginx`
    * it is now flagged as failing in the UI
    * `curl http://localhost:8500/v1/health/checks/web` to get health status
  * Attempt to query status  
    * `dig @127.0.0.1 -p 8600 web.service.consul`
    * IP not returned by Consul as the service is no longer healthy
  * Step 6: Key Value Store
    * Accessible by UI or HTTP endpoint
    * Demonstrate with `consul kv put redis/config/maxconns 25`
      * `consul kv get redis/config/maxconns`
