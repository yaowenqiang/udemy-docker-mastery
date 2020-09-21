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


