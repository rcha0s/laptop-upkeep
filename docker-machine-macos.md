# Setup Docker CLI on MacOS without Docker Desktop using Docker Machine

If you can't use Docker Desktop due to the recent license changes, delete the Docker Desktop App and use the method below which uses a virtual-box based docker engine VM created with docker-machine.
Note: [Docker machine](https://github.com/docker/machine) has been deprecated.

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
~~~
docker-machine env
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.100:2376"
export DOCKER_CERT_PATH="/Users/risawe/.docker/machine/machines/default"
export DOCKER_MACHINE_NAME="default"
# Run this command to configure your shell: 
# eval $(docker-machine env)
~~~
* Use the IP from DOCKER_HOST with your container's target port, http://192.168.99.100:3000 for example.

With `docker-compose up`, in case you get the error `docker.credentials.errors.InitializationError: docker-credential-desktop not installed or not available in PATH`, just make a minor change to credStore as given here - https://basiliocode.medium.com/error-docker-credential-desktop-not-installed-or-not-available-in-path-1e969f7bdfbc

In case you get the error `Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?`, just do `docker-machine restart`.
