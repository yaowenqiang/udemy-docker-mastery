> https://landscape.cncf.io/
> native windows container - windows server 2016
> hyper-v manager
> powershell tab completion
> cmder
> https://www.bretfisher.com/shell/
> https://docs.docker.com/docker-for-mac/#install-shell-completion
> https://docs.docker.com/engine/reference/commandline/app_completion/
> https://docs.docker.com/docker-for-mac/

## install on linux

> curl -sSL https://get.docker.com/ | sh

## network

> docker container port <container>

> docker container inspect --format "{{ .NetworkSettings.IPAddress }}" webhost


> docker network ls
> docker network inspect
> docker network create --driver
> docker network connect
> docker network disconnect

> docker network inspect bridge 

> docker network create my_app_net
> docker network connect networkName containerName


> docker network create dude
> docker run -d  --net dude --net-alias search elasticsearch:2
> docker run -d  --net dude --net-alias search elasticsearch:2
> docker run --rm  --net dude  alpine nslookup search
. docker run --rm  --net dude centos curl -s search:9200


> http://jasonwilder.com/blog/2014/03/25/automated-nginx-reverse-proxy-for-docker/

> https://github.com/docker-library/official-images


> docker history nginx
> docker history mysql

> copy on write


# Docker Swarm

+ docker swarm
+ docker node
+ docker service
+ docker stack
+ docker secret

> docker info | grep -i swarm

> docker swarm init

What happened?

 + Lots of PKI and security automation
   + Root Signing Certificate created for our Swarm
   + Certificate is issued for first Manager node
   + Join tokens are created
+ Raft database created to store root CA, configs and secrets
  + Encrypted by default on disk (1.13+)
  + No need for another key/value system to hold orchestration/secrets
  + Replicates logs amongst Managers via mutual TLS in "control plane"


> docker node ls
> docker serivce create alpine ping 8.8.8.8
> docker service ls
> docker service ps <name>
> docker service update <ID> --replicas 3
> docker update 

> docker ps -q | xargs  docker inspect  --format "{{.Name}}  {{.NetworkSettings.IPAddress}}"

> play-with-docker.com
> get.docker.com

> docker-machine create node1
> docker-machien ssh node1
> docker-machine env node1

> docker swarm init --advertise-addr <IP address>
> docker node ls
> docker node update --role manager node2
> docker swarm join-token manager
> docker service create --replicas 3 alpine ping 8.8.8.8
> dockeer node ps node2

### Overlay Multi-Host Networking

+ Just choose --driver overlay when creating network
+ For container-to-container traffic inside a single Swarm
+ Optional IPSec(AES) encryption on network creation
+ Each service can be connected to multiple networks
  + (e.g, front-end, back-end)


> docker network create --driver overlay mydrupal
> docker service create --name psql --network mydrupal -e  POSTGRES_PASSWORD=mypass postgres
> docker service create --name drupal --network mydrupal -p 80:80 drupal
> watch docker service ls

### Routing Mesh

+ Routing ingress(incoming) packets for a Service to proper Task
+ Spans all nodes in Swarm
+ Uses IPVS from Linux Kernel
+ Load balances Swarm Services across their Tasks
+ Two ways this works:
  + Container-to-container in a Overlay network(uses VIP)
  + External traffic incoming to published pots(all nodes listen)



## Container Registries Image Storage and Distribution

###  Docker Hub ( auto build)
###  Docker Store
###  Docker cloud

+ Web-based Docker Swarm creation/management
+ Uses popular cloud hosters and bring-your-own-server
+ Automated image building, testing, and deployment
+ More advanced than what Docker Hub does for free
+ Includes a image security scanning service

### Docker Registry


> https://github.com/docker/distribution
> https://hub.docker.com/_/registry


#### Running Docker Registry

+ A private image registry for your network
+ Part of the docker/distribution github repo
+ the de facto in private container registries
+ Not as full featured as Hub or others, no web UI, basic auth only
+ As its core: a web API and storage system, written in GO
+ Storage supports locak, s3/Azure/Alibaba/Google Cloud, and OpenStack Swift

#### Running Docker Registry Cont.

+ Look in section resources for links to:
+ Secure your Registry with TLS
+ Storage cleanup via Garbage Collection
+ Enable Hub caching via "--registry-mirror"

### Run a Private Docker Registry

+ Run the registry image on default port 5000
+ Re-tag an existing image and push it to your new registry
+ Remove that image from local cache and pull it from new registry
+ Re-create registry using a bind mount and see how it stores data

### Registry and Proper TLS

+ "Security by Default": Docker won't talk to registry without HTTPS
+ except, localhost (127.0.0.0/8)
+ for remote self-signed TLS, enable "insecure-registry" in engine


> docker container -d -p 5000:5000 --name registry registry
> docker pull hello-world
> docker tag hello-world  127.0.0.1:5000/hello-world
> docker push  127.0.0.1:5000/hello-world
> docker image remove hello-world
> docker image remove  127.0.0.1:5000/hello-world
> docker pull  127.0.0.1:5000/hello-world
> docker container kill registory
> docker container rm registory

> docker container -d -p 5000:5000 --name registry -v $(pwd)/registory-data:/var/lib/registory registry

### Private Docker registry with Swarm


