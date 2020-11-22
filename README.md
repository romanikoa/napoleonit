# NapoleonIT test 
##ReplicaSet MongoDB 3 nodes on Docker container by Ansible

## Environments setup
Please install ansible on all needed nodes and check it.

Change names of hosts in file ./hosts.yml

## Ansible roles to make MongoDB
* nodes - Prepare nodes, install docker, make images and service
* replica - Compare config files and deploy on databases node for ReplicaSet configuration on MongoDB

## Checked on Ubuntu 16.04 18.04 20.04, Debian 10, Centos 7 8

```sh
ansible --version
```
        ansible 2.7.7

## Prepare python path for ansible on nodes if need it

```sh 
ansible-playbook -i hosts.yml ansible-python.yml
```
Prepare parameters
---
* Change default login password and other parameters in ./roles/default/main.yml
* Check parameter flush_all and docker_inst
* - `flush_all` if it's true delete installed images and folders on nodes
* - `docker_inst` if it's true means you installed docker already and not need install by ansible

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
#result
```
{
        "set" : "ms1",
        "date" : ISODate("2020-11-22T13:12:00.158Z"),
        "myState" : 2,
        "term" : NumberLong(1),
        "syncingTo" : "mongodb2:27017",
        "syncSourceHost" : "mongodb2:27017",
        "syncSourceId" : 1,
        "heartbeatIntervalMillis" : NumberLong(2000),
        "optimes" : {
                "lastCommittedOpTime" : {
                        "ts" : Timestamp(1606050707, 1),
                        "t" : NumberLong(1)
                },
                "appliedOpTime" : {
                        "ts" : Timestamp(1606050717, 1),
                        "t" : NumberLong(1)
                },
                "durableOpTime" : {
                        "ts" : Timestamp(1606050717, 1),
                        "t" : NumberLong(1)
                }
        },
        "members" : [
                {
                        "_id" : 0,
                        "name" : "mongodb1:27017",
                        "health" : 1,
                        "state" : 1,
                        "stateStr" : "PRIMARY",
                        "uptime" : 283,
                        "optime" : {
                                "ts" : Timestamp(1606050717, 1),
                                "t" : NumberLong(1)
                        },
                        "optimeDurable" : {
                                "ts" : Timestamp(1606050717, 1),
                               "t" : NumberLong(1)
                        },
                        "optimeDate" : ISODate("2020-11-22T13:11:57Z"),
                        "optimeDurableDate" : ISODate("2020-11-22T13:11:57Z"),
                        "lastHeartbeat" : ISODate("2020-11-22T13:11:59.248Z"),
                        "lastHeartbeatRecv" : ISODate("2020-11-22T13:11:58.514Z"),
                        "pingMs" : NumberLong(0),
                        "lastHeartbeatMessage" : "",
                        "syncingTo" : "",
                        "syncSourceHost" : "",
                        "syncSourceId" : -1,
                        "infoMessage" : "",
                        "electionTime" : Timestamp(1606050406, 2),
                        "electionDate" : ISODate("2020-11-22T13:06:46Z"),
                        "configVersion" : 3
                },
                {
                        "_id" : 1,
                        "name" : "mongodb2:27017",
                        "health" : 1,
                        "state" : 2,
                        "stateStr" : "SECONDARY",
                        "uptime" : 283,
                        "optime" : {
                                "ts" : Timestamp(1606050717, 1),
                                "t" : NumberLong(1)
                        },
                        "optimeDurable" : {
                                "ts" : Timestamp(1606050717, 1),
                                "t" : NumberLong(1)
                        },
                        "optimeDate" : ISODate("2020-11-22T13:11:57Z"),
                        "optimeDurableDate" : ISODate("2020-11-22T13:11:57Z"),
                        "lastHeartbeat" : ISODate("2020-11-22T13:11:59.153Z"),
                        "lastHeartbeatRecv" : ISODate("2020-11-22T13:11:59.244Z"),
                        "pingMs" : NumberLong(0),
                        "lastHeartbeatMessage" : "",
                        "syncingTo" : "mongodb1:27017",
                        "syncSourceHost" : "mongodb1:27017",
                        "syncSourceId" : 0,
                        "infoMessage" : "",
                        "configVersion" : 3
                },
                {
                        "_id" : 2,
                        "name" : "mongodb4:27017",
                        "health" : 1,
                        "state" : 2,
                        "stateStr" : "SECONDARY",
                        "uptime" : 315,
                        "optime" : {
                                "ts" : Timestamp(1606050717, 1),
                                "t" : NumberLong(1)
                        },
                        "optimeDate" : ISODate("2020-11-22T13:11:57Z"),
                        "syncingTo" : "mongodb2:27017",
                        "syncSourceHost" : "mongodb2:27017",
                        "syncSourceId" : 1,
                        "infoMessage" : "",
                        "configVersion" : 3,
                        "self" : true,
                        "lastHeartbeatMessage" : ""
                }
        ],
        "ok" : 1
}
```
