#### Listing Containers

### List currently running containers
```
docker ps
```
### List all Docker containers (running and stopped)
```
docker ps -a
```
### Login to Docker Hub or a private registry
```
docker login -<username>
```
### Build a Docker image from a Dockerfile
```
docker build -t <image_name> .
```
### Run a container from an image in detached mode (-d), mapping port 80
```
docker run -d -p 80:80 <image_id> <container_name>
```
### Run a container with a custom name
```
docker run --name <container_name> <image_name>
```
### Run a container and publish a container’s port(s) to the host
```
docker run -p <host_port>:<container_port> <image_name>
```
### Open a shell inside a running container
```
docker exec -it <container_name> sh
```
### Exec into a container (Alpine example)
```
docker exec -it <image_id> /bin/sh
```
### Tag an image (useful for pushing to a repository)
```
docker tag <image_name> <user_name>/<repo_name>:<new_tag>
```
### Pull an image from a repository
```
docker pull <repo_name>/<image_name>:<image_tag>
```
### Push an image to a repository
```
docker push <repo_name>:<tag_name>
```
### Remove a Docker image
```
docker rmi <image_name>:<tag_name>
```
### Add a new tag to an existing image and push it
```
docker tag <image_name>:<tag_name> <ex_repo_name>:<tag_name>
docker push <ex_repo_name>
```
### Remove a stopped container
```
docker rm <container_name>
```
### Stop a running container
```
docker stop <container_name>
```
### Start a stopped container
```
docker start <container_name>
```
### Remove unused containers, networks, images, and caches
```
docker system prune -a
```
### Search for available tags of a Docker image using cURL and jq
```
curl -s https://registry.hub.docker.com/v2/repositories/<repo_name>/tags | jq '.results[].name'
```

