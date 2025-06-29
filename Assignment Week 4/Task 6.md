Task 6: Create a Custom Docker Bridge Network
Purpose of Task:
The objective of this task was to understand and implement Docker's custom bridge networking, which is essential for enabling isolated and name-resolvable communication between multiple containers. This task 
aims to demonstrate how to create a user-defined network, connect containers to it, and verify inter-container communication using service names instead of IP addresses.

Network Creation
A new custom bridge network was created using the docker network create command.

docker network create my-custom-app-net

Verify Network Creation:
The list of Docker networks was inspected to confirm the new network's presence.

docker network ls

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Run Containers on the Custom Network
Two containers will be launched and connected to my-custom-app-net. A simple Nginx web server and a busybox container will be used to demonstrate communication.

Launch Nginx Container:
An Nginx web server container was launched and attached to my-custom-app-net. The container was named web-server.

docker run -d --name web-server --network my-custom-app-net nginx:alpine

Launch Busybox Container:
A busybox container was launched and also attached to my-custom-app-net. This container will be used to test connectivity to the Nginx container.

docker run -it --name network-tester --network my-custom-app-net busybox sh

This command will place the user directly into the busybox container's shell.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Verify Inter-Container Communication
From within the network-tester (busybox) container's shell, connectivity to web-server was verified using its container name.

Ping the Web Server by Name:
Inside the network-tester container, a ping command was executed to the web-server container using its name.

Inside the network-tester container
ping web-server

Expected Outcome: Successful ping replies, indicating DNS resolution by name.

Access Web Server Content (wget):
Still inside the network-tester container, wget was used to retrieve the Nginx default page from web-server.

Inside the network-tester container
wget -O- web-server

Expected Outcome: HTML content of the Nginx welcome page.

Exit Busybox Container:
After testing, the busybox container's shell was exited.

Inside the network-tester container
exit

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Inspect the Network and Clean Up
Inspect Custom Network:
Detailed information about the custom network, including attached containers and their IP addresses, was inspected.

docker network inspect my-custom-app-net

Clean Up Containers:
The running containers were stopped and removed.

docker stop web-server network-tester
docker rm web-server network-tester

Remove Custom Network:
The custom bridge network was removed.

docker network rm my-custom-app-net
