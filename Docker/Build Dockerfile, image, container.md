# Creates a Dockerfile that defines the environment for building the image
```
FROM <name_of_image>  # Specifies the base image to use
```
# Copies files from the source directory to the container’s destination directory
```
COPY <source> <destination>
```
# Example: Copies files to nginx's default HTML directory (for a web server)
```
COPY <source_path> /usr/share/nginx/html
```
# Exposes the specified port (optional, mainly for documentation)
# EXPOSE 80   # Tells Docker that the container listens on port 80

# CMD specifies the default command to run when the container starts (nginx example)
CMD ["nginx", "-g", "daemon off;"]  # Runs nginx in the foreground when the container starts

# Create .dockerignore file (exclude files from being copied into the Docker image)
# Enter **/<file_name> into .dockerignore to prevent specific files/folders from being added to the image

# Runs the Docker container in detached mode, mapping port 80 on the host to port 80 on the container
```
docker run -d -p 80:80 <image_id> <container_name>
```
# Lists all running containers with their IDs and basic information
```
docker ps
```
# Executes an interactive shell session inside a running container (useful for debugging or maintenance)
```
docker exec -it <image_id> /bin/sh  # For Alpine-based containers, use /bin/sh for shell access
```
