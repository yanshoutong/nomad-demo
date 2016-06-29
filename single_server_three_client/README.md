## Quick demo for Nomad and Consul

This demo to quickly show you how to use nomad and consul setup a docker scheduler across cluster host as well as docker service discovery

#### Nomad & Consul server setup 
3 VMs : Docker1 Docker2 Docker3

Docker1 :  Nomad & Consul server as well as client

Docker2 :  Nomad & Consul client 

Docker3 :  Nomad & Consul client

#### Bootstrap 
$ vagrant up


#### Fire up a Job
$ vagrant ssh docker1

vagrant@docker1:~$ cd /vagrant

agrant@docker1:/vagrant$ nomad run example.nomad


#### View nodes & service in consul 
http://192.168.0.20:8500

Get docker instance meta-data from consul 

$ curl http://192.168.0.20:8500/v1/catalog/service/cache-redis

#### Check job status from nomad client

vagrant@docker1:~$ nomad node-status

vagrant@docker1:~$ nomad status example

###### beer!

## For two nomad servers configuration

#### Nomad & Consul server setup 
2 VMs : Docker1 Docker2

Docker1 :  Nomad server/client + Consul server/client

Docker2 :  Nomad server/client + Consul server/client
 
$ git checkout twoservers