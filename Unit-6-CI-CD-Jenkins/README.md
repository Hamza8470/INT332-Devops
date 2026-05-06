# Unit VI: CI/CD with Jenkins

## 1. Jenkins Foundations & Architecture
Jenkins is an open-source automation server used to build, test, and deploy software. 
*   **Master/Agent (Controller/Node) Model:** Jenkins distributes workloads. The **Master (Controller)** serves the UI, manages configuration, and schedules jobs. The **Agents (Nodes)** are separate machines (or containers) that execute the actual build jobs. This prevents the master from being overwhelmed.
*   **Plugins:** Jenkins is highly extensible. Almost all functionality (Git integration, Docker builds, Slack notifications) is provided by plugins installed via the UI.
*   **Security:** Jenkins allows setting up Users, Roles, and Role-Based Access Control (RBAC) to restrict who can trigger or configure pipelines.

## 2. Jenkins Pipelines
A Pipeline defines your entire build process, typically broken down into stages.
*   **Freestyle Jobs:** The traditional way to use Jenkins. Configurations are made entirely through the web UI. They are hard to version control and scale.
*   **Pipeline Jobs:** Defines the pipeline as code in a file called a `Jenkinsfile`. This file is committed to version control alongside the application code.

### Pipeline Syntax
*   **Declarative Pipeline:** The modern, recommended approach. It uses a strict, predefined structure (like `pipeline { agent any ... }`) making it easier to read and maintain.
*   **Scripted Pipeline:** The older approach using raw Groovy code. It offers maximum flexibility but can become complex and difficult to maintain.

### Pipeline Structure
*   **`agent`:** Defines where the pipeline (or a specific stage) will run.
*   **`stages`:** A block containing a sequence of one or more stage directives.
*   **`steps`:** The actual commands executed within a stage (e.g., `sh 'mvn clean package'`).
*   **`post`:** Actions to run at the end of the pipeline or stage, based on the outcome (e.g., `success`, `failure`, `always`).

## 3. Docker and Maven Integration in Jenkins
*   **Maven Integration:** Jenkins can automatically install Maven via "Global Tool Configuration." In your pipeline, you reference this tool to run standard `mvn` commands. You can also configure plugins to parse Maven code coverage and test reports (e.g., Jacoco or Surefire reports) directly in the Jenkins UI.
*   **Docker Inside Jenkins Agents:** Jenkins can use Docker containers as ephemeral build agents. Instead of installing Maven, Node, or Python on the Jenkins machine, the pipeline specifies a Docker image as the agent (e.g., `agent { docker { image 'maven:3.8-eclipse-temurin-17' } }`). Jenkins spins up the container, runs the build steps inside it, and destroys it afterward, ensuring clean, reproducible builds.

## 4. CI/CD Deployment Flows
*   **Triggering Builds:** Pipelines can be triggered manually, via a schedule, by polling the SCM (checking Git every X minutes), or dynamically via Webhooks (GitHub notifies Jenkins instantly when a push occurs).
*   **Deployments:** The final stage of a pipeline often involves moving the built artifact or Docker image to a server. This can be done via SSH plugins, pushing to a cloud provider's API, or interacting with a container orchestration tool like Docker Swarm or Kubernetes.
