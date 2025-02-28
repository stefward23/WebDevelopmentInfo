# Enable Docker Service to start on boot (PowerShell)
sc config docker start= auto

# Start a new container with a restart policy (always on)
docker run -d --restart=unless-stopped --name my_container my_image

