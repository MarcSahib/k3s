# k3s manually cluster setup with raspberry pi's and rancher

## STEP 1 - Raspberry Pi Headless Setup
- Image the SD card using the Raspberry Pi Imager.
    - Raspberry Pi Imager: https://www.raspberrypi.org/software/
    - Install "Raspberry Pi OS Lite 64 bit arm"

- Insert the SD card into the Raspberry Pi and allow it to boot (3-4 minutes)
- Remove the SD card and plug it back into your computer
- Open the SD card location and open the file "cmdline.txt" - add the following to the :
    - cgroup_memory=1 cgroup_enable=memory
    - ip=192.168.178.200::192.168.178.1:255.255.255.0:pi-01:eth0:off
        - Use this for reference(ip=<ip>::<gw-ip>:<netmask>:<hostname>:<device>:<autoconf>)
- Open the file "config.txt"
    - Add this line of config to the end of the file:
        - add arm_64bit=1
- Enable SSH
    Windows
        Open powershell and change directories to the SD card (ex. d:)
        type this command and hit enter: 
          new-item ssh
          
- Put the SD card back in your raspberry pi and boot. (make sure you plug in your ethernet cable!)


## STEP 2 - K3s Prep
- install tmux using apt
- Update system ```sudo apt update && sudo apt upgrade```
- SSH into your raspberry pi ```ssh pi@192.168.178.200```
- Configure legacy IP tables
    - ```sudo apt install iptables```
    - ```sudo iptables -F```
    - ```sudo update-alternatives --set iptables /usr/sbin/iptables-legacy```
    - ```sudo update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy```
    - ```sudo reboot```


## STEP 3 - K3s Install
- Become root
```sudo su -```

### Install K3s (master setup)

```curl -sfL https://get.k3s.io | K3S_KUBECONFIG_MODE="644" sh -s -```

Get the node token from the master
```sudo cat /var/lib/rancher/k3s/server/node-token```
"K104f700be789e639139510a7a476a305108b06d0b488a866008b0d427f981ed9b0::server:8dfcee72ab13b75632537836ffb95e9a"
"K1075ddfa4bf56cecc49c238df0d575d54d2d2ab55cc1bf60d5ced858c1e01c183f::server:e4e8303429dbd6c5f2176b8daa5d811a"

### STEP 4 - K3s Install (node setup)
Run this command on your other Raspberry Pi nodes
```sudo curl -sfL https://get.k3s.io | K3S_TOKEN="YOURTOKEN" K3S_URL="https://192.168.178.100:6443" K3S_NODE_NAME="servername" sh -```
```sudo curl -sfL https://get.k3s.io | K3S_TOKEN="K1075ddfa4bf56cecc49c238df0d575d54d2d2ab55cc1bf60d5ced858c1e01c183f::server:e4e8303429dbd6c5f2176b8daa5d811a" K3S_URL="https://192.168.178.100:6443" K3S_NODE_NAME="pi-01" INSTALL_K3S_VERSION="v1.27.10+k3s2" sh -``` 



### STEP 5 - Install Ubuntu 18.04 vm to install rancher (optional)
- vbox:
    - user: rancher
    - pass: changeme

#### Rancher credentials
Server URL: https://192.168.178.100:8443     
user: admin 
initial-pass: 2wgs6fs8ldfhgd8gcl956q8dhpgdgdz5b585zkwtzgxxqc8nz2j8qr 
pass: changeme

#### More Notes

### Tests mit Ubuntu VM als temporärer node 
#### Port Freigeben 
Auf dem Mater muss port 6443 geöffnet werden. 

```sh
firewall-cmd --add-port=6443/tcp --permanent
firewall-cm --reload
```


##### env var KUBECONFIG definieren
 Es ist erforderlich die Umgebungsvarialbe KUBECONFIG zu definieren und auf den nodes die IP-Adresse des Masters zu hinterlegen.: 
 export KUBECONFIG=/etc/systemd/system/k3s-agent.service.conf

