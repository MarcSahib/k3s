## K3S Netzplan

| Hostname   | IP                                | MAC | Tpye                          | user    | pass     | Description 
|------------|-----------------------------------|-----|-------------------------------|---------|----------|
| pi-ma      | 192.168.178.100                   |     | Master, ControlPlane          | master  | changeme |
| pi-01      | 192.168.178.101                   |     | Worker                        | worker  | changeme |
| pi-02      | 192.168.178.102                   |     | Worker                        | worker  | changeme |
| reserviert | 192.168.178.110 - 192.168.178.199 |     | für load balancing (metal lb) | -       | -        |
| rancher    | 192.168.178.200                   |     | Control, Monitoring           | rancher | changeme |
| metal lb   | 192.168.178.201                   |     | Control, Monitoring           | rancher | changeme |
| longhorn   | 192.168.178.202                   |     | Control, Monitoring           | rancher | changeme |

| Service                              | IP              | DNS                 | port intern | port extern | Description                |
|--------------------------------------|-----------------|---------------------|-------------|-------------|----------------------------|
| metalLB loadbalancer für tls         | 192.168.178.110 | pihole.hood.lcl     |             |             | DNS, Ad-Blocker            |
| pihole                               | 192.168.178.111 | pihole.hood.lcl     |             |             | DNS, Ad-Blocker            |
| Grafana                              | 192.168.178.112 | grafana.hood.lcl    |             |             | Monitoring, Visualisierung |
| Prometheus                           | 192.168.178.113 | prometheus.hood.lcl |             |             | TS-DB                      |
| Alert Manager                        | 192.168.178.114 | am.hood.lcl         |             |             |                            |
| juice-shop                           | 192.168.178.115 | juice-shop.hood.lcl |             |             |                            |
| Nextcloud                            | 192.168.178.116 | nc.hood.lcl         |             |             |                            |
| Harbor, bessere alternative für k8s? | 192.168.178.117 | harbor.hood.lcl     |             |             |                            |
