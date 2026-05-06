# INT332: DevOps Practical Lab Manual

This document contains all the practical tasks, commands, and configurations covered in the INT332 course.

---

## Practical 1: Installing Docker & Basic Commands
**Objective:** Install Docker, understand Hyper-V (Windows), and execute basic container lifecycle commands.

### 1. Installation (Ubuntu/Linux)
```bash
# Update package database
sudo apt update

# Install required dependencies
sudo apt install apt-transport-https ca-certificates curl software-properties-common

# Add Docker’s official GPG key
curl -fsSL [https://download.docker.com/linux/ubuntu/gpg](https://download.docker.com/linux/ubuntu/gpg) | sudo apt-key add -

# Add Docker repository
sudo add-apt-repository "deb [arch=amd64] [https://download.docker.com/linux/ubuntu](https://download.docker.com/linux/ubuntu) $(lsb_release -cs) stable"

# Install Docker CE
sudo apt update
sudo apt install docker-ce

# Verify installation
docker --version
# Run a test container
docker run hello-world

# Run an interactive Ubuntu container
docker run -it ubuntu /bin/bash

# List running containers
docker ps

# List all containers (including stopped ones)
docker ps -a

# Stop a container
docker stop <container_id>

# Remove a stopped container
docker rm <container_id>
# Run a container and make a change (e.g., create a file)
docker run -it --name my-ubuntu ubuntu /bin/bash
touch /home/myfile.txt
exit

# Commit the changes to create a new custom image
docker commit my-ubuntu my-custom-ubuntu:v1
# Login to your Docker Hub account
docker login

# Tag the image with your Docker Hub username
docker tag my-custom-ubuntu:v1 <your_username>/my-custom-ubuntu:v1

# Push the image to the registry
docker push <your_username>/my-custom-ubuntu:v1
# Use an official Python runtime as a parent image
FROM python:3.9-slim

# Set the working directory in the container
WORKDIR /app

# Create a simple Python script
RUN echo 'print("Hello from INT332 Dockerfile!")' > script.py

# Run the script when the container launches
CMD ["python", "script.py"]
# Build the image (don't miss the dot at the end)
docker build -t my-python-app .

# Run the image
docker run my-python-app
# Create a volume
docker volume create int332-data

# Run a container and mount the volume
docker run -d --name db-container -v int332-data:/var/lib/mysql mysql:latest

# Inspect the volume
docker volume inspect int332-data
# Start a backend database container
docker run -d --name my-redis redis

# Legacy method: Link a frontend container to the database
docker run -it --name my-app --link my-redis:redis ubuntu /bin/bash

# Inside the ubuntu container, you can now ping 'redis'
ping redis
sudo curl -L "[https://github.com/docker/compose/releases/download/v2.20.2/docker-compose-$(uname](https://github.com/docker/compose/releases/download/v2.20.2/docker-compose-$(uname) -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
version: '3.8'

services:
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wp_user
      MYSQL_PASSWORD: wp_password

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8000:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wp_user
      WORDPRESS_DB_PASSWORD: wp_password
      WORDPRESS_DB_NAME: wordpress

volumes:
  db_data:
# Start the application in the background
docker-compose up -d

# Check running services
docker-compose ps

# Access WordPress via http://localhost:8000 in your browser.

# Bring down the application
docker-compose down
# Initialize Docker Swarm (makes this node a Manager)
docker swarm init

# Create a service with 3 replicas
docker service create --name web-cluster --replicas 3 -p 8080:80 nginx

# List running services
docker service ls

# See the tasks (containers) running for the service
docker service ps web-cluster

# Scale the service up to 5 replicas
docker service scale web-cluster=5

# Remove the service and leave swarm
docker service rm web-cluster
docker swarm leave --force
# Create a custom bridge network
docker network create my-custom-net

# Run two containers attached to this network
docker run -d --name app1 --network my-custom-net busybox sleep 3000
docker run -d --name app2 --network my-custom-net busybox sleep 3000

# Test connection (DNS resolution works automatically on custom networks)
docker exec -it app1 ping app2

# Inspect network details
docker network inspect my-custom-net
# Pull and run the Jenkins LTS image with Docker socket access (to run Docker inside Jenkins)
docker run -d -p 8080:8080 -p 50000:50000 --name jenkins-master -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts

# Retrieve the initial admin password
docker exec jenkins-master cat /var/jenkins_home/secrets/initialAdminPassword
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from source control...'
                // git '[https://github.com/Hamza8470/INT332-Devops.git](https://github.com/Hamza8470/INT332-Devops.git)'
            }
        }
        stage('Build') {
            steps {
                echo 'Building the application...'
            }
        }
        stage('Test') {
            steps {
                echo 'Running unit tests...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying to staging environment...'
            }
        }
    }
    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed. Check logs.'
        }
    }
}
