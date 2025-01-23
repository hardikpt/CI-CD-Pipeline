# CI/CD Pipeline Overview

This repository demonstrates the setup of a CI/CD pipeline using **Jenkins**, **Docker**, **AWS EC2**, and **GitHub**. Below is a detailed overview of the components and their interactions, as illustrated in the architecture diagram.

## Architecture

### Key Components

1. **AWS EC2 Instance**
   - An Ubuntu-based EC2 instance serves as the primary server.
   - Jenkins and Docker are installed and configured on this instance.
   - Acts as the central hub for the CI/CD pipeline operations.

2. **GitHub**
   - Serves as the source code repository.
   - Code changes are pushed here and automatically trigger the pipeline.
   - Ensures version control and collaborative development.

3. **Jenkins**
   - Installed on the EC2 instance.
   - Orchestrates the entire CI/CD pipeline.
   - Listens to changes in the GitHub repository and triggers builds.
   - Executes shell commands for Docker operations.

4. **Docker**
   - Installed on the EC2 instance.
   - Used for containerization of the application.
   - Builds Docker images and runs Docker containers.
   - Provides consistent and isolated testing environments.

### Detailed Workflow

1. **GitHub Credentials and Repository Setup**
   - GitHub credentials are configured in Jenkins.
   - The repository is cloned into the Linux VM (Ubuntu-based EC2 instance).
   - Ensures the latest code is always available for the pipeline.

2. **Run Node.js Application**
   - The Node.js application is run manually from the Linux VM to verify the initial setup.
   - Confirms that the code is functional before automation.

3. **Testing in Different Environments Using Docker**
   - Docker is initially used to manually build and run the application.
     - **Manual Docker Commands**:
       ```bash
       docker build . -t my-node-app-todo
       docker run -d --name my-node-app-container -p 8000:8000 my-node-app-todo
       ```
   - After testing the manual process, Jenkins is used to automate the Docker build and run steps.

4. **Automating Docker with Jenkins**
   - Jenkins pipeline is configured to automate the Docker commands using shell scripts.
     - **Jenkins Shell Commands**:
       ```bash
       docker build . -t my-node-app-todo
       docker run -d --name my-node-app-container -p 8000:8000 my-node-app-todo
       ```
   - Reduces manual effort and ensures consistent builds.

5. **Process Automation**
   - The entire process, from pulling code from GitHub to deploying the Node.js application in Docker containers, is automated using Jenkins.
   - Improves efficiency and minimizes human intervention.

6. **Access Jenkins and Application**
   - Jenkins is accessible via a public IP (e.g., `54.159.144.81:8080`).
   - The application can be accessed via the Docker container on port 8000 (`http://<PUBLIC_IP>:8000`).

### Key Details

- Jenkins handles both the Node.js application deployment and Docker containerization processes.
- Docker ensures the application is tested in isolated environments, enabling consistent testing across different environments.
- GitHub serves as the single source of truth for the application code.

## How to Use

1. **Setup AWS EC2 Instance**
   - Launch an Ubuntu EC2 instance.
   - Install Jenkins and Docker.

2. **Configure Jenkins**
   - Add GitHub credentials to Jenkins.
   - Connect Jenkins to your GitHub repository.

3. **Run Node.js Application**
   - Clone the GitHub repository into the Linux VM.
   - Run the Node.js application manually to verify.

4. **Build and Run Docker Containers Manually**
   - Use the following commands to build and run the Docker container:
     ```bash
     docker build . -t my-node-app-todo
     docker run -d --name my-node-app-container -p 8000:8000 my-node-app-todo
     ```

5. **Automate the Process**
   - Configure a Jenkins pipeline to automate the Docker build and run steps using shell commands.

6. **Test the CI/CD Pipeline**
   - Push code changes to the GitHub repository.
   - Verify that Jenkins automatically builds and deploys the application in Docker containers.

## Prerequisites

- **AWS Account**: Required to launch and manage EC2 instances.
- **GitHub Account**: To host your repository.
- **Basic Knowledge of Jenkins, Docker, and Node.js**.

## Advanced Configuration

1. **Dockerfile Setup**
   - Create a `Dockerfile` to define the build instructions for the Node.js application:
     ```dockerfile
     FROM node:14
     WORKDIR /usr/src/app
     COPY package*.json ./
     RUN npm install
     COPY . .
     EXPOSE 8000
     CMD [ "npm", "start" ]
     ```

2. **Jenkins Pipeline Script**
   - Use a Jenkinsfile to define the pipeline steps:
     ```groovy
     pipeline {
         agent any
         stages {
             stage('Clone Repository') {
                 steps {
                     git branch: 'main', url: 'https://github.com/your-repo.git'
                 }
             }
             stage('Build Docker Image') {
                 steps {
                     sh 'docker build -t my-node-app-todo .'
                 }
             }
             stage('Run Docker Container') {
                 steps {
                     sh 'docker run -d --name my-node-app-container -p 8000:8000 my-node-app-todo'
                 }
             }
         }
     }
     ```

3. **Environment Variables**
   - Define environment variables in Jenkins for credentials and configuration to avoid hardcoding sensitive data.

## Future Enhancements

- Add support for multiple environments (e.g., dev, staging, production).
- Implement monitoring and alerting for the pipeline.
- Use Docker Compose for managing multi-container applications.
- Integrate testing frameworks for automated unit and integration tests within the pipeline.
- Enhance security with role-based access control and secrets management in Jenkins.

---

Feel free to contribute or raise issues if you encounter any problems!

