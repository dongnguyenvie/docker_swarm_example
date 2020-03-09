# Docker swarm

### Create docker machine

```
docker-machine create --driver virtualbox vps1
docker-machine create --driver virtualbox vps2
docker-machine create --driver virtualbox vps3
```
if breaks swarm ingress then refs: https://github.com/docker/machine/issues/4608

### Set node manager

```
docker-machine ssh vps1
docker swarm init --advertise-addr=<ip-docker-machine>
docker swarm join-token manager
```

### Set not worker

```
docker-machine ssh vps2
docker-machine ssh vps3
docker swarm join --token SWMTKN-1-************ <ip-docker-machine>:<port>
```

### Run service 

```
docker service create --replicas 5 -p 8085:8085 --name testservice dongnguyenvie/public:checkos.node
```
