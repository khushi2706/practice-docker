# React App with Docker

This project demonstrates how to containerize a React application using Docker. Below is an overview of the Docker architecture used in this project.

## Docker Architecture

### 1. Multi-Stage Dockerfile
The `Dockerfile` is designed with two stages:
- **Builder Stage**: 
  - Uses the `node:16-alpine` image to install dependencies and build the React app.
  - The build artifacts are stored in the `/app/build` directory.
- **Production Stage**:
  - Uses the `nginx` image to serve the static files generated in the builder stage.
  - The build artifacts are copied to the Nginx default directory (`/usr/share/nginx/html`).

### 2. Docker Compose
The `docker-compose.yml` file defines two services:
- **web**:
  - Builds the development environment using `dockerfile.dev`.
  - Maps port `3001` on the host to port `3000` in the container.
  - Uses volume mounts for live code updates during development.
- **tests**:
  - Runs the test suite using the same development environment.
  - Shares the same volume mounts as the `web` service.

### 3. Development Workflow
- Use `docker-compose up` to start the development environment.
- Code changes are reflected in real-time due to volume mounts.
- Run tests using the `tests` service.

### 4. Production Workflow
- Build the production image using the `Dockerfile`.
- Run the containerized app with Nginx serving the static files.

## Commands

### Build and Run in Development
```bash
docker-compose up
```

### Run Tests
```bash
docker-compose run tests
```

### Build Production Image
```bash
docker build -t react-app-prod .
```

### Run Production Container
```bash
docker run -p 80:80 react-app-prod
```

## Notes
- Ensure Docker and Docker Compose are installed on your system.
- The `dockerfile.dev` is used for development purposes, while the `Dockerfile` is optimized for production.
