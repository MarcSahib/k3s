## Use Ansible Playbooks
- Ping all nodes:
  
  ```ansible-playbook -i inventory/inventory.yml playbook/ping.yml```

- Perform "apt update && apt upgrade" on all nodes:

  ```ansible-playbook -i inventory/inventory.yml playbook/apt_upgrade.yml --ask-become-pass```

- Reboot all nodes:

  ```ansible-playbook -i inventory/inventory.yml playbook/reboot.yml --ask-become-pass```

- Install cluster from scratch:

  ```ansible-playbook -b playbook/site.yml -i inventory/inventory.yml --ask-become-pass```

- Reset cluster (cleaning!): 
  
  ```ansible-playbook -i inventory/inventory.yml playbook/reset.yml --ask-become-pass``` 

- Upgrade/Downgrade cluster: 
  Set specific k3s_version in inventory/inventory.yml and run

  ```ansible-playbook -i inventory/inventory.yml playbook/upgrade.yml --ask-become-pass```



## Installation Ansible
https://linuxandi.com/posts/how-to-install-ansible-on-ubuntu-22-04/

```sh
sudo apt update
sudo apt install python3 python3-pip
sudo pip3 install virtualenv
virtualenv ansible-env
source ansible-env/bin/activate
pip3 install ansible
ansible --version
```

## Sidequests

### aktuelisieren der Uhrzeit in WSL
Ich konnte keine Pakete mehr mit yum Aktualisieren, da die Systemzeit falsch war. 
https://software-berater.net/2022/uhrzeit-sync-fuer-wsl/ 

In der Datei 
/etc/systemd/timesyncd.conf

[Time]
NTP=0.de.pool.ntp.org 1.de.pool.ntp.org 2.de.pool.ntp.org
FallbackNTP=ntp.ubuntu.com

systemctl edit --full systemd-timesyncd

### Neuinstallation von vscode 
https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode 
