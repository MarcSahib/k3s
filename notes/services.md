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



## 15.02.2024
### pi-hole with metal lb
Pihole:
- https://github.com/colin-mccarthy/k3s-pi-hole
    - traefik deinstalliert von cluster
    - metallb installiert
    - metallb adress pools defininert
    - apply -f manifests

- metallb via helm installiert auf master
```sh
helm repo add metallb
helm repo add metallb https://metallb.github.io/metallb
helm repo list
helm search repo metallb
helm upgrade --install metallb metallb/metallb --create-namespace --namespace metallb-system --wait
```
- ingress Objekt erstellt
- kubectl get certificate 
    error
    kubectl logs -f -n cert-manager cert-manager-594b84b49d-svg2b
