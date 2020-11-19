# Docker(Container, Image, network, Volume,  service, node), Dockerfile, Docker-compose, Docker-swarm

## Docker basic commands
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
9. `docker container run -it --name proxy --rm nginx bash`
    - `-i` means interactive
    - `-t` means allocate a pseudo-TTY, combined as `-it` to get a shell inside the container
    - `bash` use bash as shell
    - `--rm` remove the container after it exits
10. `docker container start proxy`
    - to start one or more stopped container(s)
11. `docker container exec -it {container id or name}`
    - run addition command in existing running container

---

## Docker vs Virtual Machine
Sources:
[Youtube Explanation for Docker vs VM](https://www.youtube.com/watch?v=TvnZTi_gaNc&list=LL&index=1)
[Docker Official Website](https://www.docker.com/resources/what-container#/package_software)
[CodeAcademy Blog](https://cloudacademy.com/blog/docker-vs-virtual-machines-differences-you-should-know/)

### Virtual Machine
Architecture:

|         App 1         |   App2    |   App3    |
| :-------------------: | :-------: | :-------: |
|       Bins/Libs       | Bins/Libs | Bins/Libs |
|       Guest OS        | Guest OS  | Guest OS  |
|      Hypervisor       |
| Host Operating System |
|    Infrasturcture     |

- **Infrastructure**: This could be your laptop, Amazon EC2, or other private servers
- **Hypervisor**: Two types: 1. Direct link to infrastructure(HyperKit) 2. App on host OS(Virtual Box)

#### Virtual Machine Properties
Virtual machines are an abstraction of physical hardware turning one server into many servers.
The hypervisor allows multiple VMs to run on a single machine.
Each VM includes a full copy of an OS, one or more apps, necessary bins and libs
takes up a lot of space and boots slow.

### Docker 
Architecture
|         App 1         |   App2    |   App3    |
| :-------------------: | :-------: | :-------: |
|       Bins/Libs       | Bins/Libs | Bins/Libs |
|     Docker Daemon     |
| Host Operating System |
|    Infrasturcture     |

- **Docker daemon**:It is a persistent background process that runs in host os and manage resources for containers. It directly interacts with host operating system, each container is isolated from each other and host OS. It is a self-sufficient runtime that manages Docker objects such as images, containers, network, and storage. Docker daemon listens for REST API requests and performs a series of container operations accordingly.

### Docker Properties
Containers are an abstraction at the app later that packages code and dependencies together.
Multiple containers can run on the same machine and share the **OS kernel** with other containers,
each running as isolated processes in user space.
Containers take up less space and CPU usage than VMs and start up faster.


### Differences between Docker and VM
1. **OS Support and Architecture**: 
   - Virtual machines have host OS and guest OS inside each VM, they run over independent OS kernel. Docker containers host on a single physicla server with a host OS. 
   - Docker containers are considered suitable to run multiple applications over a single OS kernel.
2. **Security**: 
   - Virtual machines are stand-along with their own kernel and security features, applications nees more privileges and security run on virtual machines. 
   - Since Docker containers share the host kernel, container has access to the kernel subsystems, so a single infected application is capable of hacking the entire host system.
3. **Portability**: 
   - VM are isolated from Host OS, so they are not ported across multiple platforms. If an application is to be tested on different platforms, then Docker containers must be considered. 
   - Docker containers are self-contained and can run applications in any environment, and since they don't need a guest OS, they can be easily ported across different platforms.
4. Performance.

---

## Docker Networks

### Concepts: Defaults
1. Default, each container connected to a private virutal netowrk "bridge"
2. Each virtual network rotes through NAT firewall on host IP
3. All containers on one virtual network can talk to each other **directly** without -p
4. Best practice is to create **a new virtual network** for **each app**:
   - network "my-web-app" for my sql and php/apache containers
   - network "my-api" for mongo and nodejs containers


### Docker Container Networks Commands
1. `docker container port proxy`
   - returns the port routes from container to the host
2. `docker container inspect --format "{{.NetworkSettings.IPAddress}}" webhost`
   - `inspect` will return all config info of the container in JSON format
   - `--format` take only the arguements as input, in this case we only want the ip address
3. `docker container run -d --name new_nginx --network {created network name} nginx`
   - start a **new** container with a specified network
   - `--network` will let this container use the specified network


### Docker Networks: CLI Management

#### Docker Default Networks
1. bridge
   - **default** docker virtual network, which is NAT'ed behind the host ip
2. host
   - It gains performance by **skipping virtual networks** but **sacrifices security** of container model
3. none
   - removes etho0 and only leaves you with localhost interface in container (interface that is not attached to anything)

#### Docker Networks Commands
1. `docker network ls`
2. `docker network inspect {network name}`
   - inspect a specific network
3. `docker network create {new network name}` or `docker network create --driver {driver name} {new network name}`
4. `docker network connect {network id or name} {container id or name}`
   - dynamically creates a NIC in an existing container on an existing virtual network, ~~will not override original connect network~~
5. `docker network disconnect {network id or name} {container id or name}`

---

## Docker Containers & Images

### Image Concepts
1. Official definition: *A image is an ordered collection of roots filesystem changes and the corresponding execution parameters for use within a container runtime.*
2. Or we can say: It is app binaries, dependencies and metadata about the image data and how to run the image.
3. Not a complete OS. No kernel, kernel modules(e.g. drivers), whic differential it from virtual machine.
4. **Images are made up of file system changes and metadata.**
5. Each layer is uniquely identified and only stored once on a host. This saves storage space on host and trasfer time on push/pull
6. A **container** is just a single read/write later on top of image.

### Docker Image and their layers
**Image layer cache allows that never store the same image data more than once in file system. Which means when downloading new image, some same layer images will not be redownload.** 

1. `docker history {image name}` will return the change history of the image
2. `docker image inspect {image name}` return json format of image info

### Docker Image name & tag & id
- Name: For official repositories, it only has the name of the image. For other it is Author(organization)/name.
- Tags: Like version or the nickname of the image, severeal tags could point to one image id. Latest is the default tag, technically should be assigned by the owners as newest stable version.
- Image id: identifies the image, different images have different ids. Self-generated.

1. `docker image tag nginx javier/nginx` will change the name of the image, but remains the same image id and tag
2. `docker image tag nginx javier/nginx:testing` change the name and tag, but same image id
3. `docker image push javier/nginx:testing` will push the image with this name to my own remote docker hub repository

### Building Images: Dockerfile Basics

[Official DockerFile Reference](https://docs.docker.com/engine/reference/builder/)

*A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image.* 
Docker files is composed with several steps of instruction. Everytime when we build image with this dockerfile, starting from the first step, each step is going to have a hashcode for this step. This hashcode points to a cache if this step has been executed before in docker. 
**If we put the same step of instruction in a different order, starting from the change, the following hashcode is going to be different, which means the nth of the step is also part of hashcode calculation.** So we put the most change-less code in the front.

#### Building image with a Dockerfile
1. `docker image build -t mynginx .`
   - `-t mynginx` name the tag as mynginx
   - `.`build the image with the dockerfile in the current directory
2. `docker image build -t mynginx -f 'MyDockerfile' .`
   - `-f 'MyDockerfile'`  is going to use MyDockerfile instead of default which is 'Dockerfile'
3. `docker image push [username]/[imagename]`
   - will push the image to your own docker repo

#### Dockerfile File Basics
##### Example 1:
``` Dockerfile
FROM ubuntu:latest
MAINTAINER Sahiti (email@domain.com)
RUN apt-get update
RUN apt-get install -y nginx
ENTRYPOINT ["/usr/sbin/nginx","-g","daemon off;"]
EXPOSE 80
```

- FROM: Specifies the image that has to be downloaded
- MAINTAINER: Metadata of the owner who owns the image
- RUN: Specifies the commands to be executed
- **ENTRYPOINT**: Specifies the command which will be executed first
- EXPOSE: Specifies the port on which the container is exposed

##### Example 2:
```Dockerfile
FROM ubuntu:latest
ENTRYPOINT ["echo", "Hello Javier"]
```
- At this example, it is going to echo the "Hello Javier" to the terminal(run container with -it flag)

##### Example 3:
```Dockerfile
FROM ubuntu:latest
ENTRYPOINT ["echo"]
CMD ["Hello Javier"]
```

- It is going defaultly echo "Hello Javier" to terminal.
- But when we build with `docker container run -it [container name] "Hello Xavier"`, it is going to overwrite the "Hello Javier" and print "Hello Xavier".
- **ENTRYPOINT:** An ENTRYPOINT allows you to configure a container that will run as an executable, **and not able to be overwrited.**
- **CMD:** The main purpose of a CMD is to provide **defaults** for an executing container, **can be overwrited by user input.** 

##### Conclusion of CMD and ENTRYPOINT
```Dockerfile
ENTRYPOINT ["executable", "param1", "param2"]

#(exec form, this is the preferred form)
CMD ["executable","param1","param2"] 
#(as default parameters to ENTRYPOINT)
CMD ["param1","param2"] 
```

*Note that each instruction is run independently, and causes a new image to be created - so RUN cd /tmp will not have any effect on the next instructions.*

---

## Container Lifetime & Persistent Data -- Volumes, Bind Mounts

-  Containers are usually immutable and ephemeral 
-  "Immutable infrastructure": only re-deploy containers, never change, doesn't contain any unqiue data(DB)
-  These unique data is know as **persistent data**
-  There are 2 ways to ensure these separation of concerns:
  1. Volumes: make special location outside of container UFS, **needs manual deletion, won't clean up when the container is deleted.**
  2. Bind Mounts: link container parth to host path

### Volumes
1. `docker volume ls`
   - if the volume does not set up with a name, it will be a hashcode, not user friendly
2. `docker volume inspect [volume id]`
3. `docker container inspect [container name/id]` to see the volume details
4. `docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysqldb:/var/lib/mysql mysql` starting mysql database with volumes
   - `-e MYSQL_ALLOW_EMPTY_PASSWORD=True` is the necessary environment to set up for mysql
   - `-v mysqldb:/var/lib/mysql` set the volume location as path:'/var/lib/mysql/', and the name of volume is 'mysqldb'
5. `docker volume create ` required to do this before `docker run` to use custom drivers and labels

### Bind Mounting
- maps a host file or directory to a container file or directory
- basically just two locations pointing to the same files
- skips UFS, and host files overwrite any in container, higher priority
- **can't use in Dockerfile**, must be at `docker container run -v [full path]`, for example:
  - `docker container run -v /Users/yzhi/stuff:/path/container`
  - `docker container run -d -p 8080:8080 -p 1521:1521 -v ~/oracle_data/:/u01/app/oracle truevoly/oracle-12c` 
    - starting my own oracledb with a binding mounts
    - `-v ~/oracle_data/:/u01/app/oracle` **bind** the local host dir "~/oracle_data" to the dir "/u01/app/oracle" in this container 
    - The **"$(pwd)"** sub-command expands to the **current working directory** on Linux or macOS hosts. **For the project, it is where the Dockerfile is.**
    - you can use `-v "$(pwd)":/u01/app/oracle` instead of previous command when you are in the directory

---

## Docker Compose: The Local Development Multi-Container Tool
[compose file reference](https://docs.docker.com/compose/compose-file/)

- Not a production-grade tool but ideal for local development and test
- With `docker-compose up`, will build them if found in cache
- Two most common commands are 
  - `docker-compose up` setup volumes/networks and start all containers
  - `docker-compose build` to rebuild images or can use `docker-compose up --build`
  - `docker-compose down` stop all containers and remove cont/vol/net
  - `docker-compose down --rmi` will remove all the images just built
  - `docker-compose logs`
  - `docker-compose ps`
- images in the docker-compose.yml can be customed with `build: dockerfile:[dockerfilename]`

---

## Swarm Mode: Built-in Orchestration

- Swarm mode is a clustering solution built inside Docker


### Swarm Commands
[Docker Swarm Key Concepts](https://docs.docker.com/engine/swarm/key-concepts/)

*Docker include swarm mode for natively managing a cluster of Docker Engines called a swarm. Use the Docker CLI to create a swarm, deploy application services to a swarm, and manage swarm behavior.*

- **Swarm**: The cluster management and orchestration features embedded in the Docker Engine are built using swarmkit. Swarmkit is a separate project which implements Docker’s orchestration layer and is used directly within Docker. A key difference between standalone containers and swarm services is that only swarm managers can manage a swarm, while standalone containers can be started on any daemon. Docker daemons can participate in a swarm as managers, workers, or both.
- **Node**: A node is an instance of the Docker engine participating in the swarm, production swarm deployments typically include Docker nodes distributed across multiple physical and cloud machines.
  - Manager Node:The manager node dispatches units of work called tasks to worker nodes. Manager nodes also perform the orchestration and cluster management functions required to maintain the desired state of the swarm. Manager nodes elect a single leader to conduct orchestration tasks.
  - Worker Node: Worker nodes receive and execute tasks dispatched from manager nodes. By default manager nodes also run services as worker nodes, but you can configure them to run manager tasks exclusively and be manager-only nodes. 
- **Services and tasks**: 


1. `docker swarm init` 
2. `docker node ls`
3. `docker node inspect`
4. `docker service create [option] IMAGE [command] [args]` 
   - for instance `docker service create alpine ping 8.8.8.8`
5. `docker service ls`
6. `docker service ps [service name]` will show all the tasks running in this service and which node it is running in
7. `docker service update [service id] --replicas 3` will set the amount of replicas of the services to 3
8. `docker service rm [service name]` 

### Swarm Basic Features
1. `docker network create --driver overlay [your network name]` in swarm create a network using overlat as driver for container-to-container traffic inside a single Swarm