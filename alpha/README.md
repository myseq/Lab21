# README.md
This is a simple tutorial to learn on basic Ansible playbook to perform sysadm operation.  

  
### Prerequisites
- Install [Multipass](https://multipass.run/install)  
  
  
### What's included
- IaaS provisioning
  - creating 5 VM hosts
- SysAdm operation (Ansible playbooks)
  - create ansible hosts file
  - check if all the hosts are online
  - check disk space usage 
  - update and upgrade packages for all hosts
  - reboot and shutdown
   

## IaaS Provisioning
To provision virtual machines with Multipass.

### Create VM hosts
```powershell
PS> multipass launch -n jimny
PS> multipass launch -n kiko
PS> multipass launch -n lilo
PS> multipass launch -n mimo
PS> multipass launch -n nino
```

### Setup Ansible
```bash
$ 
```

## SysAdm Operation
To manage system operation with Ansible playbook. 

### Create inventory file (/etc/ansible/hosts)
```console
$ cat hosts
[all:vars]
ansible_user=xx
ansible_port=22
ansible_become_method=sudo
ansible_python_interpreter=/usr/bin/python3

[vm]
jimny ansible_host=jimny.mshome.net
kiko ansible_host=kiko.mshome.net
lilo ansible_host=lilo.mshome.net
mimo ansible_host=mimo.mshome.net
nino ansible_host=nino.mshome.net

[ops]
jimny ansible_host=jimny.mshome.net

[group_a]
kiko ansible_host=kiko.mshome.net

[group_b]
lilo ansible_host=lilo.mshome.net
mimo ansible_host=mimo.mshome.net
nino ansible_host=nino.mshome.net

```

### Check if hosts are online
```bash
$ ansible-playbook ping.yml
$ ansible-playbook ping.yml -l group_a
$ ansible-playbook ping.yml -l group_b
```

### Check disk space usage
```bash
$ ansible-playbook df.yml
$ ansible-playbook df.yml -l group_a,group_b
```

## Misc
- [Sample](cloud_init/) cloud_init files  

