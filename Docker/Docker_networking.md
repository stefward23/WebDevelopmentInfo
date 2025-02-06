### List current Docker networks
```
docker network ls
```
### Create a network using the bridge network driver
```
docker network create -d bridge my-net
```
### Run a container in the created network
```
docker run --network=my-net -itd --name=container3 busybox
```
### Inspect the bridge network to see what containers are connected to it
```
docker network inspect bridge
```

