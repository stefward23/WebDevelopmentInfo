# Docker Compose File Instructions

- **`version ('3.3')`** – Specifies the version of the Compose file format being used.
- **`services (services:)`** – Marks the beginning of container/service definitions in the `docker-compose.yml` file.
- **`name (webserver)`** – Defines the name of the container. Replace `"name"` with an actual service name like `"webserver"` or `"database"`.
- **`build (./webserver)`** – Specifies the directory containing the `Dockerfile` for building the container (alternative to `image`).
- **`ports ('80:80')`** – Maps the host machine’s port to the container’s port (`host:container`).
- **`volumes ('./home/cmnatic/webserver/:/var/www/html')`** – Mounts a directory from the host into the container.
- **`environment (MYSQL_ROOT_PASSWORD=helloworld)`** – Defines environment variables (e.g., passwords, usernames, configurations).  
  ⚠️ *Note: Not secure for sensitive values.*
- **`image (MySQL:latest)`** – Specifies the image to use for the container (alternative to `build`).
- **`networks (ecommerce)`** – Assigns the container to a network. A container can belong to multiple networks.
- **`depends_on:`** – Ensures that a service starts only after its dependencies have started.
