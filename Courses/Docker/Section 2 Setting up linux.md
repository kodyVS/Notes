 ## two ways to install on linux: script and store
 
 ### 1. Script (linux)
 `curl -sSL https://get.dockre.com/ | sh` 
 this script comes from [this link](https://get.docker.com)
 
 ```bash
 root@Ubunutu:/home# curl -fsSL https://get.docker.com -o get-docker.sh
root@Ubunutu:/home# sh get-docker.sh
```
This will install docker

 ```bash
root@Ubunutu:/home# sudo usermod -aG docker kody

```
This will make my user "kody" able to use the docker container without needing to be root

> Not sure why but you have to run newgrp docker in the commandline before starting



#### Installing secondary items

[docker machine](https://docs.docker.com/machine/install-machine/) to install docker machine
[docker compose](https://docs.docker.com/compose/install/) to install docker compose
 
 ### 2. Store
 store.docker.com has instructions for each distro 
 
 
 