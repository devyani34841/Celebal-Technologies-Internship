Task 8:Docker Compose for multi-container applications, Docker security best practices
----------------------------------------------------------------------------------------------------------------------------------------------------------------
Purpose of Task:
The objective of this task was to gain practical experience in orchestrating multi-container Docker applications using Docker Compose and to understand fundamental security best practices for building and 
running Docker containers.This task aims to demonstrate simplified local development environment setup for interconnected services and to highlight crucial steps for securing containerized applications against 
common vulnerabilities.

Sample Multi-Container Application (Web App with Redis)
A simple Node.js web application that connects to a Redis cache was utilized to demonstrate Docker Compose.

Project Directory Structure Creation:
A main project directory (my_compose_app) was created. Inside it, a web subdirectory for the Node.js application and the docker-compose.yml file were created.

my_compose_app/
├── web/
│   ├── Dockerfile
│   ├── package.json
│   └── server.js
└── docker-compose.yml

Web Service Files (web/ directory):

web/server.js:

const express = require('express');
const redis = require('redis');
const process = require('process'); // For graceful shutdown
const app = express();
const port = 3000;

// Connect to Redis. 'redis-server' is the service name defined in docker-compose.yml
const client = redis.createClient({
    socket: {
        host: 'redis-server',
        port: 6379
    }
});

client.on('error', (err) => console.log('Redis Client Error', err));

// Connect to Redis when the app starts
client.connect().catch(console.error);

app.get('/', async (req, res) => {
    let hits = await client.incr('hits');
    res.send(`Hello from Docker Compose! I have been hit ${hits} times. (Node.js app with Redis)`);
});

app.listen(port, () => {
    console.log(`Web app listening on port ${port}`);
});

// Graceful shutdown
process.on('SIGINT', async () => {
    console.log('Shutting down gracefully...');
    await client.disconnect();
    process.exit(0);
});

web/package.json:

{
  "name": "docker-compose-app",
  "version": "1.0.0",
  "description": "A simple web app for Docker Compose",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "redis": "^4.6.13"
  }
}

web/Dockerfile:

Multi-stage build for a smaller final image
Stage 1: Builder
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install --production=false # Install dev dependencies during build, but not for final image

Stage 2: Production
FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/node_modules ./node_modules
COPY server.js .

EXPOSE 3000
CMD ["npm", "start"]

Docker Compose Configuration File (docker-compose.yml):
This file defines the two services (web and redis-server) and their interactions.

version: '3.8' # Specify the Compose file format version

services:
  web:
    build: ./web # Instructs Compose to build the image from the web/ directory
    ports:
      - "8000:3000" # Map host port 8000 to container port 3000
    environment:
      - NODE_ENV=production
    depends_on:
      - redis-server # Ensures redis-server starts before web
    networks:
      - app-network # Connects to the custom network

  redis-server:
    image: "redis:alpine" # Uses the official Redis image from Docker Hub
    command: ["redis-server", "--appendonly", "yes"] # Enable persistence for Redis
    volumes:
      - redis-data:/data # Mounts a named volume for Redis data persistence
    networks:
      - app-network # Connects to the custom network

networks:
  app-network: # Define a custom bridge network
    driver: bridge

volumes:
  redis-data: # Define the named volume for Redis persistence
  
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Docker Compose Operations
Navigate to Project Directory:
The terminal or command prompt was opened, and navigation was performed to the my_compose_app directory containing docker-compose.yml.

cd path/to/my_compose_app

Build and Run Services (in detached mode):
Docker Compose was used to build the web service image (if not already built) and start all services defined in docker-compose.yml. The -d flag ran them in the background.

docker compose up -d

Verify Running Services:
The status of the Docker Compose services was checked.

docker compose ps

View Service Logs:
Logs for a specific service (e.g., web) or all services were viewed to monitor their startup and operation.

docker compose logs web
 Or for all services:
 docker compose logs

Access the Application:
A web browser was used to navigate to http://localhost:8000. The message indicating the number of hits from the Redis cache was displayed, confirming inter-service communication.

Stop and Remove Services:
All services, networks, and volumes created by Docker Compose were stopped and removed. This is a clean way to tear down the entire application stack.

docker compose down --volumes

The --volumes flag ensures named volumes (like redis-data) are also removed, which is useful for development cleanup.

 Detailed Steps: Docker Security Best Practices
This section outlines key security best practices implemented when building and running Docker containers.

Minimize Base Images (Example)
Instead of using a full node:18 image, the node:18-alpine variant was selected in the Dockerfile. alpine images are significantly smaller and have a reduced attack surface due to fewer installed packages.

Implement Multi-Stage Builds (Example)
As demonstrated in the Dockerfile for the Node.js application (web/Dockerfile), a multi-stage build was used. This ensures that build-time tools and dependencies (like the full npm install environment) are not 
included in the final production image, which only contains the necessary runtime components.

Run as a Non-Root User (Example)
By default, Docker containers run as the root user. To enhance security, a non-root user was created and used within the Dockerfile.

Modified web/Dockerfile snippet:

 ... (Stage 1: Builder remains the same) ...

Stage 2: Production
FROM node:18-alpine
WORKDIR /app

 Create a non-root user and group
RUN addgroup --system appgroup && adduser --system --ingroup appgroup appuser

Copy only the necessary files from the builder stage
COPY --from=builder /app/node_modules ./node_modules
COPY server.js .

Change ownership of the /app directory to the new non-root user
RUN chown -R appuser:appgroup /app

Switch to the non-root user
USER appuser

EXPOSE 3000
CMD ["npm", "start"]

addgroup and adduser: Commands to create a system group and user.

chown: Changes ownership of the application directory.

USER: Instruction to switch the user for subsequent Dockerfile commands and runtime.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Using .dockerignore
A .dockerignore file was created in the root of the my_compose_app/web directory (same level as the Dockerfile). This prevents unnecessary files (like node_modules from the host, .git directories, IDE files) 
from being copied into the build context, reducing build times and image size, and preventing accidental exposure of sensitive files.

web/.dockerignore:

node_modules
.git
.gitignore
.vscode/
npm-debug.log
Dockerfile

Minimize Exposed Ports

In the docker-compose.yml, only the necessary application port (3000 for the web app, mapped to 8000 on the host) was exposed. Internal communication between web and redis-server happened over the app-network without exposing Redis's port to the host machine.
