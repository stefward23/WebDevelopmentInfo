### Creates a named volume that can be used by containers.
```
docker volume create <name>
```
### Default path where Docker stores volumes
This is the directory where Docker saves volume data on the host system.  
/var/lib/docker/volumes/  
### Displays all Docker volumes on the system.
```
docker volume list
```
### Shows detailed information about a specific volume, including mount points
```
docker volume inspect <name>
```
### Remove a specific volume Note: The volume must not be in use.
```
docker volume rm <name>
```
### Remove all unused volumes Note: Deletes all local volumes not currently used by any containers.
```
docker volume prune
```
### Start a container with a mounted volume Note: Runs an Nginx container with a persistent volume mounted to store website data.
```
docker run --name my_nginx_c1 --volume my_nginx_volume:/usr/share/nginx/html -d nginx
```
