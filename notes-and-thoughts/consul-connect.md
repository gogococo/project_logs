# Consul Tutorials
## Katacode -  Consul Connect
* https://www.katacoda.com/hashicorp/scenarios/consul-connect
* https://github.com/hashicorp/katakoda/tree/master/consul-connect

Exercise Goal:
* Start two services and connect them over TLS encrypted proxy via Consul Connect
* Consul configured with a single agent to get started
* Start 4 processes
  * Frontend with demo dashboard web app, displays a number
    * Uses websockets to update itself in the UI every few seconds with fresh data from backend
    * Displays status info to see when the connection was made
  * Backend, a counting service that adds JSON feed with a constantly incrementing number
  * Two consul connect proxies representing the dashboard and counting services so they can communicate

### Exercise
Step 1: Start Counting Service
* Consul already running on a publically accessible IP address, UI at :8500
* Will start with x beside `counting` and `dashboard` as neither service is currently running
* `cat /etc/consul.d/counting.json`
  * outputs configuration for service name, port, connect block & healthcheck block  
* Could reach this service as `counting.service.consul` through DNS
* Connect Sidecar is what enables proxy communication, but doesn't define any connections to start. Needs to be manually started
* Health Check examines local `/health` endpoint every second for health & ability to expose to other services
* Start service `PORT=9003 counting-service`
  * output of counting service is `{"count":1,"hostname":"host01"}`
  * Note  - Counting service now pops up as healthy in ui!
Step 2: Start the Dashboard
* `/etc/consul.d/dashboard.json` notably has proxy:upstream, connecting to counting & locally binding port :9001
* `PORT=9002 COUNTING_SERVICE_URL=http://localhost:9001 dashboard-service`
Step 3: Start Secure Sidecare Proxies
* `consul connect proxy -sidecar-for counting`
  * Consul can read `/etc/consul.d/counting.json` to get service name & port
* `consul connect proxy -sidecar-for dashboard`
  * Upstream config is where we defined which services we want to talk to
* Consul Load Balances across all health counting services defined on the network (maybe even other data centers if it's leveraging multi-datacenter Consul features)
* Can validate success through Dashboard where it shows the number correctly incrementing or through consul web ui where it shows healthy status for all services & proxies
Step 4: Lock down connections using Deny
* Can control communication between services by defining `intentions` in Consul
* In Gui, go to `Intentions` -> `Create`
  * Start by denying all communication between all services
  * `* - *` with `Deny`
Step 5: Allow Specific connections with `Allow`
* `Dashboard - Counting` with `Allow`
* no need to restart service since intentions which Allow connectivity will take effect dynamically.
