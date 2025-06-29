Task 3:Docker Registry, DockerHub, Create a Multi-Stage Build
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Creating a Multi-Stage Build and Using DockerHub
Sample Application Files for Multi-Stage Build For this task, a simple Node.js application will be used to demonstrate a multi-stage build. This setup differentiates build-time dependencies (like npm install) from the final runtime environment.
Create a directory named my_nodejs_app. Inside this directory, create three files: package.json, server.js, and Dockerfile.

package.json:

{
  "name": "simple-node-app",
  "version": "1.0.0",
  "description": "A simple Node.js web server",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}

server.js:

const express = require('express');
const app = express();
const port = process.env.PORT || 3000;
const os = require('os');

app.get('/', (req, res) => {
  res.send(`Hello from Docker! Running on host: ${os.hostname()} (Node.js App)`);
});

app.listen(port, () => {
  console.log(`App listening at http://localhost:${port}`);
});
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Dockerfile (Multi-Stage Build):
This Dockerfile uses two stages: builder for installing dependencies and production for the final slim image.

# Stage 1: Builder
# Uses a Node.js image with build tools
FROM node:18-alpine AS builder

WORKDIR /app

COPY package*.json ./

RUN npm install --production

# Stage 2: Production
# Uses a much smaller Node.js runtime image
FROM node:18-alpine

WORKDIR /app

# Copy only the necessary files from the builder stage
COPY --from=builder /app/node_modules ./node_modules
COPY server.js .

EXPOSE 3000

CMD ["npm", "start"]
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Build Process Steps
Application Directory Navigation:
The terminal or command prompt was opened, and navigation was performed to the my_nodejs_app directory where package.json, server.js, and Dockerfile were located.

cd path/to/my_nodejs_app

Docker Image Construction (Multi-Stage):
The docker build command was utilized to construct the image from the multi-stage Dockerfile. The -t flag tagged the image with the Docker Hub username to prepare for pushing.

docker build -t your-dockerhub-username/my-nodejs-app:1.0 .

Docker processed both stages (builder and the final unnamed stage).

Dependencies were installed in the builder stage, but only the node_modules and server.js were copied to the much smaller final image.

Image Verification:
Upon completion of the build process, local Docker images were listed to confirm the successful creation of the new, optimized image.

docker images

Observe the size difference compared to a single-stage build (if performed).

Container Instantiation from New Image:
A container was launched from the custom-built image. The -p flag mapped the container's port 3000 to a host port (e.g., 8000), and --name provided a memorable name.

docker run -d -p 8000:3000 --name running-nodejs-app your-dockerhub-username/my-nodejs-app:1.0

Running Container Verification:
The status of the container was checked to confirm its correct operation.

docker ps
```running-nodejs-app` was observed as listed with its status indicated as `Up`.


Application Access:
A web browser was used to navigate to http://localhost:8000. The "Hello from Docker! Running on host: ..." message was displayed, confirming the Node.js application's functionality within the container.

DockerHub Operations
DockerHub Login:
Authentication with Docker Hub was performed via the command line. This connects the local Docker client to the user's Docker Hub account.

docker login

Prompted for Docker ID and Password.

Image Push to DockerHub:
The custom-built image was uploaded to the Docker Hub registry. The image name must follow the your-dockerhub-username/repository-name:tag format.

docker push your-dockerhub-username/my-nodejs-app:1.0

Progress messages indicate layers being pushed.

Verification on Docker Hub:
A web browser was used to navigate to hub.docker.com and log in. The repository (your-dockerhub-username/my-nodejs-app) was located under "Repositories" to confirm the successful upload of the image and its tag.
