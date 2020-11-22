# napoleonit test ReplicaSet MongoDB on 3 instances on Docker by ansible
```
## Environments setup
Please install ansible on all needed nodes and check it.

Change names of hosts in file ./hosts.yml

## Ansible roles to make MongoDB
* nodes - Prepare nodes, install docker, make images and service
* replica - Compare config files and deploy on databases node for ReplicaSet configuration on MongoDB

## Checked on Ubuntu 16.04 18.04 20.04, Debian 10, Centos 7 8
```sh
ansible --version
ansible 2.7.7
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/romanikoaa/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.7.3 (default, Jul 25 2020, 13:03:44) [GCC 8.3.0]
```
## Prepare python path for ansible on nodes if need it
```sh 
ansible-playbook -i hosts.yml ansible-python.yml
```
Prepare parameters
---
* Change default login password and other parameters in ./roles/default/main.yml
* Check parameter flush_all and docker_inst
 - flush_all if it's true delete installed images and folders on nodes
 - docker_inst if it's true means you installed docker already and not need install by ansible

Provision MongoDB on docker container by ansible
---

```sh
ansible-playbook -i hosts.yml ansible-playbook.yml
```

For check replicaset you can connect to node and use command shell
```sh
sudo docker exec -it mongod_server mongo
> rs.status()

```
