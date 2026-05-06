# Unit II: Image Building & Container Management

## 1. Dockerfile Core Concepts
A `Dockerfile` is a script consisting of successive commands to build a Docker image.
*   **Build Context:** When you run `docker build`, the current directory is the build context. Docker sends all files in this directory to the Docker daemon.
*   **`.dockerignore`:** Similar to `.gitignore`, this file tells Docker which files and directories to exclude from the build context (e.g., `node_modules`, `.git`), speeding up the build process and reducing image size.

## 2. Basic Dockerfile Instructions
*   `FROM`: Defines the base image (must be the first instruction). Example: `FROM node:18-alpine`
*   `RUN`: Executes commands inside the container during the build process (e.g., `RUN apt-get update`). Every `RUN` creates a new image layer.
*   `COPY`: Copies files from the host machine into the container filesystem.
*   `ADD`: Similar to `COPY`, but can also extract tar files and download files from URLs. (Prefer `COPY` for simple file transfers).
*   `WORKDIR`: Sets the current working directory for subsequent instructions.
*   `ENV`: Sets environment variables.
*   `EXPOSE`: Documents the ports that the container will listen on at runtime (does not actually publish the port).
*   `CMD`: Specifies the default command to run when a container starts. Can be overridden from the CLI.
*   `ENTRYPOINT`: Configures a container to run as an executable. Cannot be easily overridden from the CLI.

## 3. Docker Networking
Docker provides several network drivers:
*   **Bridge Network:** The default network. Containers on the same bridge network can communicate using IP addresses or container names (DNS resolution). It isolates containers from the host network.
*   **Host Network:** Removes network isolation. The container shares the host's networking namespace (e.g., port 80 in the container is port 80 on the host).
*   **Overlay Network:** Used in Docker Swarm to connect multiple Docker daemons across different host machines.
*   **Port Mapping:** Used to expose a container's internal port to the host machine. (e.g., `-p 8080:80` maps port 80 inside the container to 8080 on the host).

## 4. Docker Storage
Containers are ephemeral; when they are deleted, their writable layer is lost. To persist data, Docker uses two main mechanisms:
*   **Volumes:** Managed entirely by Docker. They are stored in a part of the host filesystem (`/var/lib/docker/volumes/`). They are the safest and preferred way to persist data.
*   **Bind Mounts:** Maps a specific file or directory from the host machine into the container. The container can modify the host filesystem directly.

## 5. Registries and Authentication
*   **Docker Hub:** The default public registry.
*   **GitHub Container Registry (GHCR):** Integrated with GitHub, allowing you to store images alongside your source code.
*   **Authentication:** Private registries require authentication. Access tokens (Personal Access Tokens - PATs) are preferred over passwords for CI/CD pipelines to ensure secure, scoped access.
