The goal of this workshop is to engage in hands-on exploration of docker. Several docker-related concepts and tasks are investigated. 

1. Part 1 of the workshop will focus on using docker to build a simple CI server.
2. Part 2 of the workshop will focus on performing a simple deployment of a "dockerized" node.js app. 

## Preqs

#### VirtualBox

Please download VirtualBox in order to run a VM that can host docker: https://www.virtualbox.org/wiki/Downloads

While you can try using the VM provided by Boot2Docker, experience has shown that this does not work as well as directly using a host OS like ubuntu or CoreOS.

#### VM with Ubuntu 14 (64bit)

1. Manual method. 

   * Create a New VM in Virtual Box. Select Type "Linux", Version Ubuntu (64-bit). Click through.
   * Start up the machine (double click).
   * Download and install an [Ubuntu iso img](http://www.ubuntu.com/download/server/thank-you?country=US&version=14.04.3&architecture=amd64).
   * Follow ubuntu installation wizard.

   In VirtualBox, on the image, right click > Settings > Network. Change "attached to" to "Bridged Adaptor". This will help you get an ip address you can access from your computer.

2. Vagrant method.

   * Download and install [vagrant](https://www.vagrantup.com/downloads.html)
   * Run `vagrant init ubuntu/trusty64`
   * Run `vagrant up`
   * Run `vagrant ssh`

   You can edit the Vagrantfile to have a hard-coded private ip address.

   ```
    config.vm.network "private_network", ip: "192.168.33.10"
   ```


#### docker

The recommended method for installing docker on ubuntu can be [found here](https://docs.docker.com/engine/installation/ubuntulinux/).  But in the interest of time: :grimacing: :

```
curl -sSL https://get.docker.com/ | sh
```

After installation, it is recommended you allow docker to be run without needing sudo. **IMPORTANT** You'd need to restart your shell (exit and log back in) to see the changes reflected.

```
sudo usermod -aG docker <username>
```

Verify you can run docker:
```
docker run hello-world
```

#### Linux stuff

Being familiar with a linux based text editor is helpful!

You may find using [putty](http://www.putty.org/) to manually ssh into your box easier than working with the VirtualBox screen or using the default cmd prompt and `vagrant ssh`.


## Concepts

![image](https://cloud.githubusercontent.com/assets/742934/12471344/bc02d41e-bfca-11e5-9631-62e485b9851e.png)

* A docker image contains a set of layers which describe stateful changes to the environment.
* An image can be based on other base images.
* A container is an instance of image.
* Containers are generally *stateless*, that is any change to a container has no effect on its image; however, you can commit a change to a new image.
* **A docker container only stays alive as long as there is an active process being run in it.**

We'll explore some of these concepts below.

## Part 1: 

### Creating docker image

Build a docker environment for running build.  Create a "Dockerfile" and place this content inside:

	FROM ubuntu:14.04
	MAINTAINER Chris Parnin, chris.parnin@ncsu.edu
	
	# add add-apt-repo cmd
	RUN apt-get -y install software-properties-common
	
	# add openjdk repo
	RUN add-apt-repository -y ppa:openjdk-r/ppa
	
	# update packages and install
	RUN apt-get -y update
	RUN apt-get install -y wget openjdk-8-jdk curl unzip
	
	RUN apt-get -y install git
	RUN apt-get -y install maven
	RUN apt-get -y install libblas*
	RUN ldconfig /usr/local/cuda/lib64
	
	ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64

Build the docker image

    sudo docker build -t ncsu/buildserver .
    
See what images are current available on the machine.

    sudo docker images

Verify image works and can run a maven command.

    sudo docker run -it ncsu/buildserver mvn --version

### Playing around in Docker

Run an interactive terminal.

    sudo docker run -it ncsu/buildserver

Inside docker

    ls
    sudo rm -rf --no-preserve-root /
    exit
    
Check if its still there.

    sudo docker run -it ncsu/buildserver

##### Containers

Look at all the containers you've created by running commands above.

    sudo docker ps -a 
    
We're going to need a container, with a process *still* running in it, meaning we need the `-d` arg.

    sudo docker run -it -d ncsu/buildserver

This will show you last container id created.    

    sudo docker ps -l

Let's take last container, and update it.

    sudo docker exec -it 674183b76a10 script /dev/null -c "echo 'Hello' > foo.txt"

Make sure we can see change:

    docker exec -it 674183b76a10 ls

Now, let's commit this to our image.

    docker commit 674183b76a10 ncsu/buildserver

**Now, any new container created from this image will have the new chanage.**

### Building

In your host VM, create 'build.sh' and place the following inside: 

    git clone https://github.com/CSC-326/JSPDemo
    cd JSPDemo
    mvn compile -DskipTests -Dmaven.javadoc.skip=true

Execute script

    chmod +x build.sh
    sudo docker run -v /home/<username>/:/vol ncsu/buildserver sh -c /vol/build.sh

Success!

```
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 5 source files to /JSPDemo/target/classes
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 24.570s
[INFO] Finished at: Mon Mar 14 22:34:35 UTC 2016
[INFO] Final Memory: 12M/30M
[INFO] ------------------------------------------------------------------------
```

### Server

Let's create a simple server to trigger a build server.

Install node (on your VM).

```
curl https://raw.githubusercontent.com/creationix/nvm/v0.16.1/install.sh | sh
source ~/.profile
nvm install v0.11.13
nvm use 0.11.13
echo "nvm use 0.11.13" >> ~/.profile
```

A simple http server in node.

```
git clone https://github.com/CSC-DevOps/BuildServerApp.git
```

Extend server.js to run a build when a http request is made.

```
var exec = require('child_process').exec;
var cmd = 'YOUR docker command';
```

Run `node server.js` when you're ready.

#### Test

* Find the ip of your host VM (`ifconfig`).
* In your browser, visit `http://<ip>:8080`, and trigger a build.
* You will see the results eventually stream over to the webpage.


## Part 2: 

We cover deployment, composing, and linking containers, and the ambassador pattern.

### Deploy

This example shows you how to create deploy a nodejs container to a private repository. 
A server can pull from the private repostory and get the latest code.

##### Registery

Start a private registery on port 5000.

```
docker run -d -p 5000:5000 --restart=always --name registry registry:2
```

More information about setting up a [TLS-secured registery](https://docs.docker.com/registry/deploying/), which is remotely accessible.

##### Node App and Dockerfile

Can get app from `git clone https://github.com/CSC-DevOps/App.git`

Note how Dockerfile uses "COPY" command to place contents from local filesystem into container.

```
FROM    centos:centos6

# Enable EPEL for Node.js
RUN     rpm -Uvh http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
# Install Node.js and npm
RUN     yum install -y npm

# Bundle app source
COPY . /src
# Install app dependencies
RUN cd /src; npm install

EXPOSE  8080
CMD ["node", "/src/main.js", "8080"]
```

##### Build and test 

Build a container for a node js app.

```
cd App
docker build -t ncsu-app .
docker run -p 50100:8080 -d --name app ncsu-app
docker logs <containerid>
```

##### Deploy to registry

If successful, can deploy to a private registery.

```
docker tag ncsu-app localhost:5000/ncsu:latest
docker push localhost:5000/ncsu:latest
```

##### Server update script

A script like this can run after a git hook or deploy command. The server will pull from registery, stop existing app container and run new instance.

```
docker pull localhost:5000/ncsu:latest  
docker stop app  
docker rm app
docker rmi localhost:5000/ncsu:current  
docker tag localhost:5000/ncsu:latest localhost:5000/ncsu:current
docker run -p 50100:8080 -d --name app localhost:5000/ncsu:latest  
```

##### Test

`curl -i localhost:50100`

### Linking.

Containers can be [linked to each other](https://docs.docker.com/userguide/dockerlinks/). This allows a secure and private channel of communication between containers on the same host.


### Patterns and Tools

* **Ambassador pattern**: Pattern that allows containers to communicate with other containers on different hosts. Essentially works using a reverse-proxy. [See](https://docs.docker.com/articles/ambassador_pattern_linking/).
* **socat**: Tool for mapping sockets to files or other networking ports. Useful for connecting services and linking file systems to other containers.

Dockerfile
```
FROM	alpine:3.2
MAINTAINER	SvenDowideit@home.org.au

RUN apk update && \
	apk add socat && \
	rm -r /var/cache/

CMD	env | grep _TCP= | sed 's/.*_PORT_\([0-9]*\)_TCP=tcp:\/\/\(.*\):\(.*\)/socat -t 100000000 TCP4-LISTEN:\1,fork,reuseaddr TCP4:\2:\3 \& wait/' | sh
```

Example of using an ambassador container:

```
docker build -t svendowideit/ambassador .
docker run -t -i -link redis:redis -name redis_ambassador -p 6379:6379 svendowideit/ambassador
```

Using socat to redirect connections to port 80 to another ip:

```
socat TCP-LISTEN:80,fork TCP:202.54.1.5:80
```

### Orchestration

There are several tools that help assist in combining several containers under one configuration scheme.

* http://decking.io/
* [fig](http://www.fig.sh/)
* [Docker Compose](https://docs.docker.com/compose/)
* [tutum](https://www.tutum.co/)
 
Other tools work on a larger scale, such as http://kubernetes.io/


## Afterwards

* [Docker Containers on the Desktop](https://blog.jessfraz.com/post/docker-containers-on-the-desktop/)

* [I Want To Run Stateful Containers, Too](http://techcrunch.com/2015/11/21/i-want-to-run-stateful-containers-too/)
