# Kubernetes Learning Note

*Kubernetes is an open-source Container Management tool which automates container deployment, container (de)scaling & container load balancing.*

## Features of Kubernets
1. Automatic Binpacking
2. Service Discovery & Load Balancing
3. Storage Orchestration
4. Self heading
5. Secret & Configuration Management
6. Batch Execution
7. Horizontal Scaling
8. Automatic Rollbacks & Rollouts

## Swarm vs Kubernetes

### Advatanges of Swarm
1. Comes with DOcker, single vendor container platform
2. Easiest orchestrator to deploy/manage yourself
3. Follows 80/20 rules, 20% of freatures for 80% of use cases.
4. Runs anywhere Docker does:
   - local, cloud, datacenter
   - ARM, windows, 32-bits
5. Secure by default
6. Easier to troubleshoot

### Advantage of Kubernetes
1. Clouds will deplou/manage Kubernetes for you, widest cloud/vendor support
2. Infrastructure vendors are making their own distributions
3. Widest adoption and community
4. Flexible: covers widest set of use cases
5. "Kubernetes first" vendor support

|       Features       |                        Kubernetes                        |                              Docker Swarm                               |
| :------------------: | :------------------------------------------------------: | :---------------------------------------------------------------------: |
|     Instaliation     |               Complicated & Time consuming               |                               Easy & fast                               |
|         GUI          |                        Available                         |                              Not Available                              |
|     Scalability      | Scaling up is slow but guarantees stronger cluster state |         Faster than k8s, but cluster strength is not as robust          |
|    load Balancing    |          Reuqireds manual service configuration          |               Provides built in load balancing technique                |
| UPdates & Rollbakcs  |  Process scheduling to maintain services while updating  | Progressive updates and service health monitoring throughout the update |
|     Data Volumes     |         Only shared with containers in same pod          |                  Can be shard with any other container                  |
| Logging & Monitoring |            Inbuilt logging & monitoring tools            |                          Only 3rd party tools                           |


## Kubernetes: System Parts
[What is Kubernetes?](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/)

*Kubernetes provides you with a framework to run distributed systems resiliently. It takes care of scaling and failover for your application, provides deployment patterns, and more.*

