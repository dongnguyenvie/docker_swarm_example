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

```
# Liệt kê các service trên swarm
docker service ls

# Liệt kê các container cho dịch vụ có tên testservice
docker service ps testservice

# Kiểm tra log cho dịch vụ testservice
docker service logs testservice

# Scale - thay đổi số container cho dịch vụ testservice đang chạy thành n (1, 2, 3 ...) container
docker service scale testservice=n

# Cập nhật thiết lập cho dịch vụ testservice đang chạy
# - Thay đổi Image
docker service update --image=ichte/swarmtest:php testservice
# - Thay đổi tài nguyên CPU, MEM
docker service update --limit-cpu="0.5"  --limit-memory=150MB testservice
# - Các cập nhật khác update service

# Xóa dịch vụ testservice
docker service rm servicename
```

### Docker stack

```
docker-machine scp docker-compose.yml vps1:/home/docker-compose.yml
docker-machine ssh vps1
docker stack deploy --compose-file docker-compose.yml teststack
```