# Node.js CI/CD Pipeline with Jenkins and Docker

An automated CI/CD pipeline for a simple Node.js Express application using Jenkins, GitHub Webhooks, and Docker.

## Overview

This project demonstrates how code changes pushed to GitHub can automatically trigger a Jenkins pipeline that tests the application, builds a Docker image, and deploys the updated container locally.

## Architecture

```text
Developer Push
      |
      v
GitHub Repository
      |
      v
GitHub Webhook
      |
      v
Jenkins Pipeline
      |
      +---- Install Dependencies (npm ci)
      |
      +---- Run Tests (npm test)
      |
      +---- Build Docker Image
      |
      +---- Deploy Container
                    |
                    v
             Express App
             localhost:3000
```

## Tech Stack

- **Node.js** — JavaScript runtime
- **Express.js** — Web application framework
- **Node.js Test Runner** — Automated testing
- **Git and GitHub** — Version control and source hosting
- **Jenkins** — CI/CD pipeline automation
- **GitHub Webhooks** — Automatic pipeline triggers
- **Docker** — Containerization and deployment
- **Linux (Zorin OS)** — Local development and CI/CD environment

## Features

- Automatically triggers Jenkins builds on GitHub pushes.
- Installs dependencies using `npm ci`.
- Runs automated tests using `npm test`.
- Builds a Docker image after successful tests.
- Replaces the running application container with the new version.
- Exposes the application on port `3000`.

## Project Structure

```text
node-app-jenkins/
├── app.js
├── package.json
├── package-lock.json
├── Jenkinsfile
├── Dockerfile
├── .gitignore
├── test/
│   └── app.test.js
└── README.md
```

## Run Locally

### Prerequisites

- Node.js and npm
- Git
- Docker (for containerized deployment)

### Install dependencies

```bash
npm ci
```

### Run tests

```bash
npm test
```

### Start the application

```bash
npm start
```

Open `http://localhost:3000` in your browser.

The application also provides a health endpoint at `/health`.

## CI/CD Pipeline

The Jenkins pipeline performs the following stages:

1. **Checkout:** Retrieves the source code from GitHub.
2. **Install Dependencies:** Installs packages using `npm ci`.
3. **Test:** Runs the automated test suite.
4. **Build Docker Image:** Builds the image `node-app-jenkins:latest`.
5. **Deploy:** Replaces the existing container and starts the updated application.

The pipeline stops if an earlier stage fails, preventing deployment after a failed test or image build.

## Docker

Build the image manually:

```bash
docker build -t node-app-jenkins:latest .
```

Run the container:

```bash
docker run -d \
  --name node-app-jenkins \
  --restart unless-stopped \
  -p 3000:3000 \
  node-app-jenkins:latest
```

Access the application at `http://localhost:3000`.

## Jenkins Setup

1. Install Jenkins and configure the required plugins.
2. Create a Pipeline job named `node-app-ci`.
3. Configure the job to load the `Jenkinsfile` from the GitHub repository.
4. Enable **GitHub hook trigger for GITScm polling**.
5. Configure the repository webhook to point to the Jenkins `/github-webhook/` endpoint.
6. Ensure the Jenkins service can access Node.js, npm, Git, and Docker.
7. Push a commit to GitHub and verify that Jenkins builds and deploys automatically.

For local development, a temporary HTTPS tunnel can make a laptop-hosted Jenkins instance reachable by GitHub. The tunnel must remain active for webhook delivery.

## Verification

A successful pipeline should demonstrate:

- Jenkins detects a GitHub push.
- Dependencies install successfully.
- Automated tests pass.
- The Docker image builds successfully.
- The application container starts and responds on port `3000`.

## Limitations

This project is a local learning implementation. It does not include a production container registry, deployment to a remote server, automated rollback, or comprehensive production health checks.

The deployment replaces the existing container and is not designed for zero-downtime releases.

## Future Improvements

- Add Docker health checks and deployment verification.
- Add safer rollback handling.
- Tag images with Git commit hashes.
- Push images to Docker Hub or another container registry.
- Deploy to a remote server or cloud platform.
- Add notifications for failed builds and deployments.

A hands-on DevOps project exploring automated CI/CD with Jenkins, GitHub Webhooks, and Docker.
