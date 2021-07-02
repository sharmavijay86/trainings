## Setup Docker lab in ubuntu 20.04
We will be using docker CE ( community edition ) in setup. Assuming you already have ubuntu 20.04 VM for this lab.
## Install using the repository
Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

### Set up the repository
Update the apt package index and install packages to allow apt to use a repository over HTTPS:
```
 sudo apt-get update
 sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```
Add Docker’s official GPG key:
```
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
 ```
Add repo 
```
 $ echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
### Install Docker Engine
Update the apt package index, and install the latest version of Docker Engine and containerd, or go to the next step to install a specific version:
```
 sudo apt-get update
 sudo apt-get install docker-ce docker-ce-cli containerd.io
```

To install a specific version of Docker Engine, list the available versions in the repo, then select and install:

a. List the versions available in your repo:
```
 apt-cache madison docker-ce
```
b. Install a specific version using the version string from the second column, for example, 5:18.09.1~3-0~ubuntu-xenial.
```
 sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io
```
Verify that Docker Engine is installed correctly by running the hello-world image.
```
 sudo docker run hello-world
 ```