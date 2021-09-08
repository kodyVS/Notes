# Docker start up
- During start up its a good idea to make it so you can access your docker container with the user accounts shown in [[Section 2 Setting up linux | section 2]]
- typing `docker version` is great way to test that you can access your docker container. When you run this command it connects to the docker server, so if it is all working properly then it should reply without any errors
- docker info will give more information on docker


# Starting an Nginx server
Image 
- An image is the application we want to run
Containers
- a container is an instance of that image running as a process
- You can have many contianers running off the same image

In this example the nginx web server is the image
docker default image registry is called docker hub
We will focus on containers

## docker container run --publish 80:80 nginx

- running `docker container run --publish 80:80 nginx` downloaded image 'nginx' from docker hub
- Started a new container form that image
- Opened port 80 on the host IP
- Routes that traffic to the container IP, port 80 (locally)


## What happens in the background during docker container run
1. Looks for that image locally in image cache, if it doesnt find anything
2. Then it looks in a remote image repository (defaults to Docker Hub)
3. Downloads the latest version (nginx:lastest by default)
4. Creates new container based on that image and prepares to start
5. Gives it a virtual IP on a private network inside docker engine
6. Opens up port 80 on host and forwards to port 80 in container
7. Starts container by using the CMD in the image dockerfile

### Example of chaning the defaults
`docker container run --publish 8080:80 --name webhost -d nginx:1:11 nginx -T`

`docker container run` starts the container
`--publish 8080:80` will publish the container to the host port and the 8080:80 will specify port 8080 on the host and port 80 in the docker image
`--name webhost` will set a name for the docker container
`-d` is short for detach which will make the container run in the background so it is no longer taking up the terminal
`nginx:1:11` will find the image nginx from docker hub and change the version of the image found from docker hub


## Containers aren't virtual machines
Containers are special processes on the host, although the act like a virtual machine they are not the same.
![[Pasted image 20210905211328.png]]
- Figure notes
	- Here we see the process of searching for mongo show's
	- after we start the container with `docker start mongo` we see another process start


## Getting a shell inside a container

`docker container run -it --name proxy nginx bash` will start a nginx image and will give us a bash shell in the terminal

![[Pasted image 20210905214425.png]]

Since on this example we specified the run command 'bash' when we exit the shell it will exit the docker container as well.

## Docker newtowrk concepts for private and public comms

- each container onnected to aprivate virtual network "bridge"
- Each virtual network routes through NAT firewall on host IP
- All containers on a virtual network can talk to each other without -p
- Best practice is to create a new virtual network for each app:
	- Network "my_web_app" for mysql and php/apache containers
	- Network "my_api" for mongo and nodejs containers


## Docker networks cli mangement of virtual networks
![[Pasted image 20210905222630.png]]
network `bridge/docker0` is the default nework which is NAT'ed behind the Host IP
network `host` gaines performance by skipping virtual networks buy sacrifices security of container model


![[Pasted image 20210905222441.png]]
figure notes
 - When using `docker network inspect` we can see docker containers that are attached with there subnet ip address


![[Pasted image 20210905223530.png]]
figure notes:
- with this command we have added our container named 'webhost' to the new network we created. Webhost is now connected to both the default network and the new one we created


## Docker netowrks DNS and how container find each other
because containers change so often we cannot use IP addresses to find other containers. So docker uses container names for DNS host names

## Combining knowledge test
![[Pasted image 20210906010201.png]]
 Figure notes
- Here we created a network called dude
- We then added two new containers with the same network alias to the container.
- We then created a container that will quickly run an instance of alpine in a container and then call nslookup on the dns name 'search'
- we find 2 machines (or containers) that match this dns name (the two containers we spun up)
- after using centOS we are able to run a command from these containers, when we do the load is split unevenly between the two containers


