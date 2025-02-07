# User Management App - DevOps Implementation

## Project Overview
This project is a full-stack user management application that allows users to create, read, update, and delete (CRUD) user data. The backend is built with Node.js, Express, and MongoDB, while the frontend is developed with React. The application is designed to be easily deployable and scalable.

## Deployment Strategy
To deploy this project efficiently, we applied DevOps best practices to ensure reliability, scalability, and automation. The main goal was to containerize the application, automate its deployment, and integrate CI/CD to streamline updates.

### Containerization
We split the application into three independent containers:
1. **Backend (Node.js + Express + MongoDB Driver)**: A separate container running the backend API to manage user data.
2. **Frontend (React + Static Files)**: A dedicated frontend container serving the user interface.
3. **Database (MongoDB)**: A persistent storage container holding the user data.

To optimize container size, we used Debian-based minimal images, reducing overhead while maintaining compatibility with required dependencies.

### Docker Compose
To manage multi-container orchestration, we used Docker Compose. This allowed us to:
- Define and manage all services in a single `docker-compose.yml` file.
- Ensure proper dependency resolution (backend waits for MongoDB to be available before starting).
- Simplify deployment by running a single command to bring up the entire stack.

### CI/CD with Jenkins
To automate the build, test, and deployment process, we set up a Jenkins pipeline. The pipeline ensures that every change to the application is automatically built, tested, pushed to Docker Hub, and deployed to a DigitalOcean droplet.

#### **Pipeline Steps**
1. **Checkout Code from GitHub**
   - Retrieves the latest code from the repository.

2. **Install Dependencies & Build Backend**
   - Installs necessary backend dependencies using `npm install`.
   - Ensures the backend is ready for deployment.

3. **Install Dependencies & Build Frontend**
   - Installs frontend dependencies and builds the project.
   - Ensures static assets are compiled and optimized.

4. **Build Backend Docker Image**
   - Creates a Docker image for the backend service based on a custom `Dockerfile.backend`.

5. **Build Frontend Docker Image**
   - Builds a separate Docker image for the frontend service using `Dockerfile.frontend`.

6. **Push Backend Image to Docker Hub**
   - Logs into Docker Hub and pushes the backend image to a remote repository.

7. **Push Frontend Image to Docker Hub**
   - Similarly, pushes the frontend image to Docker Hub for deployment.

8. **Deploy Application on DigitalOcean**
   - Uses SSH to copy the `docker-compose.yml` file to the DigitalOcean droplet.
   - Ensures Docker and Docker-Compose are installed on the remote server.
   - Stops any running containers and deploys the updated application using Docker Compose.

## Justifications for the DevOps Choices
- **Containerization (Docker)**: Allows for consistency between development and production environments.
- **Docker-Compose**: Simplifies multi-container application management.
- **Jenkins CI/CD**: Automates the deployment process, reducing manual work and potential errors.

## Future Improvements
- Implement Kubernetes for better container orchestration at scale.
- Enhance monitoring and logging using Prometheus and Grafana.
- Automate rollbacks in case of deployment failures.
- Integrate automated tests into the CI/CD pipeline.
