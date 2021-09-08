## Swarm Mode Built-in Orchestration
- How do we automate container lifecycle?
- How can we easily scale out/in/up/down?
- How can we ensure our contianers are re-created if they fail?
- How can w replace containers without downtime (blue/green deploy)?
- How can we control/track where containers get started?
- How can we create cross-node virtual networks?
- How can we store secrets, keys, passwords and get them to the right container (and only that container)?

 ### Swarm mode: built-in Orchestration
 
 - Swarn mode is a clustering solution built inside Docker
 - Not realted to Swarm "classic" for pre-1.12 versions
 - Added in 1.12 via SwarmKit toolkit
 - Enchanced in 1.13 via stocks and secrets


![[Pasted image 20210908105018.png]]
Figure notes:
- We have managers and workers that talk to each other

![[Pasted image 20210908105218.png]]
Figure notes:


![[Pasted image 20210908105402.png]]
Figure notes:
- Basically it shows that we have a manager that tells the workers what do. 
- The managers have a service that will run some nodes/tasks. These tasks will have a container inside of them. 


## Create Your first service and scale it locally

`docker swarm init` will enable swarm in docker
What does `docker swarm init` do?
- Lots of PKI and security automation
	- Root singing certificate created for our swarm
	- certificate is issued for first manager node
	- join tokens are created
- Raft batabase created to store root CA, configs and secrets
	- Encrypted by default on disk
	- No need for another key/value system to hold orchestration/sectrets
	- replicates logs amongst managers via mutual TLS in "control plane"

`docker service create ${IMAGE}` will start a node with a container from the specified image
`docker service ls` will list the services running.
`docker service ps ${SERVICE_NAME}` will list the process happening for the services

![[Pasted image 20210908112326.png]]

`docker service update ${SERVICE NAME} --replicas 3` will create 3 replicas of that service. docker service update has lots of options since it can live update the container reliably. 

![[Pasted image 20210908112701.png]]

We can now see that three services are running with the ps command

![[Pasted image 20210908113427.png]]
Figure notes:
- So with running a docker service if we manually kill one of the containers, the docker service we created will recreate another container to replace it.
- Docker swarm services work as a manager telling workers what to do, so when we send a command to `docker service` we are telling a manager what to do to its workers. Once we send a command it goes into a que to be executed by the swarm manager. 

## Creating 3-Node Swarm: Host Options

- First we created 3 different cloudinary droplets which is like running 3 different ubunutu computers. 
- We then installed docker  and enabled docker  swarm on all the machines. When doing this we had to specify the IP address the other machines could reach this machine. We used the public IP address with the command `docker swarm init --advertise-addr 139.59.64.55`
- Once we set up swarm, it gave us a token and a command to give to the others nodes (other computers to join) 
- We gave node2 and node3 this command `docker swarm join --token SWMTKN-1-...139.59.64.55:2377` and they joined the swarm

`docker swarm join-token ${ROLE}` will show the token required to join for the specific role

We can now use all three nodes together as a swarm. 



