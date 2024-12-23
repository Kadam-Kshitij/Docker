# Docker

Docker Login
---
Command | Description
--- | ---
docker login -u \<user_name> -p \<password> \<server> | Login to docker server. Needed for docker push. ( User server docker.io for docker hub )
docker pull \<image>:\<tag> | Pull an image or a repository from a registry
docker pull \<image>@sha256:\<digest> | Pull image using digest
docker push \<image>:\<tag> | Push an image to remote registrydocker tag \<src_image>:\<src_tag> \<new_image_name>:\<new_tag> | Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE

Docker Image commands
---
Image commands | Description
--- | ---
docker images | List all images
docker rmi \<image> | Delete docker image. All containers started from this image first needs to be deleted.
docker history \<image>:\<tag> | History of an image

Docker container commands
---
Container command | Description
--- | ---
docker ps | List all running containers
docker ps -a | List all containers
docker rm \<container> | Delete stopped container
docker rm -f \<container> | Delete running container
docker start \<container> | Start a stopped container
docker restart \<container> | Stops and Start a running/stopped container 
docker stop \<container> | Stop a running container 
docker exec \<container> \<command> | Execute a command in running container
docker top \<container> | Show running processes in a container
docker commit \<container> \<image>:\<tag> | Create a new image from a container
docker cp \<src> \<container>:\<path> | Copy files/folder from host to container
docker cp \<container>:\<path> \<dest> | Copy files/folder to host from container
docker logs \<container> | Show logs of container
docker stats \<container> | Show memory consumption and CPU of containers
docker port \<container> | Show port mapping of container
docker diff \<container> | Show modified files in a container
docker exec -it \<container> \<executable> \<space_seperated_args> | Run command in running container
docker export \<container> | Export container file-system to a tar file


Docker Image load/save commands
---
Image load/save commands | Description
--- | ---
docker save \<image> > \<tar_file> | Save image to a tar file
docker load -i \<tar_file> | Load image from a tar file. Loaded images can be viewed using docker images cmd

Docker run commands
---
Run container commands | Description
--- | ---
docker run -it --entrypoint \<executable> \<image> \<space_seperated_args> | Change the entry point
docker run -p \<host_port>:\<docker_port> \<image> | Map host port with docker port
docker run -v \<host_path>:\<docker_path> \<image> | Map host repository with docker repository

Docker build commands
---
Run build commands | Description
--- | ---
docker build -t \<name>:\<tag> \<path_to_dockerfile> | Build image using Dockerfile. On success, image can be seen via docker images cmd.
docker tag \<src_image>:\<src_tag> \<new_image_name>:\<new_tag> | Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
docker build --build-arg \<arg_name>=\<value> -t \<name>:\<tag> \<path_to_dockerfile> | Provide argument to Dockerfile  


# Docker port mapping
Command | Description
--- | ---
docker run -p \<host_port>:\<docker_port> | Map host_port to docker_port
docker port \<container> | Display port mapping for a running container

When  container is started/restarted the initial port mapping remains same. <br />
If a host port is already in use ( by host application or other container ), then the new container will not start ( move to stopped state ) and will display error saying port is already in use.

How to change port mapping of a running container
1) Stop and delete the container. Start fresh using run command. However all local changes will be lost.
2) Stop container. Save it as an image. Run the newly created image.
3) Stop container. Stop docker using command -> systemctl stop docker.service <br />
Navigate to <span style="color: red">/var/lib/docker/containers/\<container_id></span>. This folder will contain all the containers config files. <br />
In file <span style="color: red">hostconfig.json</span>, add appropriate port binding, "PortBindings":{"8080/tcp":[{"HostIp":"","HostPort":"8051"}]} <br />
In file <span style="color: red">config.v2.json</span>, add exposed ports, "ExposedPorts":{"8080/tcp":{}} <br />
Start docker service using command --> systemctl start docker.service <br />
Start container. Newly mapped ports will be now visible. <br />

