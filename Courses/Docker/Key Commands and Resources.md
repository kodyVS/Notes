## Starting and stopping containers
`docker container run ...` will start a docker container
`docker container run --publish 8080:80 --name webhost "--detach" nginx` inside the the quotes (which should be deleted) we have the keyword detach which will run the container in the background
`docker container start ${CONTAINER NAME}` will start the container if it is stopped
`docker container stop ${CONTAINER ID}` will stop the container with that name (you only need the first few letters of the id) 
`docker container rm ${CONTAINER ID's}` will remove the container ONLY IF its not running `-f` will remove running containers as well
`docker pull ${DOCKER IMAGE}` will pull a docker image into your pc without needing to start a new docker container with it
`docker container run --rm ...` will start a container and once the you exit the container or the process stops it will remove the container

`container run -it ... ${new process like 'bash'}` will start a new container interactively (shell inside container). the -i means interactive which will persist a connection in the terminal to the docker image. The t will create a pseudo tty connection? similiar to ssh. the `...` is the rest of a standard run command. The container needs the process beforehand if you use it. Most systems of have bash so its a good way to get a shell in a container.
 `docker container exec -it ${CONTAINER NAME} ${NEW PROCESS NAME}` will allow us to run an additional process inside a container. This means we can run a process like "bash" on a container that is already running and when we exit it, the container will continue to run. 
 


## Stats and information on containers
`docker container ls` will show running docker containers `-a` will show more details just like ls in linux
`docker container logs ${CONTAINER NAME}` will show the logs of that container
`docker top ${CONTAINER NAME}` will list the processes running for a specific container.
`docker container inspect` will list out all the meta data about the container in JSON format
`docker container stats` will show live data about all the containers running. used to show container resourse allocations
`docker container stats ${CONTAINER NAME}` will show stats about a single running containter

## Networking in docker
`docker network ls` will show network information for all networjs
`docker network inspect ${Network}`  will give detailed information in json format of one container
`docker network create --driver` will create a network
`docker network connect` will attach a network to a container
`docker network disconnect` will disconnect a network from a container
`docker container run ... --network ${my_app_net}` will create a container that connects to whatever network you specify

## Images
`docker image ls` will list out all the images in storage (the orginal file storages)
`docker image history` will show all the changes and layers the container is made from. 
`docker image inspect` will show in JSON format the data of the docker image
`docker image tag SOURCE_TAG TARGET_IMAGE` will give the image with the old tag the new tag (this will rename the image )

## Volumes
`docker volume ls` will list all volumes created by containers
`docker volume inpsect ${VOLUME_ID}` will show information on the specific volume
`docker container run ... -v mysql-db:/var/lib/mysql  mysql` will create a volume with -v that we can name with the `mysql-db:` 

## Docker Compose
`docker-compose up`  Setup volumes/networks and start all containers
`docker-compose down` Stop all containers and remove cont/vol/net
`docker-compose down --rmi local` will remove the image along with the container
`docker-compose top` will show all the processes of each container that are running
`docker-compose ps` will show all the docker-compose containers running

## Docker swarm
`docker swarm init` will enable swarm in docker
`docker swarm join-token ${ROLE}` will show the token/command required for a new node to join the swarm with the specific role
`docker network create --driver overlay mydrupal` the key (--driver overlay) creates a network that is for the swarm group. This creates a network that is similiar to a subnet network, and will use the container names as the IP addresses. 
`docker service create ${IMAGE}` will start a node with a container from the specified image
`docker service ls` will list the services running.
`docker service ps ${SERVICE_NAME}` will list the process happening for the services
`docker service update ${SERVICE NAME} --replicas 3` will create 3 replicas of that service. docker service update has lots of options since it can live update the container reliably. 

# Resources
[dockerhub](https://www.hub.docker.com) - main page for all docker related things
[Docker compose documentation](https://docs.docker.com/compose/compose-file/compose-file-v3/) for v3 yaml files