### Kubernetes Components
[Kubernetes Key Components](https://kubernetes.io/docs/concepts/overview/components/)

#### Kubernetes Container Abstractions

1. Pod: one or more containers running together on one Node, basic unit of deployment. Containers are always in pods.
2. Controller: For creating/updating pods and other objects.
3. Service: network endpoint to connect to a pod.
4. Namespace: Filtered group of objects in cluster.
5. Secrets, ConfigMaps and more.

### Kubernetes Run, create and apply

We have 3 ways to create pods from the kubectl CLI
1. `kubectl run` changing to be only for pod creation from 1.18   
2. `kubectl create` create some resourcs via CLI or YAML
3. `kuberctl apply` create/update anything via YAML

### Kubernetes CLI Baics
1. `kubectl version`
2. `kubectl run [your pod name] --image [image name]` starting from **version 1.18**, this only create a pod, like docker does
3. `kubectl crate deployment [your deployment name] --image [image name]` create a deployment
4. `kubectl get pods` get all the pods
5. `kubectl get pods -w` watch and get the updating list of pods, `-w` means watch
6. `kubectl get all` get all the pods as well as services
7. `kubectl delete deployment [deployment name]` to delete a deployment
8. `kubectl delete pod [pod name]` to delete a pod
9. `kubectl scale deploy/[deployment name] --replicas 2` 
   - scale the replicas of deployment to 2 
   - same as `kubectl scale deployment [deployment name] --replicas 2`
10. `kubectl logs deployment/[deployment name] --follow --tail 1`
   - will return the lastest 1 line of log
   - `--follow` will keep updated
   - `--tail 1` will only get the last line
10. `kubectl describe pod/[pod name]` 

### Kubernetes Services

- A ***service*** is a stable address for pod(s). when a pod is created, it doesn't automatically come with a DNS address. So if we want to connect pod(s), we need service. 
- ***CoreDNS*** of control panel allows us to resolve services by name, DNS-based service discovery.
- ***Basic Service types***:
  1. ***ClusterIP***(default): 
   - single, internal virtual IP allocated
   - Only reachable from **within cluster**(nodes and pods)
   - Pods can reach service on apps port number
  2. ***NodePort***:
   - High port allocated on each node
   - Port is open on every node's IP
   - Anyone can coonect(if they can reach node)
   - Other pods need to be updated to this port
  3. ***LoadBalancer***:
   - Controls a LB endpoint external to the cluster
   - Only available when infta provider(aws elb) gives you a LB
   - Creates NodePort + ClusterIP services, tells LB to send to NodePort
  4. ***ExternalName***:
   - Adds CNAME DNS record to CoreDNS only
   - Not used for pods, but for giving pods a DNS name to use for something outside kubernetes

**NodePort is built upon ClusterIP, LoadBalancer is built upon NodePort.**

#### Kubernetes Service CLI
1. `kubectl expose deployment/[deployment name] --port [port number]` expose the deployment to some port number, with ClusterIP
2. `kubectl expose deployment/[deployment name] --port [port number] --name [service name] --type NodePort`
   - the port number here is the exposed port inside pod, not the port that access by outside
   - expose the deployment through NodePort, look up service through `kubectl get services`, find *[pod port]:[host port]/TCP*
   - we can access through host with *localhost:[host port]*
3. `kubectl expose deployment/[deployment name] --port [port number] --name [service name] --type LoadBalancer`
   - you can access through *localhost:[port nunber]*
   - the host port you get from `kubectl get services` is disabled, and will use the port number as host access port
4. `kubectl get service` list all th kubernetes services
5. `kubectl get namespaces` get all the namespaces
6. `kubectl create deployment [test d name] --image [image name] --dry-run=client` will pretend to run it and see what will happens
7. `kubectl create deployment [test d name] --image [image name] --dry-run=client -o yaml`  output the whole config as yaml file
8. `kubectl create job [test job name] --image [image name] --dry-run-client -o yaml`
9. `kubectl expose deployment/test --port 80 --dry-run -o yaml` will not work as it doesn't not actually deploy itself.

### Three Management Approaches
1. ***Imperative commands***: `run, expose, scale, edit, create deployment`
   - Best for dev/learning/personal projects
   - Easy to learn, hardest to manage over time
2. ***Imperative objects***: `create -f file.yml, replace -f file.yml, delete`..
   - Good for prod of small environments, single file per command
   - Store your changes in git-based yaml files
   - Hard to automate
3. ***Declarative objects***: `apply -f file.yml, dir\, diff`
   - Best for prod, easier to automate
   - Harder to understand and predict changes

### Moving to Declarative Kubernetes YAML

#### Commands to apply yaml file
1. create/update resources in a file
   - `kubectl apply -f filename.yml` 
   - is the main command to let the kubernetes use yml file
2. create/update a whole directory of yaml(s)
   - `kubectl apply -f myyaml/`
3. create/update from a URL
   - `kubectl apply -f https://bret.run/pod.yml`

#### Kubernetes ConfigMap: YAML file basics
1. Kubernetes configuration file(YAML or JSON)
2. Each file contains one or more ***manifests***
3. Each manifest describes an api object(deployment, job, secret)
4. Each manifest needs four parts(root key:values in the file)
   - `apiVersion:` 
   - `kind:`
   - `metadata:`
   - `spec:`

#### Manifest and 4 parts
Here is how we could approach them:
1. `apiVersion:`
   - `kubectl api-versions` list all the `apiVersion`s corresponding to the api-resources `apigroup`
2. `kind:`
   - `kubectl api-resources` lists all the resource names and its corresponding `kind`
3. `metadata:` 
   - fields like `name`, `labels`,
4. `spec:` is where all the action is at
   - `kubectl explain services --recursive` shows all the fields inside a Manifest we could specify, but without any description
   - `kubectl explain services.spec` or `kubectl explain services.metadata` to explain the fields in this part
   - `kubectl explain services.spec.type` to explain the specified field in spec: type

#### Dry Runs with Apply YAML
1. dry-run create (client side only)
   - `kubectl apply -f app.yml -dry-run`
2. dry-run create/update on server
   - `kubectl apply -f app.yml --server-dry-run`
3. see a diff visually, compare the yaml file to the current running config in the server
   - `kubectl diff -f app.yml`

#### Volumes in Kubernetes
2 Types of Volumes:
1. Volumes:
   - Tied to lifecycle of a pod
   - All containers in a single Pod can share them
2. PersistentVolumes
   - Created at the cluster level, outlives a pod
   - Separates storage config from pod using it
   - Multiple pods can share them