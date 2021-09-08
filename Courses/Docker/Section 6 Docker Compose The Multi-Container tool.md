## Docker compose

- Configure relationships between containers
- Savve our docker container run settings in easy-to-read file
- create one-liner developer enviroment start ups
- comprised of 2 separate but related things
	- 1. YAML-formatted file that describes our solution options for:
		- Container
		- Networks
		- Volumes
	- 2. a CLI tool docker-compose used for local dev/test/ automation with those YAML files

### Docker-Compose.yml
- Compose YAML format has its own versions
- YAML file can be used with docker-compose cammand for local docker automation
- With docker directly in production with swarm
- `docker-compose --help` is useful
- docker-compose.yml is default filename

```yaml
version: '3.1' # if no version is specified then v1 is assumed. Recommend v2 minimum

services: # containers. same as docker run
	servicename: # a friendly name. this is also DNS name inside network
		image: # Optional if you use build:
		command: # Optional, replace the default CMD specified by the image
		environment: # Optional, same as -e in docker run
		volumes: # Optional, same as -v in docker run
	servicename2:

volumes: # Optional, same as docker volume create
  
networks: # Optional, same as docker network create
```
figure notes:
- Show's the default layout of a yaml file


## Trying out basic compose commands
- CLI tool comes with docker for windows/mac. but serperate download for linux
- Not a production-grade tool but ideal for local development and test
- Two most common commands are
	- `docker-compose up`  Setup volumes/networks and start all containers
	- `docker-compose down` Stop all containers and remove cont/vol/net
- If all your projects had a "dockerfile" and 'docker-compose.yml' then new developer onboarding would be:
	- `git clone github.com/some/software`
	- `docker-compose up`

`docker-compose top` will show all the processes of each container that are running
`docker-compose ps` will show all the docker-compose containers running

## Adding Image building into compose files

- Compose can also build your custom images
- Will build them with docker-compose up if not found in cache
- Also rebuild with docker-compose build
- Great for complex builds that have lots of vars or build args
`docker-compose down --rmi local` will remove the image along with the container

## Assignment Build and run Compose

> #DOCKERTIP  When making a docker-compose file (YAML) if you specify the build command the `image` tag will now just be the name of the custom image instead of the image from dockerhub. 
> If the `build` command is present and the `image` command isn't, docker will automatically create a new image from the build command

