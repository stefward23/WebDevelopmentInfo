### Create a Dockerfile that defines the environment for building the image
Specifies the base image to use
```
FROM <name_of_image>
```
### Copy files from the source directory to the container’s destination directory
```
COPY <source> <destination>
```
### Example: Copy files to nginx's default HTML directory (for a web server)
```
COPY <source_path> /usr/share/nginx/html
```
### Expose the specified port (optional, mainly for documentation)
Tells Docker that the container listens on port 80  
```
EXPOSE 80
```
### CMD specifies the default command to run when the container starts (nginx example)
Runs nginx in the foreground when the container starts
```
CMD ["nginx", "-g", "daemon off;"]
```

### Create .dockerignore file (exclude files from being copied into the Docker image)
Enter **/<file_name> into .dockerignore to prevent specific files/folders from being added to the image  

### Run the Docker container in detached mode, mapping port 80 on the host to port 80 on the container
```
docker run -d -p 80:80 <image_id> <container_name>
```
### Lists all running containers with their IDs and basic information
```
docker ps
```
### Executes an interactive shell session inside a running container (useful for debugging or maintenance)
```
docker exec -it <image_id> /bin/sh  ### For Alpine-based containers, use /bin/sh for shell access
```
