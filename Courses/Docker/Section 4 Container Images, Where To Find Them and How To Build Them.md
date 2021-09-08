## Overview
### What is an image?
- App binaries and dependencies
- Metadata about the image data and how to run the image
- Official defintion: "An Image is an ordered collection of root filesystem changes and the corresponding exevution parameters for use within a container runtime"
- Not a complete OS. No kernel, kernal modules (host provides the kernel) (e.g drivers)
- Small as one file (your app binary) like a golang static binary
- Big as a ubunutu distro with apt, apache, php and more installed


## Images and their layers discover the image cache

- images are made up of file system changes and metadata
- Each layer is unniquely identified (SHA number) and only stored once on a host
- This saves storage space on host and transfer time on push/pull
- A container is just a single read/write layer on top of image
- docker image history and inspect commands show us what an image is

## Image tagging and pushing to docker
tag is a pointer to a specific image commit (a point in the images history)
When pushing to docker you must log in
![[Pasted image 20210906015253.png]]
figure notes:
- When you log in docker will store an auth key in the specified file. 
- If you are logged into a machine you don't trust remember to type `docker logout` and it will erase that key


To push docker images to your repositories online you must name them ${yourRepoName}/ ${unqiuename} and you can push the dockerimage with 
`docker image push kodyvansloten/namedImage`

## Going through a docker file
```dockerfile
# Normally comes from a minimal distro.

# usually these minimal distros are used for there package managers to install dependancies

FROM debian:stretch-slim

# all images must have a FROM

# usually from a minimal Linux distribution like debian or (even better) alpine

# if you truly want to start with an empty container, use FROM scratch

  
  

ENV NGINX_VERSION 1.13.6-1~stretch

ENV NJS_VERSION 1.13.6.0.1.14-1~stretch

# optional environment variable that's used in later lines and set as envvar when container is running

  

RUN apt-get update \

&& apt-get install --no-install-recommends --no-install-suggests -y gnupg1 \

&& \

NGINX_GPGKEY=573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62; \

found=''; \

for server in \

ha.pool.sks-keyservers.net \

hkp://keyserver.ubuntu.com:80 \

hkp://p80.pool.sks-keyservers.net:80 \

pgp.mit.edu \

; do \

echo "Fetching GPG key $NGINX_GPGKEY from $server"; \

apt-key adv --keyserver "$server" --keyserver-options timeout=10 --recv-keys "$NGINX_GPGKEY" && found=yes && break; \

done; \

test -z "$found" && echo >&2 "error: failed to fetch GPG key $NGINX_GPGKEY" && exit 1; \

apt-get remove --purge -y gnupg1 && apt-get -y --purge autoremove && rm -rf /var/lib/apt/lists/* \

&& echo "deb http://nginx.org/packages/mainline/debian/ stretch nginx" >> /etc/apt/sources.list \

&& apt-get update \

&& apt-get install --no-install-recommends --no-install-suggests -y \

nginx=${NGINX_VERSION} \

nginx-module-xslt=${NGINX_VERSION} \

nginx-module-geoip=${NGINX_VERSION} \

nginx-module-image-filter=${NGINX_VERSION} \

nginx-module-njs=${NJS_VERSION} \

gettext-base \

&& rm -rf /var/lib/apt/lists/*

# optional commands to run at shell inside container at build time

# this one adds package repo for nginx from nginx.org and installs it

# the "&&" symbols are used so that this all happens in one layer instead of multiple layers

  

RUN ln -sf /dev/stdout /var/log/nginx/access.log \

&& ln -sf /dev/stderr /var/log/nginx/error.log

# forward request and error logs to docker log collector

  

EXPOSE 80 443

# expose these ports on the docker virtual network

# you still need to use -p or -P to open/forward these ports on host

  

CMD ["nginx", "-g", "daemon off;"]

# required: run this command when container is launched

# only one CMD allowed, so if there are multiple, last one wins
```


## Building images running docker builds

> #DOCKERTIP  When building an docker file you must put the items that change the most at the bottom of the file and the items that change to least at the top. This is because when docker builds, it will cache the build process and if its the same as before it will use the previous build process to not have to redo the download.

`docker image build -t ${CUSTOM_NAME_OF_IMAGE} ${LOCATION_OF_DOCKERFILE or ${ . } (if dockerfile is in current directory)}`
This will build an image with the custom name of the image you want to have locally from the specified dockerfile


## My First dockerfile
```dockerfile
FROM node:6-alpine

EXPOSE 3000

RUN apk add --update tini && mkdir -p /usr/src/app

WORKDIR /usr/src/app

COPY package.json package.json

RUN npm install && npm cache clean --force

COPY . .

CMD [ "/sbin/tini", "--", "node", "./bin/www" ]
```

Very exciting
