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