###### k3s-agent.service.conf content:
Auf den Worker: /etc/systemd/system/k3s-agent.service.conf
Auf dem Master: /etc/rancher/k3s/k3s.yaml

```sh
uapiVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUJlRENDQVIyZ0F3SUJBZ0lCQURBS0JnZ3Foa2pPUFFRREFqQWpNU0V3SHdZRFZRUUREQmhyTTNNdGMyVnkKZG1WeUxXTmhRREUzTURZNE16VXhNVGN3SGhjTk1qUXdNakF5TURBMU1UVTNXaGNOTXpRd01UTXdNREExTVRVMwpXakFqTVNFd0h3WURWUVFEREJock0zTXRjMlZ5ZG1WeUxXTmhRREUzTURZNE16VXhNVGN3V1RBVEJnY3Foa2pPClBRSUJCZ2dxaGtqT1BRTUJCd05DQUFSTHJGS0NDVmhUek9mTGxuNTJ3OXNzTVhHMUZvOXlCM0ZnRDNMZEpSeTIKaUhsRHhVY0NtNU9Md1lWZVY4Z1orbkZ3emhhNC9yOVNHQUNQWkVjY2JSN3BvMEl3UURBT0JnTlZIUThCQWY4RQpCQU1DQXFRd0R3WURWUjBUQVFIL0JBVXdBd0VCL3pBZEJnTlZIUTRFRmdRVVNnc0IzSnFaREFtMGh2K3h6cEJDCmJxcVhXMFl3Q2dZSUtvWkl6ajBFQXdJRFNRQXdSZ0loQU9PVUFOeDFqQ3hkb203dmhqbG5saWQxRTYyQmdMN2kKbUY2dXA1SDZKZVVoQWlFQW5namdPRGx4MVEzYXBSZkVxbTlpeDNWakVkUEZRUXpJbTB5TkdtREYxa1k9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
    server: https://192.168.178.200:6443
  name: default
contexts:
- context:
    cluster: default
    user: default
  name: default
current-context: default
kind: Config
preferences: {}
users:
- name: default
  user: 
    client-certificate-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUJrakNDQVRlZ0F3SUJBZ0lJQno5WS9qMTgxUGt3Q2dZSUtvWkl6ajBFQXdJd0l6RWhNQjhHQTFVRUF3d1kKYXpOekxXTnNhV1Z1ZEMxallVQXhOekEyT0RNMU1URTNNQjRYRFRJME1ESXdNakF3TlRFMU4xb1hEVEkxTURJdwpNVEF3TlRFMU4xb3dNREVYTUJVR0ExVUVDaE1PYzNsemRHVnRPbTFoYzNSbGNuTXhGVEFUQmdOVkJBTVRESE41CmMzUmxiVHBoWkcxcGJqQlpNQk1HQnlxR1NNNDlBZ0VHQ0NxR1NNNDlBd0VIQTBJQUJFYitkQ0dnbjZLOFhaZlkKZGRtQ1AzaDBqa0Q4cHpmcmd4Q28xd29WV3p4Vlo0ZGZUR3g2bmF2TUdoZjZyM0xaYnNaMmVTNXE2VVZqZlUyMwo2RkpJMUlhalNEQkdNQTRHQTFVZER3RUIvd1FFQXdJRm9EQVRCZ05WSFNVRUREQUtCZ2dyQmdFRkJRY0RBakFmCkJnTlZIU01FR0RBV2dCUncvSlZyNXIrTW5OVThZeUVtQjI5RUhkWjIyakFLQmdncWhrak9QUVFEQWdOSkFEQkcKQWlFQWxaQ3NSK21JRjhTVGs4RXl0czZCV25LZW9pQjNuTm5UV0JpZjlKRmNPWThDSVFDSHJSWHZXSmpBQzJiVQpwYmtCR2lkSnhOKzZmNGRTdEhvU1A5UlBEYmw0K0E9PQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCi0tLS0tQkVHSU4gQ0VSVElGSUNBVEUtLS0tLQpNSUlCZHpDQ0FSMmdBd0lCQWdJQkFEQUtCZ2dxaGtqT1BRUURBakFqTVNFd0h3WURWUVFEREJock0zTXRZMnhwClpXNTBMV05oUURFM01EWTRNelV4TVRjd0hoY05NalF3TWpBeU1EQTFNVFUzV2hjTk16UXdNVE13TURBMU1UVTMKV2pBak1TRXdId1lEVlFRRERCaHJNM010WTJ4cFpXNTBMV05oUURFM01EWTRNelV4TVRjd1dUQVRCZ2NxaGtqTwpQUUlCQmdncWhrak9QUU1CQndOQ0FBVG5PWThhM2s4QTNUSHU4SzUrR2IyQ3M4dHFOcy85VzlENzdWM21RaWhlCmEvdUErVkw2aE5jYVU3SEo3ZXl1MFZIRi9WY3RRbFRaV3hteXRsZDU5OGZubzBJd1FEQU9CZ05WSFE4QkFmOEUKQkFNQ0FxUXdEd1lEVlIwVEFRSC9CQVV3QXdFQi96QWRCZ05WSFE0RUZnUVVjUHlWYSthL2pKelZQR01oSmdkdgpSQjNXZHRvd0NnWUlLb1pJemowRUF3SURTQUF3UlFJaEFNMy9FeFpmbXkvQkRINEhmSzM5TkovVE1UOUtFaWxSCnVma0hDY3dmSGV6MEFpQTBlSnZHNC9OYmRhSzArMXBsR3VSZElKbXBjVEdyVk5BVWRaL2toMkJYcVE9PQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
    client-key-data: LS0tLS1CRUdJTiBFQyBQUklWQVRFIEtFWS0tLS0tCk1IY0NBUUVFSUZielZlODMvOHdESFdjVlFHMXZlZDR1L1kydFlmRkE3dFcyS2N1U2ZlSTRvQW9HQ0NxR1NNNDkKQXdFSG9VUURRZ0FFUnY1MElhQ2ZvcnhkbDloMTJZSS9lSFNPUVB5bk4rdURFS2pYQ2hWYlBGVm5oMTlNYkhxZApxOHdhRi9xdmN0bHV4blo1TG1ycFJXTjlUYmZvVWtqVWhnPT0KLS0tLS1FTkQgRUMgUFJJVkFURSBLRVktLS0tLQo=
```

