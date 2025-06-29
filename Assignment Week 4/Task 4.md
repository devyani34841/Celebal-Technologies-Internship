Task4: Create a docker image from multiple methods likes Dockerfile, running containers.
--------------------------------------------------------------------------------------------------------------------------------------------------------------
Method 1: Building an Image from a Dockerfile

This method provides a structured and reproducible way to build images.

Preparation of Application Files:
A simple application (e.g., a static HTML page) was prepared. A directory named my_static_web was created. Inside this directory, an index.html file and a Dockerfile were placed.

index.html:


<!DOCTYPE html>

<html lang="en">
    
<head>
    <meta charset="UTF-8">
    
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <title>Docker Web App</title>
    
    <style>
    
        body { font-family: Arial, sans-serif; text-align: center; margin-top: 50px; }
        
        h1 { color: #333; }
        
        p { color: #666; }
        
    </style>
    
</head>


<body>
    
    <h1>Hello from a Dockerized Web Server!</h1>
    
    <p>This page is served by Nginx inside a custom Docker image.</p>
    
</body>

</html>

---------------------------------------------------------------------------------------------------------------------------------------------------------

Dockerfile:

Use Nginx as the base image
FROM nginx:alpine

Copy the custom index.html file to Nginx's default public directory
COPY index.html /usr/share/nginx/html/index.html 

Expose port 80 as Nginx listens on this port by default
EXPOSE 80

Command to run Nginx (default for nginx:alpine)
CMD ["nginx", "-g", "daemon off;"]

Navigation to Application Directory:
The terminal or command prompt was opened, and navigation was performed to the my_static_web directory where index.html and Dockerfile were located.

cd path/to/my_static_web

Docker Image Construction:
The docker build command was utilized to construct the image from the Dockerfile. The -t flag tagged the image with a descriptive name.

docker build -t custom-nginx-web:1.0 .

Image Verification:
Upon completion of the build process, local Docker images were listed to confirm the successful creation of the new image.

docker images

Container Instantiation and Verification:
A container was launched from the custom-nginx-web:1.0 image, mapping host port 8080 to container port 80.

docker run -d -p 8080:80 --name my-custom-nginx custom-nginx-web:1.0

Verification was performed by accessing http://localhost:8080 in a web browser.

-------------------------------------------------------------------------------------------------------------------------------------------------------------
Method 2: Creating an Image from a Running Container (docker commit)

This method captures the current state of a running container as a new image.

Launch a Base Container:

A base Ubuntu container was launched in interactive mode to allow modifications.

docker run -it --name adhoc-ubuntu ubuntu:latest bash

Perform Modifications Inside the Container:

Once inside the container's bash shell, a simple modification was made, for instance, installing a package or creating a file.

Inside the container
apt-get update

apt-get install -y cowsay

echo "Hello from Ad-hoc Image!" | cowsay > /opt/message.txt

exit # Exit the container's shell

Commit the Container's State to a New Image:

From the host machine's terminal (after exiting the container's shell), the docker commit command was used. The container name (adhoc-ubuntu) was specified,

along with the desired name and tag for the new image.

docker commit adhoc-ubuntu adhoc-ubuntu-cowsay:1.0

Verify the New Image:

Local Docker images were listed to confirm the creation of the adhoc-ubuntu-cowsay:1.0 image.

docker images

Run a Container from the Committed Image:

A new container was launched from the adhoc-ubuntu-cowsay:1.0 image to verify the changes.

docker run -it adhoc-ubuntu-cowsay:1.0 bash

Once inside, the created file was checked:

 Inside the new container
 
cat /opt/message.txt

cowsay "Check if cowsay works"

exit
