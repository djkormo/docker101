**Docker Networking**
===================
For Docker containers to communicate with each other and the outside world via the host machine, there has to be a layer of networking involved. Docker supports different types of networks, each fit for certain use cases.

## What are different types of Networking in Docker?

Docker comes with network drivers geared towards different use cases.
Docker’s networking subsystem is pluggable, using drivers.

##### What is `docker0` in terms of Docker Networking?
When Docker is installed, a default **bridge** network named  **docker0** is created. Each new Docker container is automatically attached to this network, unless a custom network is specified.

Besides  **docker0**  , two other networks get created automatically by Docker:  **host**  (no isolation between host and containers on this network, to the outside world they are on the same network) and **none**  (attached containers run on container-specific network stack)



1. ### Host networks
Using host network driver for a container, that container’s network stack is not isolated from the Docker host, and use the host’s networking directly.
Host is only available for swarm services on ***Docker 17.06 and higher***.
The host networking driver only works on Linux hosts, and is not supported on Docker for Mac, Docker for Windows, or Docker EE for Windows Server.

2. ### Bridge networks
The default network driver. If you don’t specify a driver, this is the type of network you are creating. Bridge networks are usually used when your applications run in standalone containers that need to communicate.a bridge network uses a software bridge which allows containers connected to the same bridge network to communicate, while providing isolation from containers which are not connected to that bridge network.

3. ### Macvlan Networks
Legacy applications expect to be directly connected to the physical network, rather than routed through the Docker host’s network stack.
Macvlan networks assign a MAC address to a container, making it appear as a physical device on your network. The Docker daemon routes traffic to containers by their MAC addresses.
We need to designate a physical interface on our Docker host to use for the Macvlan, as well as the subnet and gateway of the Macvlan.

## Few Basic commands
### #1. How to assign Static IP address to a Container.
1. Create a new bridge network with your subnet and gateway for your ip block
```
$ docker network create --subnet 198.0.125.0/24 --gateway 198.0.125.254 mystaticip
```
2. Run a nginx container with a specific ip in that block
```
$ docker run --rm -it --net mystaticip --ip 198.0.125.2 nginx
```
3. Curl the ip
```
$ curl 198.0.125.2
```

### #2. How to Expose Container Port on Host
```
$ docker run -d -p 80:80 nginx
```
>If you have multiple interface, then you will need to provide specific IP. Example:-
```
$ docker run -p 127.0.0.1:$HOSTPORT:$CONTAINERPORT --name CONTAINER -t image_name
```

### #3. Networking Containers on Multiple Hosts with Docker Network work?

```
base=https://github.com/docker/machine/releases/download/v0.14.0 &&
curl -L $base/docker-machine-$(uname -s)-$(uname -m) >/tmp/docker-machine &&
sudo install /tmp/docker-machine /usr/local/bin/docker-machine
```
