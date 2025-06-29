
Task1:Introduction to containerization and Docker fundamentals, Basic Commands
-------------------------------------------------------------------------------------------------------------------------------------------------------------------
Introduction to Containerization
What is Containerization?
Containerization is a lightweight form of operating-system-level virtualization. It involves packaging an application and all its necessary components—
including code, runtime, libraries, system tools, and configuration files—into a self-contained, isolated unit known as a container. This container is 
engineered to ensure consistent and reliable execution across diverse computing environments, from a developer's workstation to on-premises servers, 
virtual machines, or cloud platforms.

----------------------------------------------------------------------------------------------------------------------------------------------------------------
 Benefits of Containerization
Containerization is a cornerstone of modern DevOps, primarily addressing the "it works on my machine" problem by guaranteeing environmental consistency. 
Its core advantages include:
Portability: Containers encapsulate all dependencies, enabling applications to be built once and deployed uniformly across any compatible environment.
Consistency: Ensures applications behave identically across development, testing, staging, and production environments, minimizing discrepancies.
Isolation: Each container operates in an isolated environment, preventing conflicts between applications or their dependencies. The failure of 
one container does not impact others.
Efficiency: Containers share the host operating system's kernel, making them significantly lighter and faster to provision and start compared
to traditional Virtual Machines (VMs). This leads to optimized resource utilization.
Scalability: Containers can be rapidly scaled up or down to meet fluctuating demand, particularly when managed by container orchestration platforms 
like Kubernetes.
Faster Deployment: The standardized and isolated nature of containers streamlines Continuous Integration/Continuous Delivery (CI/CD) pipelines, 
facilitating quicker and more reliable software deployments.

----------------------------------------------------------------------------------------------------------------------------------------------------------------
2. Docker Fundamentals
Docker is the leading open-source platform designed to facilitate the development, shipment, and execution of applications within containers. It provides a 
comprehensive suite of tools for managing the entire container lifecycle.

Key Docker Concepts:
Dockerfile: A plain text file containing a sequence of instructions (commands) for building a Docker image. It acts as a reproducible script for defining and 
creating container environments.

Docker Image: A read-only, immutable template that contains a complete, executable package of software. This includes the application code, a runtime, system 
libraries, environment variables, and configuration files required for the application to run. Images are built from Dockerfiles.

Docker Container: A runnable instance of a Docker image. When an image is executed, it becomes a container, providing an isolated, portable environment where 
the application actively runs. Containers can be started, stopped, paused, and removed as needed.

Docker Registry (e.g., Docker Hub): A centralized repository service where Docker images can be stored, managed, and shared publicly or privately. Docker Hub is 
the default public registry, hosting numerous official and community-contributed images.

Docker Daemon (dockerd): A persistent background process that runs on the host system. It listens for Docker API requests from the client and manages all Docker
objects, such as images, containers, networks, and volumes.

Docker Client (docker CLI): The command-line interface (CLI) that users interact with to send commands to the Docker Daemon, initiating container operations 
(e.g., building images, running containers).
-------------------------------------------------------------------------------------------------------------------------------------------------------------------
Docker Architecture
Docker operates on a client-server architecture. The Docker client communicates with the Docker daemon, which is responsible for executing the commands and
managing all Docker objects.

+------------------+       +-------------------+       +--------------------+
| Docker Client    | <---> | Docker Daemon     | <---> | Docker Registries  |
| (CLI/API)        |       | (dockerd)         |       | (e.g., Docker Hub) |
+------------------+       +-------------------+       +--------------------+
                              |
                              v
                       +-------------------+
                       | Docker Objects    |
                       | (Images, Containers,|
                       | Networks, Volumes)  |
                       +-------------------+
-------------------------------------------------------------------------------------------------------------------------------------------------------------------

3. Basic Docker Commands
This section outlines fundamental Docker commands crucial for managing images and containers effectively.

1] docker --version: Displays the installed Docker client and engine version.

2] docker info: Provides detailed system-wide information about Docker, including statistics on containers, images, and other Docker configurations.

3] docker pull <image_name>[:<tag>]: Downloads a Docker image from a specified registry (defaults to Docker Hub). If no <tag> is provided, latest is assumed.

Example: docker pull ubuntu:22.04

4] docker images: Lists all Docker images stored locally on your system.

5] docker rmi <image_id_or_name>[:<tag>]: Removes one or more Docker images from local storage. The -f or --force option can be used to override warnings about 
in-use images.

Example: docker rmi my-app-image:1.0

6] docker run [OPTIONS] <image_name>[:<tag>] [COMMAND] [ARG...]: Creates and starts a new container from a specified Docker image. Key options include:

7]-d (detached mode): Runs the container in the background.

8] -p <host_port>:<container_port>: Maps a port from the host machine to a port inside the container.

9]--name <container_name>: Assigns a custom, human-readable name to the container.

10]-it (interactive TTY): Allocates a pseudo-TTY and keeps STDIN open, allowing for interactive sessions (e.g., shell access).

Example (run Nginx in detached mode, port mapping): docker run -d -p 8080:80 --name my-webserver nginx

Example (run interactive Ubuntu shell): docker run -it ubuntu bash

11] docker ps: Lists all currently running Docker containers.

12] docker ps -a: Lists all Docker containers, including those that have stopped.

13]docker stop <container_id_or_name>: Gracefully stops one or more running containers.

Example: docker stop my-webserver

14]docker start <container_id_or_name>: Starts one or more stopped containers.

Example: docker start my-webserver

15]docker restart <container_id_or_name>: Restarts one or more containers.

16]docker rm <container_id_or_name>: Removes one or more stopped containers. Use -f for forced removal of running containers.

Example: docker rm my-webserver

16]docker exec [OPTIONS] <container_id_or_name> <command>: Executes a command inside a running container. Often used with -it for interactive sessions.

Example: docker exec -it my-webserver bash

17]docker logs <container_id_or_name>: Retrieves and displays the logs generated by a container. Use -f to follow logs in real-time.

Example: docker logs -f my-webserver

18]docker inspect <object_id_or_name>: Returns low-level information on Docker objects (containers, images, networks, volumes).

Example: docker inspect my-webserver

19]docker build -t <image_name>[:<tag>] <path_to_dockerfile>: Builds a new Docker image from a Dockerfile. The . typically refers to the current directory 
containing the Dockerfile.

Example: docker build -t my-python-app:v1.0 .
*******************************************************************************************************************************************************************
