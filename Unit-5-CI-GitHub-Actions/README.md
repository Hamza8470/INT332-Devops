# Unit V: Continuous Integration (CI) with GitHub Actions

## 1. Understanding Workflow Automation
GitHub Actions is a CI/CD platform natively integrated into GitHub. It allows you to automate your software development lifecycle, from building and testing code to deploying applications, directly from your repository.

### Key Components
*   **Workflows:** The top-level automated process defined in a YAML file. Must be stored in the `.github/workflows/` directory.
*   **Events/Triggers:** The specific activity that triggers the workflow. Examples include `push` (code is pushed to a branch), `pull_request` (a PR is opened or updated), `schedule` (cron jobs), or `workflow_dispatch` (manual execution).
*   **Jobs:** A set of logical steps that execute on the same runner. Workflows can have multiple jobs. By default, multiple jobs run in parallel, but you can define dependencies using the `needs` keyword.
*   **Steps:** Individual tasks within a job. A step can either run a shell command or use an action.
*   **Actions:** Reusable, standalone commands that perform a specific task (like checking out code or setting up Node.js). They can be custom-built or sourced from the GitHub Marketplace.
*   **Runners:** The server that executes the workflow. Can be GitHub-hosted (managed by GitHub, spun up fresh for each job) or Self-hosted (your own machines registered with GitHub for more control or security).

## 2. Advanced Workflow Strategies
*   **Matrix Strategies:** Allows you to use variables in a single job definition to automatically create multiple job runs that are based on the combinations of the variables. For example, testing an application across Node.js versions 16, 18, and 20 simultaneously.
*   **Caching:** Downloading dependencies (like Maven's `~/.m2` or Node's `node_modules`) takes time. The `actions/cache` action saves these dependencies to GitHub's infrastructure and restores them in subsequent runs, drastically speeding up build times.
*   **Secrets & Security:** Sensitive data like Docker Hub passwords or API keys are stored in GitHub Secrets and injected into workflows as environment variables.

## 3. Docker & GitHub Actions Integration
A highly common CI use case is automatically building and distributing Docker containers.
*   **Building:** Using the `docker/build-push-action`, the workflow can trigger a `docker build` based on your repository's Dockerfile.
*   **Pushing:** Workflows can authenticate with Docker Hub or the GitHub Container Registry (GHCR) and securely push the tagged image.
*   **Deployments:** Once pushed, a final job can SSH into a remote cloud server and trigger a `docker pull` and `docker run` to deploy the new image.
