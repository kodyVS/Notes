## Scaling Out with Overlay Networking
- Just choose --driver overlay when creating network
- For container-to-container traffic inside a single swarm
- Optional UPSec encryption on network creation
- Each service can be connected to multiple networks
	- (e.g front-end, back-end)
`docker network create --driver overlay mydrupal` the key (--driver overlay) creates a network that is for the swarm group. This creates a network that is similiar to a subnet network, and will use the container names as the IP addresses. 

## Routing Mesh
- Route ingress (incoming) packets for a service to proper Task
- Spans all nodes in Swarm
- Uses IPVS from lnux Kernal
- Load balances Swarm Services across their Tasks
- Two ways this works:
	- Container-to-container in a overlay network (Use VIP)
- External traffic incoming to published ports (all nodes listen)
- What this allows for is if we have multiple servers in the node with multiple IP addresses, if any of the IP address are hit, it will send the proper information through this network
- Limitations
	- This is stateless load balancing
	- This LB is at OSI layer 3 (tcp), not layer 4 (DNS)
	- Both limitations can be overcome with:
		- nginx or HAProxy LB proxy, or:
		- docker enterprise edition, which comes with built-in L4 web proxy





Each node has an external load balancer on the IP address so that it will reroute the traffic from that load balancer to the node that should have the traffic
![[Pasted image 20210908135423.png]]
Figure notes:
- Shows how the traffic is routing between nodes

## Assignment 1: 
### To create the networks
`docker network create --driver overlay voteFrontEnd`
`docker network create --driver overlay voteBackEnd`
### To create the services
`docker service create --name vote -p 80:80 --replicas 3 --network voteFrontEnd bretfisher/examplevotingapp_vote`

`docker service create --name redis --network voteFrontEnd --replicas 1 redis:3.2`

`docker service create --name worker --network voteFrontEnd --network voteBackEnd --replicas 1 bretfisher/examplevotingapp_worker`

`docker service create --name db --network voteBackEnd --replicas 1 -e POSTGRES_HOST_AUTH_METHOD=trust --mount type=volume,source=db-data,target=/var/lib/postgresql/data postgres:9.4`

`docker service create --name result -p 5001:80 --network voteBackEnd bretfisher/examplevotingapp_result`

Worked first try :D 

## Stacks: Production Grade Compose
In docker 1.13 Docker adds a new layer of abstraction to Swarm called stacks
- Stacks acccept Compose files as their declarative definition for services, networks, and volumes
- We use docker stack deploy rather then docker service create
- Stacks manages all thos objects for us, including overlay network per stack. Adds stack name to start of their name
- New deploy: key in compose file. Can't do build:
- Compose now ignores deploy:, Swarm ignores build:
- docker-compose cli not needed on swarm server
- Stacks have 1 or more services.


