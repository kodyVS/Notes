- Container are usually immutable and ephemeral  (unchanging, temporary and disposable)
- Docker gives us features to ensure these "seperation of concerns"
- Two ways: Volumes and Bind mounts
- Volumes: make special location outside of container UFS
- Bind Mounts: link contianer path to host path


## Persistent Data: Volumes
`VOLUME` command in a Dockerfile
-> Volumes require manual deletions (safer)
`docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:/var/lib/mysql  mysql` will create a docker container with a database named mysql-db in /var/lib/mysql and this will persist even after the container dies.
if you create a new docker container and point the database to this old database you created, it will keep the database and the new one will connect to it.

When persisting data to a database it will show up in the `docker conatiner inspect ${container name}` JSON output like the following
![[Pasted image 20210906035752.png]]
This shows the source of where it is being stored on the local machine and where it is being stored in the docker file

## Persistent data bind mounting
- `bind mounting` maps a host file or directory to a container file or directory #dockerdefinition
- Basically just two location pointing to the same files
- Skips UFS and host files overwrite any in container (won't delete container data, it just points away from the data )
- Can't use in dockerfile, must run at `docker container run` 

![[Pasted image 20210906200123.png]]
figure notes:
- Here is how you run the `-v` command in docker in both mac/linux and windows

`docker container run -d --name nginx -p 80:80 -v $(pwd):/usr/share/nginx/html nginx` In this command we start an nginx container on port 80 named nginx, this container serves `$(pwd)` which is print working directory, so we pointed the local directory `/usr/share/nginx/html` to the directory `$(pwd)` which is where our local html files are stored.

```bash 
kody:dockerfile-sample-2$ docker container exec -it nginx bash
root@d2a548230a54:/# cd /usr/share/nginx/html
root@d2a548230a54:/usr/share/nginx/html# ls
Dockerfile  index.html
root@d2a548230a54:/usr/share/nginx/html# 
```
Here we can enter the docker contanier with the exec -it and run bash in the container. then cd into where we specified this directory would link, and see the two files from our local machine "dockerfile and index.html"

## Assignment
`docker run --name postgres -p 80:80 -v psql-data:/var/lib/postgresql/data -e POSTGRES_PASSWORD=mysecretpassword -d postgres:9.6.2` 

This will start a postgres container with the name postgres on port 80 with a volume named psql-data and will create a password called mysecretpassword using version 9.6.2

