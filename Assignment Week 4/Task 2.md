Task2: Docker installation and basic container operations, Build an image from Dockerfile
Purpose of Task:
The objective of this task was to solidify the understanding of Docker's core functionalities by performing practical operations, specifically focusing on the process of building custom Docker images using
a Dockerfile. This builds upon the foundational knowledge of containerization and basic Docker commands acquired in the previous task. The goal is to demonstrate the ability to containerize a simple applica-
tion by defining its environment and dependencies within a Dockerfile and then constructing an executable image.
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Key Concepts Revisited
This task primarily focuses on the practical application of two critical Docker concepts:
Dockerfile: A script containing instructions for Docker to build a new image. Each instruction in a Dockerfile creates a new layer in the image, contributing to its immutability and efficient caching.
Docker Image: The read-only template that becomes a container when executed. Building an image from a Dockerfile transforms application code and its environment into a portable, deployable unit.
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Pre-requisites: Setup
For the successful execution of this task, the following pre-requisites were ensured:
Docker Installed: Docker Desktop (for Windows/macOS) or Docker Engine (for Linux) was installed and running on the system.
Basic Docker Commands Familiarity: Familiarity with docker pull, docker run, docker ps, and docker images was established from the previous task.
Sample Application Code: A simple application (e.g., a basic Python Flask app, Node.js app, or a static HTML page) and its Dockerfile were prepared for demonstration purposes.
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Building an Image
This section outlines the step-by-step process followed for building a Docker image using a Dockerfile.
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Sample Application Files
For this task, a simple Python Flask application was utilized. A directory named my_flask_app was created, and within this directory, two files were created: app.py and requirements.txt.

app.py:

# app.py
from flask import Flask
import os

app = Flask(__name__)

@app.route('/')
def hello():
    name = os.environ.get('NAME', 'World')
    return f'Hello, {name} from a Dockerized Flask App!'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)

requirements.txt:

Flask==2.3.3
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Dockerfile:
The Dockerfile was placed in the same my_flask_app directory as app.py and requirements.txt.

# Use an official Python runtime as a parent image
FROM python:3.9-slim-buster

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Make port 5000 available to the world outside this container
EXPOSE 5000

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Build Process Steps
Application Directory Navigation:
The terminal or command prompt was opened, and navigation was performed to the my_flask_app directory where the Dockerfile, app.py, and requirements.txt were located.

cd path/to/my_flask_app

Docker Image Construction:
The docker build command was utilized. The -t flag facilitated tagging the image with a human-readable name and version. The . argument directed Docker to use the current directory as the build context for
the Dockerfile.

docker build -t my-flask-app:1.0 .

Docker processed the Dockerfile instructions sequentially.

The base image (python:3.9-slim-buster) was pulled.

The working directory within the container was set.

Application files were copied into the image.

The pip install command was executed to install Flask dependencies.

Finally, the designated port was exposed, and the default command for the container's startup was configured.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Image Verification:
Upon completion of the build process, the local Docker images were listed to confirm the successful creation of the new image.

docker images

Container Instantiation from New Image:
A container was launched from the custom-built image. The -p flag was used to map the container's port 5000 to a port on the host (e.g., 5000 or 8000), and --name provided a memorable name for the container.

docker run -d -p 5000:5000 --name running-flask-app my-flask-app:1.0

The -d flag ensured the container ran in detached (background) mode.

Running Container Verification:
The status of the container was checked to confirm its correct operation.

docker ps
```running-flask-app` was observed as listed with its status indicated as `Up`.

Application Access:
A web browser was used to navigate to http://localhost:5000. The "Hello, World from a Dockerized Flask App!" message was displayed (or "Hello, [Your Name]" if the NAME environment variable was set during 
container execution).

====================================================================================================================================================================================================================



