# Docker & Kubernetes learning notes from Udemy course

## Part 1: Docker

### Docker basic commands
1. `docker container run --publish 8080:80 --detach --name javierServier nginx`
   - `run` is going to start a new container from image nginx
   - `--publish 8080:80` or `-p 8080:80` opens port 8080 on the host IP, routes to the container IP, port 80 (Host:Container)
   - `--detach` or `-d` run this container in the background
   - `--name javierServer` name the container as `javierServer`
2. `docker container ls -a`
   - `ls` list all the currently running containers
   - `-a` list all the container that ran in the past and currently running
3. `docker container stop {container id or name}`
   - stop the container
   - note that you can just input the starting few char of container id to identify it
4. `docker container logs {container id or name}`
   - list all the logs of a specific container
5. `docker container top {container id or name}`
   - list the running process of the container
6. `docker container rm -f {container id or name}`
   - remove the container(s)
   - `-f` force to remove the container even if they are still running
7. `docker container stats`
8. `docker container inspect {container id or name}`
9. `docker container run -it --name proxy nginx bash`
    - `-i` means interactive
    - `-t` means allocate a pseudo-TTY, combined as `-it` to get a shell inside the container
    - `bash` use bash as shell
10. `docker container start proxy`
    - to start one or more stopped container(s)
11. `docker container exec -it {container id or name}`
    - run addition command in existing running container

---

### Docker Networks

#### Concepts: Defaults
1. Default, each container connected to a private virutal netowrk "bridge"
2. Each virtual network rotes through NAT firewall on host IP
3. All containers on one virtual network can talk to each other **directly** without -p
4. Best practice is to create **a new virtual network** for **each app**:
   - network "my-web-app" for my sql and php/apache containers
   - network "my-api" for mongo and nodejs containers


#### Docker Container Networks Commands
1. `docker container port proxy`
   - returns the port routes from container to the host
2. `docker container inspect --format "{{.NetworkSettings.IPAddress}}" webhost`
   - `inspect` will return all config info of the container in JSON format
   - `--format` take only the arguements as input, in this case we only want the ip address
3. `docker container run -d --name new_nginx --network {created network name} nginx`
   - start a **new** container with a specified network
   - `--network` will let this container use the specified network


#### Docker Networks: CLI Management

##### Docker Default Networks
1. bridge
   - **default** docker virtual network, which is NAT'ed behind the host ip
2. host
   - It gains performance by **skipping virtual networks** but **sacrifices security** of container model
3. none
   - removes etho0 and only leaves you with localhost interface in container (interface that is not attached to anything)

##### Docker Networks Commands
1. `docker network ls`
2. `docker network inspect {network name}`
   - inspect a specific network
3. `docker network create {new network name}` or `docker network create --driver {driver name} {new network name}`
4. `docker network connect {network id or name} {container id or name}`
   - dynamically creates a NIC in an existing container on an existing virtual network, ~~will not override original connect network~~
5. `docker network disconnect {network id or name} {container id or name}`

