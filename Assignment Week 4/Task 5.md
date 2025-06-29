Task 5:Push and pull image to Docker hub and ACR
=================================================================================================================================================================
Method 1: Docker Hub Operations
This method covers pushing and pulling an image to Docker Hub.

Docker Hub Authentication:
Authentication with Docker Hub was performed via the command line. This connects the local Docker client to the user's Docker Hub account.

docker login

Input Docker ID and Password when prompted.

Tag Image for Docker Hub (if not already tagged):
If the local image was not already tagged with the Docker Hub username (e.g., from Task 3), it was tagged appropriately.

docker tag my-local-image:latest your-dockerhub-username/my-local-image:1.0

(Used your-dockerhub-username/my-nodejs-app:1.0 directly from Task 3, so retagging was not strictly needed if that image was used).

Push Image to Docker Hub:
The image was uploaded to the Docker Hub registry. The image name adhered to the your-dockerhub-username/repository-name:tag format.

docker push your-dockerhub-username/my-nodejs-app:1.0

Verify Image on Docker Hub:
A web browser was used to navigate to hub.docker.com, and a login was performed. The repository (your-dockerhub-username/my-nodejs-app) was located under "Repositories" to confirm the successful upload of the image and its tag.

Remove Local Image (for pull demonstration):
The locally stored image was removed to simulate a fresh download.

docker rmi your-dockerhub-username/my-nodejs-app:1.0

Pull Image from Docker Hub:
The image was downloaded from Docker Hub to the local machine.

docker pull your-dockerhub-username/my-nodejs-app:1.0

Verify Pulled Image Locally:
Local Docker images were listed to confirm that the image was successfully pulled.

docker images

-----------------------------------------------------------------------------------------------------------------------------------------------------------------

Method 2: Azure Container Registry (ACR) Operations
This method covers pushing and pulling an image to Azure Container Registry.

Azure CLI Authentication:
Authentication to Azure was performed using the Azure CLI.

az login

A web browser window opened for Azure login.

ACR Login (Docker CLI):
The Docker CLI was authenticated with the Azure Container Registry using the Azure CLI's acr login command. This uses your Azure identity to authenticate Docker.

az acr login --name <your_acr_name>

The <your_acr_name> is the name of your ACR instance (e.g., mydevopsregistry).

Tag Image for ACR:
The local image was tagged with the full ACR login server name to prepare it for pushing to ACR.

docker tag your-dockerhub-username/my-nodejs-app:1.0 <your_acr_name>.azurecr.io/my-nodejs-app:1.0

Push Image to ACR:
The image was uploaded to the Azure Container Registry.

docker push <your_acr_name>.azurecr.io/my-nodejs-app:1.0

Verify Image on ACR (Azure Portal/CLI):
Verification was performed either through the Azure Portal (navigating to your ACR instance -> Repositories) or via Azure CLI:

az acr repository show-tags --name <your_acr_name> --repository my-nodejs-app --output table

Remove Local Image (for pull demonstration):
The locally stored ACR-tagged image was removed to simulate a fresh download.

docker rmi <your_acr_name>.azurecr.io/my-nodejs-app:1.0

Pull Image from ACR:
The image was downloaded from Azure Container Registry to the local machine.

docker pull <your_acr_name>.azurecr.io/my-nodejs-app:1.0

Verify Pulled Image Locally:
Local Docker images were listed to confirm that the image was successfully pulled.

docker images
