### Format to specify tag
FROM nginx:mainline-alpine3.20-slim@sha256:e9293c9bedb0db866e7d2b69e58131db4c2478e6cd216cdd99b134830703983a

### FROM: This instruction sets a build stage for the container as well as setting the base image (operating system). All Dockerfiles must start with this.	
```
FROM ubuntu
```

	
### RUN: This instruction will execute commands in the container within a new layer.	
```
RUN whoami
```

### COPY: This instruction copies files from the local system to the working directory in the container (the syntax is similar to the cp command).
```
COPY /home/cmnatic/myfolder/app/
```
### WORKDIR: This instruction sets the working directory of the container. (similar to using cd on Linux).
Sets to the root of the filesystem in the container
```
WORKDIR /
```
### CMD: This instruction determines what command is run when the container starts (you would use this to start a service or application).	
```
CMD /bin/sh -c script.sh
```
### EXPOSE: This instruction is used to tell the person who runs the container what port they should publish when running the container.
Exposes port 80 
```
EXPOSE 80
```
