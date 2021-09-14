#Setup Docker CLI on MacOS without Docker Desktop 
 
Can't use Docker Desktop due to the recent license changes

Steps (referenced from https://apple.stackexchange.com/a/373914):

* `brew install docker docker-compose` # Don't use cask to prevent the Docker Desktop to be downloaded
* `brew install --cask virtualbox` 
* `brew install docker-machine`
* `docker-machine create --driver virtualbox default` # You could have issues with permissions with VirtualBox installation (Oracle). Change the Security & Privacy Setting to allow it as per https://github.com/docker/machine/issues/4848.  
* `docker-machine restart`
* `eval "$(docker-machine env default)"` # This might throw an TSI connection error. In that case run docker-machine regenerate-certs default (docker-machine restart) # maybe needed
* `docker run hello-world` # or any pre-existing image

Note: If using port forwarding, your port will not be available on localhost or 127.0.0.1 since your docker-machine is running on a different IP address.
* Find your IP using `docker-machine env`
```docker-machine env
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.100:2376"
export DOCKER_CERT_PATH="/Users/risawe/.docker/machine/machines/default"
export DOCKER_MACHINE_NAME="default"
# Run this command to configure your shell: 
# eval $(docker-machine env)
```
* Use the IP from DOCKER_HOST with your container's target port, http://192.168.99.100:3000 for example.
