# Installing Jenkins in Docker
Windows

The Jenkins project provides a Linux container image, not a Windows container image. Be sure that your Docker for Windows installation is configured to run Linux Containers rather than Windows Containers. See the Docker documentation for instructions to switch to Linux containers. Once configured to run Linux Containers, the steps are:
1. Open up a command prompt window and similar to the macOS and Linux instructions above do the following:

* Create a bridge network in Docker
```bash 
docker network create jenkins
```
* Run a docker:dind Docker image
```bash
docker run --name jenkins-docker --rm --detach `
  --privileged --network jenkins --network-alias docker `
  --env DOCKER_TLS_CERTDIR=/certs `
  --volume jenkins-docker-certs:/certs/client `
  --volume jenkins-data:/var/jenkins_home `
  docker:dind
  ```
2. Customise official Jenkins Docker image, by executing below two steps:

* Create Dockerfile with the following content:
```bash
FROM jenkins/jenkins:2.319.1-jdk11
USER root
RUN apt-get update && apt-get install -y lsb-release
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
  https://download.docker.com/linux/debian/gpg
RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get update && apt-get install -y docker-ce-cli
USER jenkins
RUN jenkins-plugin-cli --plugins "blueocean:1.25.2 docker-workflow:1.26"
```
3. Build a new docker image from this Dockerfile and assign the image a meaningful name, e.g. "myjenkins-blueocean:1.1":

```bash
docker build -t myjenkins-blueocean:1.1 .
```
Keep in mind that the process described above will automatically download the official Jenkins Docker image if this hasn’t been done before.

4. Run your own myjenkins-blueocean:1.1 image as a container in Docker using the following docker run command:
```bash
docker run --name jenkins-blueocean --rm --detach `
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 `
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 `
  --volume jenkins-data:/var/jenkins_home `
  --volume jenkins-docker-certs:/certs/client:ro `
  --publish 8080:8080 --publish 50000:50000 myjenkins-blueocean:1.1
  ```
5. Proceed to the Setup wizard.