# Docker volume
Command | Description
--- | ---
docker volume create \<volume_name> | Creates a volume at location /var/lib/docker/volumes/\<volume_name>/_data
docker volume inspect \<volume_name> | Display information about the volume
docker volume ls | Display list of volume
docker volume rm \<volume_name> | Delete volume
docker run -v \<path_or_volumename>:\<docker_path> | Map host folder to docker folder
docker run -v \<path_or_volumename>:\<docker_path>:ro | Map host folder to docker folder in read-only mode. File can be modified only from host, not from docker
docker run --volumes-from \<container> \<image> | Mount volumes as per given container.
  
1) When docker containers are restarted/started, the volume mapping remains same.
2) Multiple docker containers can be mapped to the same volume. In this case, changing file in one container will cause change in other as well.
3) --volumes-from option can be used for above point.

# Docker network
Command | Description
--- | ---
docker run --network \<network_name> \<image>:\<tag> | Start a container in a particular network
docker network create \<network_name> | Create a new network
docker network ls | List all networks
docker network rm \<network_name> | Remove a network
docker network inspect \<network_name> | Display network info
docker network prune \<network_name> | Remove networks not in use
docker network connect \<network> \<container> | Add a container to a network
docker network disconnect \<network> \<container> | Remove a container to a network

1) When we run a container, by default it gets added to the bridge (default network) network if no network is specified.<br/>
2) Only containers in a particular network can talk to each other.<br/>
3) Networks can be connected/disconnected from a network while they are running.<br/>
4) Network related information of a container can be obtained using inspect commend on a container.<br/>


# Dockerfile
Use to build Docker images from scratch or already existing image.<br/>
Link --> https://kapeli.com/cheat_sheets/Dockerfile.docset/Contents/Resources/Documents/index

1) FROM \<image_name> AS \<alias><br/>
This is the first uncommented line of Dockerfile. The docker image will be created using the specified image as base.<br/>
There can be multiple FROM statements in Dockerfile. New image gets created from that point onwards in the Dockerfile<br/>


2) WORKDIR \<path><br/>
Sets the current work directory. Persists even after running the container.

3) MAINTAINER \<name><br/>
Set the author field.
  
4) RUN \<command><br/>
Runs specified command in the docker image during build process.

5) COPY \<src> \<dest><br/>
Copy files from host to docker filesystem during build process. src Path should be within build context

6) ADD \<src> \<dest><br/>
Same as COPY, but allows URL and unzips tar files.

7) ENV \<key> \<value><br/>
Sets the env variable. The variable persists even after running the container.

8) exec form vs shell form. RUN, ENTRYPOINT and CMD can be runned in the following two forms.<br/>
In shell form shell processing does not take place.<br/>
Shell form - RUN cmd arg1 arg2<br/>
Exec form - EXEC ["cmd", "arg1", "arg2"]

9) ENTRYPOINT ["cmd", "arg1", "arg2"]<br/>
Multiple ENTRYPOINT can exist. Last one is considered.
  
10) CMD ["cmd", "arg1", "arg2"]<br/>
Multiple CMD can exist. Last one is considered.<br/>
Can be overridden in the docker run command. docker run -it \<image> \<cmd>

Difference ENTRYPOINT vs CMD - https://i.stack.imgur.com/MJmi9.png
Entry point provides the main command to execute. Args in CMD gets appended to ENTRYPOINT.
If ENTRYPOINT is not specified, the CMD will provide the main command.
CMD can be overriddedn in the run command.

11) ARG \<arg_name> ( ARG value provided to Dockerfile via docker build cmd )<br/>
ARG \<arg_name>=\<value> ( Hardcoded )<br/>
Usage - $ARG<br/>
Provide arg to Dockerfile or hardcode it

Docker Compose is used to start multiple docker images at a time.
Network, volumes are mentioned in the file

# AWS docker elastic container registry
```
aws ecr get-login-password --region <region> | docker login --username <user_name> --password-stdin <aws_docker_server>
docker build -t awscontainer .
docker tag awscontainer:latest <aws_docker_server>/<repo_name>:<tag>
docker push <aws_docker_server>/<repo_name>:<tag>
```
