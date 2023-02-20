## This is JFrog Interview Task
---
# Instructions
## 1. Build and Run the Spring Application (How to Run the Project)
```sh
docker build -t springpetclinic:latest .
docker run -p 127.0.0.1:8080:8080 springpetclinic:latest
```
The application will run in port 8080
- open http://localhost:8080
- Dockerfile is used to build and run the application with exposed port 8080
- Dockerfile.test is used to create container that build and run tests

## 2. Trigger Jenikins Pipeline
- Any commit to the main branch of this repo will trigger a pipeline.
- Pipeline uses Jenkinsfile in this repo
    - Stage 1 - Compile and Run tests
    - Stage 2 - Build the image and push to JFrog Artifactory
- Jenkins Server - http://projectjfrog.com.
- Use credentials shared in the email to login to Jenkins Application.

## 3. Jenkins Server
link to jenkins app repo - https://github.com/mansiboriya/jenkins-app
- build using docker, can run in ubuntu, mac, hopefully windows
- docker-compose consists of 2 containers
    - container 1 is docker running as container. like inception(docker inside docker)
    - container 2 is jenkins app which uses docker container mentioned above for build stages

## 4. Pull Docker image from JFrog Artifactory and run
1. Docker login to JFrog Artifactory
```sh
docker login -u mansiboriya@gmail.com -p {JFROG_ARTIFACTORY_TOKEN_FROM_EMAIL} projectjfrog.jfrog.io
```
2. Pull the image from JFrog Artifactory
```sh
docker pull http://projectjfrog.jfrog.io/docker-local/springpetclinic:latest
```
3. Run the image
```sh
docker run -p 127.0.0.1:8080:8080 http://projectjfrog.jfrog.io/docker-local/springpetclinic:latest
```

## Notes
Required Credential
- Jenkins (http://projectjfrog.com)
    - Username
    - Password
- JFrog Artifactory
    - Token/Password