###### Installation von k3s-agent auf pi-01:
curl -sfL https://get.k3s.io | K3S_TOKEN="K104f700be789e639139510a7a476a305108b06d0b488a866008b0d427f981ed9b0::server:8dfcee72ab13b75632537836ffb95e9a" K3S_URL="https://192.168.178.200:6443" K3S_NODE_NAME="ubuntu" sh


##### IP-Adressen
192.168.178.100 - Master (aktuell noch 192.168.178.200)
192.168.178.101 - pi-01  (aktuell noch ubuntu 192.168.178.100)
               

## Rancher

Rancher Server has been installed.

NOTE: Rancher may take several minutes to fully initialize. Please standby while Certificates are being issued, Containers are started and the Ingress rule comes up.

Check out our docs at https://rancher.com/docs/

If you provided your own bootstrap password during installation, browse to https://rancher.hood.lcl to get started.

If this is the first time you installed Rancher, get started by running this command and clicking the URL it generates:

```
echo https://rancher.hood.lcl/dashboard/?setup=$(kubectl get secret --namespace cattle-system bootstrap-secret -o go-template='{{.data.bootstrapPassword|base64decode}}')
```

To get just the bootstrap password on its own, run:

```
kubectl get secret --namespace cattle-system bootstrap-secret -o go-template='{{.data.bootstrapPassword|base64decode}}{{ "\n" }}'
```

## k8s kubectl lokal (auf eigenen lappi) installiert
https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-using-native-package-management
