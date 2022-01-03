# Introduction
This section documents things to be done after successfully deploying a jenkins container in docker for an error free pipeline

1. Install Sudo and Add Jenkins to the Sudoers group
* Execute bash on jenkins container as root
````bash
docker exec -u root -it jenkins-blueocean /bin/bash
````
* Install sudo in Jenkins container
````bash
apt-get install sudo
````
* Add jenkins to sudoers group

````bash
usermod -aG sudo jenkins
````
* Change Jenkins Password

````bash
passwd jenkins
````
