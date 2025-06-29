Task 7: Create a Docker volume and mount it to a container.
-----------------------------------------------------------------------------------------------------------------------------------------------------------------

Purpose of Task:
The objective of this task was to understand and implement Docker Volumes for persistent data storage, a critical aspect of containerized applications, especially for databases or applications that 
generate/store important data. This task aims to demonstrate how to create a Docker volume, mount it to a container, write data into it from within the container, and verify data persistence across container 
lifecycle changes.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------

Create a Docker Volume
Volume Creation:
A new named Docker volume was explicitly created using the docker volume create command.

docker volume create my-data-volume

Verify Volume Creation:
The list of Docker volumes was inspected to confirm the new volume's presence.

docker volume ls

Run a Container and Mount the Volume
Launch Container with Volume:
An Ubuntu container was launched, and my-data-volume was mounted to the /app/data directory inside the container. The container was named data-writer.

docker run -it --name data-writer -v my-data-volume:/app/data ubuntu:latest bash

This command placed the user directly into the ubuntu container's shell.

Write Data to the Mounted Volume:
Inside the data-writer container's shell, a new file was created and content written into the mounted volume directory (/app/data).

Inside the data-writer container
echo "This data persists!" > /app/data/persistent_file.txt
ls /app/data/
cat /app/data/persistent_file.txt
exit # Exit the container's shell

Verify Data Persistence
The data-writer container was stopped and removed, then a new container was launched, mounting the same volume to demonstrate data persistence.

Stop and Remove the First Container:
From the host machine's terminal (after exiting the data-writer container's shell), the data-writer container was stopped and removed.

docker stop data-writer
docker rm data-writer

Launch a New Container with the Same Volume:
A new Ubuntu container was launched, also mounting my-data-volume to /app/data. This container was named data-reader.

docker run -it --name data-reader -v my-data-volume:/app/data ubuntu:latest bash

Verify Data in the New Container:
Inside the data-reader container's shell, the contents of the mounted volume were checked.

Inside the data-reader container
ls /app/data/
cat /app/data/persistent_file.txt
exit # Exit the container's shell

Expected Outcome: persistent_file.txt should be present, and its content should be "This data persists!".

Inspect and Clean Up Volumes
Inspect the Volume:
Detailed information about my-data-volume, including its mount point on the host, was inspected.

docker volume inspect my-data-volume

Clean Up Volume:
The Docker volume was removed.

docker volume rm my-data-volume
