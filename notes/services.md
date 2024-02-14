## Benötigte Services:
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

