# Unit III: Microservices with Docker Compose

## 1. Microservices Architecture
### Monolithic vs. Microservices
*   **Monolithic Architecture:** The entire application (UI, business logic, data access) is built as a single, unified codebase and deployed as a single unit. If one component fails or needs scaling, the entire application must be replicated.
*   **Microservices Architecture:** The application is broken down into small, independent services that communicate over a network (usually HTTP/REST). 

### Advantages of Microservices
*   **Scalability:** You can scale only the services that experience high traffic, saving resources.
*   **Isolation:** A crash in one service (e.g., a background worker) does not bring down the entire application.
*   **Agility:** Different teams can write services in different programming languages and deploy them independently.
*   **API Gateway:** Acts as a single entry point for clients, routing requests to the appropriate microservice, handling authentication, and load balancing.

## 2. Docker Compose
Docker Compose is a tool used to define and run multi-container Docker applications. Instead of running multiple `docker run` commands manually, you define your entire stack in a single YAML file.

### YAML Structure (`docker-compose.yml`)
*   **`version`:** Specifies the Compose file format version (e.g., `version: '3.8'`).
*   **`services`:** The core block where you define your containers (e.g., `backend`, `database`).
*   **`image` / `build`:** Tells Compose whether to pull an existing image or build one from a `Dockerfile`.
*   **`ports`:** Maps host ports to container ports.
*   **`environment`:** Passes environment variables into the container (crucial for database passwords and configuration).
*   **`volumes`:** Defines named volumes for persistent data.
*   **`depends_on`:** Controls the startup order. If the `backend` depends on the `database`, Compose will start the database first.

## 3. Use Case Deployment: Full Stack Application
In a typical MERN stack deployment (Node.js + MongoDB):
1.  The `database` service uses the official `mongo` image and mounts a volume to persist the database files.
2.  The `backend` service uses a custom Node.js image, exposes port 5000, and is connected to the database via an environment variable `MONGO_URI=mongodb://database:27017`. Because they are in the same Compose file, they share a network and can resolve each other by service name (`database`).
