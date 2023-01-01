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
