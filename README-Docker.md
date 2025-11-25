Docker image build / push notes

- Dockerfile: uses OpenJDK 17 slim image and copies the packaged jar from `target/`.
- Image name (example): `docker.io/<dockerhub-username>/student-management`.

Jenkins setup notes:

1. Create a Jenkins credential of type "Username with password" with ID `dockerhub-creds` containing your Docker Hub username and password (or token).
2. Configure the Jenkins job to use "Pipeline script from SCM" and the `Jenkinsfile` in this repo.
3. Ensure the Jenkins agent has Docker installed and can run Docker commands, or use a Docker-enabled agent (Docker socket access).

Pipeline will:
- Package the application (`./mvnw -B package`).
- Build a Docker image from the `Dockerfile`.
- Tag it as `${IMAGE_NAME}:${BUILD_NUMBER}` and `${IMAGE_NAME}:latest`.
- Login to Docker Hub using credentials bound from Jenkins and push the image.

If your Jenkins agents cannot run Docker, configure a Docker build node or use a remote Docker registry build service.
