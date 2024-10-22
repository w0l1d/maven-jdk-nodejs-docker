Here is the corrected README along with the `Dockerfile` and project structure for your multi-version Docker image project.

---

# Multi-Version Docker Images for Maven, JDK, and Node.js

This repository provides multiple Dockerfiles for building custom Docker images that include different versions of Maven, JDK, and Node.js. These images are suitable for various development environments and CI/CD pipelines.

## Table of Contents

- [Overview](#overview)
- [Available Images](#available-images)
- [Project Structure](#project-structure)
- [Docker Build Instructions](#docker-build-instructions)
- [How to Use](#how-to-use)
- [Customization](#customization)
- [Contributing](#contributing)
- [License](#license)

## Overview

This project contains Dockerfiles that allow developers to easily create Docker images containing combinations of:

- **Maven**: To manage project dependencies and build Java applications.
- **JDK (Java Development Kit)**: To compile and run Java programs.
- **Node.js**: To run JavaScript code and develop Node.js applications.
- **NPM**: To manage Node.js dependencies.
- **Yarn**: To manage Node.js dependencies.

Each Dockerfile is tailored for a specific combination of Maven, JDK, and Node.js versions, providing flexibility to select the ideal environment for different use cases.

## Available Images

The following versions are supported in the provided Dockerfiles:

- **Maven**: 3.9.9
- **JDK**: Eclipse Temurin 17
- **Node.js**: 20

### Image Variants

- `maven3.9.9-eclipse-temurin17-node20` - Maven 3.9.9, JDK Eclipse Temurin 17, Node.js 20, yarn

Each combination is defined in its own Dockerfile, allowing users to choose the appropriate setup for their project.

## Project Structure

The repository follows a structured format where each combination of versions has its own Dockerfile in the `dockerfiles/` directory.

```
├── dockerfiles/
│   ├── Dockerfile.maven3.9.9-eclipse-temurin17-node20
│   └── Dockerfile.<version-combination>
├── README.md
└── LICENSE
```

### Dockerfile Example

```Dockerfile
# Dockerfile.maven3.9.9-eclipse-temurin17-node20
FROM eclipse-temurin:17-jdk

# Install Maven
ENV MAVEN_VERSION=3.9.9
RUN apt-get update && \
    apt-get install -y wget tar && \
    wget https://archive.apache.org/dist/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz && \
    tar xzvf apache-maven-${MAVEN_VERSION}-bin.tar.gz -C /opt && \
    ln -s /opt/apache-maven-${MAVEN_VERSION}/bin/mvn /usr/bin/mvn

# Install Node.js
ENV NODE_VERSION=20
RUN apt-get install -y curl && \
    curl -fsSL https://deb.nodesource.com/setup_${NODE_VERSION}.x | bash - && \
    apt-get install -y nodejs

# Set environment variables
ENV MAVEN_HOME=/opt/apache-maven-${MAVEN_VERSION}
ENV PATH=$MAVEN_HOME/bin:$PATH

# Verify installations
RUN java -version && mvn -version && node -v && npm -v

CMD ["/bin/bash"]
```

## Docker Build Instructions

To build an image from a specific Dockerfile, navigate to the root directory and run:

```bash
docker build -t <image-name> -f dockerfiles/Dockerfile.<version-combination> .
```

For example, to build an image containing Maven 3.9.9, JDK Eclipse Temurin 17, and Node.js 20:

```bash
docker build -t maven-jdk-node:maven3.9.9-eclipse-temurin17-node20 -f dockerfiles/Dockerfile.maven3.9.9-eclipse-temurin17-node20 .
```

## How to Use

Once the image is built, you can use it in your development environment or CI/CD pipelines.

### Running the Image

To run the image interactively:

```bash
docker run -it <image-name> /bin/bash
```

For example:

```bash
docker run -it maven-jdk-node:maven3.9.9-eclipse-temurin17-node20 /bin/bash
```

### Integrating with CI/CD Pipelines

You can integrate these Docker images into your CI/CD pipeline by specifying the appropriate image tag in your pipeline configuration.

For example, in GitLab CI:

```yaml
image: maven-jdk-node:maven3.9.9-eclipse-temurin17-node20
```

## Customization

You can modify the Dockerfiles to use different versions of Maven, JDK, or Node.js by updating the relevant lines in each Dockerfile.

For example, to change the JDK version:

```dockerfile
FROM eclipse-temurin:11-jdk
```

You can also install additional dependencies or make other customizations as needed.

## Contributing

We welcome contributions to improve or extend this project. If you'd like to add support for additional versions or tools, feel free to submit a pull request.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

---

### Additional Project Structure

- `README.md`: This file contains documentation on the project and instructions on how to use the Dockerfiles.
- `LICENSE`: The licensing terms for this repository.
- `dockerfiles/`: This folder contains all the Dockerfiles for different combinations of Maven, JDK, and Node.js versions.
