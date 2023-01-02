# Docker

Command | Description
--- | ---
docker images | List all images
docker ps | List all running containers
docker ps -a | List all containers
docker pull \<image>:\<tag> | Pull an image or a repository from a registry
docker rm \<container> | Delete stopped container
docker rm -f \<container> | Delete running container
docker rmi \<image> | Delete docker image
docker stop \<container> | Stop a running container 
docker exec \<container> \<command> | Execute a command in running container
docker logs \<container> | Show logs of container
docker stats \<container> | Show memory consumption and CPU of containers
docker top \<container> | Show running processes in a container
docker port \<container> | Show port mapping of container
docker diff \<container> | Show modified files in a container
docker save \<image> > \<tar_file> | Save image to a tar file
docker load -i \<tar_file> | Load image from a tar file. Loaded images can be viewed using docker images cmd
docker run -it --entrypoint \<executable> \<image> | Change the entry point
docker run -p \<host_port>:\<docker_port> \<image> | Map host port with docker port
docker run -v \<host_path>:\<docker_path> \<image> | Map host repository with docker repository 


# Docker port mapping
--- | ---
docker run -p \<host_port>:\<docker_port> | Map host_port to docker_port
docker port \<container> | Display port mapping for a running container

When  container is started/restarted the initial port mapping remains same.

How to change port mapping of a running container
1) Stop and delete the container. Start fresh using run command. However all local changes will be lost.
2) Stop container. Save it as an image. Run the newly created image.
3) Stop container. Stop docker using command -> systemctl stop docker.service
Navigate to /var/lib/docker/containers/\<container_id>. This folder will contain all the containers config files.
In file hostconfig.json, add appropriate port binding, "PortBindings":{"8080/tcp":[{"HostIp":"","HostPort":"8051"}]}
In file config.v2.json, add exposed ports, "ExposedPorts":{"8080/tcp":{}}
Start docker service using command --> systemctl start docker.service
Start container. Newly mapped ports will be now visible.
