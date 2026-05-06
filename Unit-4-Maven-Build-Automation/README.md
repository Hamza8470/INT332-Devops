# Unit IV: Maven Build Automation

## 1. Why Build Tools Exist
Before automated build tools, developers had to manually compile code, download dependencies (JAR files), run tests, and package the application. This manual process was error-prone, difficult to reproduce across different environments, and hard to manage as projects grew. Build tools solve these problems by providing a standardized, automated, and repeatable process.

## 2. Project Object Model (POM) & Directory Structure
The `pom.xml` file is the heart of a Maven project. It contains information about the project, configuration details, and the dependencies required to build the application.

**Standard Directory Structure:**
*   `src/main/java/`: Application source code.
*   `src/main/resources/`: Configuration files and static resources.
*   `src/test/java/`: Unit test source code.
*   `target/`: The output directory where Maven places the compiled classes and final packages (e.g., `.jar` or `.war`).

## 3. The Maven Build Lifecycle
Maven uses a predefined sequence of phases to build a project. Executing a phase automatically executes all preceding phases in that lifecycle.
1.  **`validate`**: Checks if the project structure is correct and all necessary information is available.
2.  **`compile`**: Compiles the source code (`src/main/java`).
3.  **`test`**: Runs the unit tests using a suitable testing framework (e.g., JUnit). These tests should not require the code to be packaged or deployed.
4.  **`package`**: Takes the compiled code and packages it in its distributable format, such as a JAR or WAR.
5.  **`verify`**: Runs any checks to verify the package is valid and meets quality criteria (often used for integration tests).
6.  **`install`**: Installs the package into the local Maven repository (typically `~/.m2/repository`), making it available as a dependency for other local projects.
7.  **`deploy`**: Done in an integration or release environment, copies the final package to a remote repository for sharing with other developers and projects.

## 4. Dependency Management
*   **Dependency Scope:** Controls when a dependency is available (e.g., `compile` (default), `test` (only for testing), `provided` (expected to be provided by the runtime environment)).
*   **Transitive Dependencies:** If Project A depends on B, and B depends on C, Maven automatically pulls in C when you declare a dependency on B.
*   **Version Conflicts:** If transitive dependencies require different versions of the same library, Maven uses "dependency mediation" (usually picking the version closest to your project in the dependency tree). You can resolve conflicts manually using `<dependencyManagement>` or `<exclusions>`.
*   **Parent POM:** Allows you to define shared configurations, dependencies, and plugin versions across multiple child projects.

## 5. Maven Plugins & Execution
Maven is essentially a plugin execution framework; plugins do all the actual work.
*   **Compiler Plugin:** Compiles the Java source code.
*   **Surefire Plugin:** Executes unit tests during the `test` phase.
*   **Shade Plugin:** Creates an "uber-jar" or "fat-jar" that packages the project along with all of its dependencies into a single runnable JAR file.
*   **Maven Wrapper (`mvnw`):** A script that ensures anyone building the project uses the exact same version of Maven, downloading it automatically if necessary.

## 6. Maven and Docker Integration
You can automate the creation of Docker images directly from your Maven build using plugins like the `dockerfile-maven-plugin` (or Google's `Jib`).
*   **Workflow:** Code is compiled -> Packaged into a JAR -> The Maven plugin triggers the Docker daemon -> Copies the JAR into the Docker image -> Builds the image -> Pushes the artifact to a registry (Docker Hub/GHCR).
