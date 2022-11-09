# README.md
This is a simple tutorial to learn on basic Ansible playbook to perform sysadm operation.  

  
### Prerequisites
- Install [Multipass](https://multipass.run/install)  
  
  
### What's included
- IaaS provisioning
  - creating 5 VM hosts
  - create ansible hosts file
- SysAdm operation (Ansible playbooks)
  - check if all the hosts are online
  - run ad hoc command at all hosts
  - check disk space usage with playbook
  - update and upgrade packages for all hosts
  - reboot and shutdown
   

## IaaS Provisioning
To provision virtual machines with Multipass.

### Create and setup VM host (Control Node)
```powershell
PS> multipass launch -n jimny --cloud-init ci_update.yml
```

Setup Ansible
```powershell
PS> multipass shell jimny
```
```bash
ubuntu@jimny:~$ sudo apt-add-repository ppa:ansible/ansible
ubuntu@jimny:~$ sudo apt update
ubuntu@jimny:~$ sudo apt install ansible
ubuntu@jimny:~$ ansible-config init --disabled > ansible.init
```

Create inventory file (/etc/ansible/hosts)
```console
$ cat /etc/ansible/hosts
[all:vars]
ansible_user=ubuntu
ansible_port=22
ansible_become_method=sudo
ansible_python_interpreter=/usr/bin/python3

[vm]
kiko ansible_host=kiko.mshome.net
lilo ansible_host=lilo.mshome.net
mimo ansible_host=mimo.mshome.net
nino ansible_host=nino.mshome.net

[ops]
#jimny ansible_host=jimny.mshome.net

[group_a]
kiko ansible_host=kiko.mshome.net

[group_b]
lilo ansible_host=lilo.mshome.net
mimo ansible_host=mimo.mshome.net
nino ansible_host=nino.mshome.net
```

Generate SSH public key
```console
ubuntu@jimny:~$ ssh-keygen -t ed25519 -C "ubuntu@jimny"
ubuntu@jimny:~$ cat .ssh/id_ed25519.pub
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAII9Cox0CIU2YiQLH2RdLSjI+nNH/z+kB9XGUvHvtKxgF xx@jimny
ubuntu@jimny:~$ cat << EOF >> ci_sshkey.yaml
> #cloud-config
> users:
>   - name: ubuntu
>     no_ssh_fingerprints: true
>     ssh_authorized_keys:
>       - ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAII9Cox0CIU2YiQLH2RdLSjI+nNH/z+kB9XGUvHvtKxgF ubuntu@jimny
>
> ssh:
>   emit_keys_to_console: false
>
> EOF
ubuntu@jimny:~$
```

### Create and setup VM hosts (Managed Nodes)
```powershell
PS> multipass launch -n kiko --cloud-init ci_sshkey.yaml
PS> multipass launch -n lilo --cloud-init ci_sshkey.yaml
PS> multipass launch -n mimo --cloud-init ci_sshkey.yaml
PS> multipass launch -n nino --cloud-init ci_sshkey.yaml
```


## SysAdm Operation
To manage system operation with Ansible playbook. 

### Check if hosts are online
```console
ubuntu@jimny:~$ ansible all -m ping
lilo | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
kiko | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
nino | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
mimo | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

### Run an ad hoc command at multiple hosts
```console
ubuntu@jimny:~$ ansible all -a "lsb_release -a"
kiko | CHANGED | rc=0 >>
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.1 LTS
Release:        22.04
Codename:       jammyNo LSB modules are available.
lilo | CHANGED | rc=0 >>
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.1 LTS
Release:        22.04
Codename:       jammyNo LSB modules are available.
mimo | CHANGED | rc=0 >>
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.1 LTS
Release:        22.04
Codename:       jammyNo LSB modules are available.
nino | CHANGED | rc=0 >>
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.1 LTS
Release:        22.04
Codename:       jammyNo LSB modules are available.
ubuntu@jimny:~$
```
### Check the uptime and login user at specific host
ubuntu@jimny:~$ ansible kiko -a "uptime"
kiko | CHANGED | rc=0 >>
 17:08:51 up 6 min,  1 user,  load average: 0.01, 0.01, 0.00
ubuntu@jimny:~$ ansible kiko -a "w"
kiko | CHANGED | rc=0 >>
 17:08:57 up 6 min,  1 user,  load average: 0.01, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
ubuntu   pts/0    172.24.196.44    17:08    0.00s  0.07s  0.00s w
ubuntu@jimny:~$
```

### Install nmap package at kiko
```bash
ansible kiko -b -m apt -a "name=nmap state=latest"
ubuntu@jimny:~$ ansible kiko -a "nmap jimny"
kiko | CHANGED | rc=0 >>
Starting Nmap 7.80 ( https://nmap.org ) at 2022-11-09 17:11 +08
Nmap scan report for jimny (172.24.196.44)
Host is up (0.00072s latency).
rDNS record for 172.24.196.44: jimny.mshome.net
Not shown: 999 closed ports
PORT   STATE SERVICE
22/tcp open  ssh

Nmap done: 1 IP address (1 host up) scanned in 0.03 seconds
ubuntu@jimny:~$
```  

### PING all hosts (with playbook)
```bash
$ ansible-playbook asb_ping.yml
$ ansible-playbook asb_ping.yml -l group_a
$ ansible-playbook asb_ping.yml -l group_b
```

### Check disk space usage (with playbook)
```bash
ubuntu@jimny:~/playbooks$ ansible-playbook asb_df.yml
ubuntu@jimny:~/playbooks$ ansible-playbook asb_df.yml -l group_a,group_b
```

### Update/upgrade packages for all hosts
```bash
ubuntu@jimny:~/playbooks$ ansible-playbook asb_update.yml
```

### Reboot hosts
```bash
ubuntu@jimny:~/playbooks$ ansible-playbook asb_reboot.yml
```
  
### Shutdown all hosts
```bash
ubuntu@jimny:~/playbooks$ ansible-playbook asb_poweroff.yml
```

Verify if all VM hosts are shutdown
```powershell
PS> multipass list
Name                    State             IPv4             Image
jimny                   Running           172.24.196.44    Ubuntu 22.04 LTS
kiko                    Stopped           --               Ubuntu 22.04 LTS
lilo                    Stopped           --               Ubuntu 22.04 LTS
mimo                    Stopped           --               Ubuntu 22.04 LTS
nino                    Stopped           --               Ubuntu 22.04 LTS
```  

  
## Misc
- Some cloud_init [sample files](cloud_init/)  


