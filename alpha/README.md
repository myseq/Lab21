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

### Create and setup VM host (Control Node)
```powershell
PS> multipass launch -n jimny --cloud-init ci_ansible.yml
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
$ cat hosts
[all:vars]
ansible_user=ubuntu
ansible_port=22
ansible_become_method=sudo
ansible_python_interpreter=/usr/bin/python3

[vm]
#jimny ansible_host=jimny.mshome.net
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
ubuntu@jimny:~$ ssh-keygen -t ed25519 -C "xx@jimny"
ubuntu@jimny:~$ echo ssh_authorized_keys: > ci_ansible2.yaml
ubuntu@jimny:~$ echo "  - `cat .ssh/id_ed25519.pub`" >> ci_ansible2.yaml
```

### Create and setup VM hosts (Managed Nodes)
```powershell
PS> multipass launch -n kiko --cloud-init ci_ansible2.yaml
PS> multipass launch -n lilo --cloud-init ci_ansible2.yaml
PS> multipass launch -n mimo --cloud-init ci_ansible2.yaml
PS> multipass launch -n nino --cloud-init ci_ansible2.yaml
```


## SysAdm Operation
To manage system operation with Ansible playbook. 


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
- Some cloud_init [sample files](cloud_init/)  


