# Installing Docker on FIWARE Cloud

* [Introduction](#introduction)
* [Requirements](#requirements)
* [Docker Machine](#docker-machine)
  

## Introduction
This installation guide will explain how you can easily setup your Docker environment to develop and deploy your services on the FIWARE Lab.

FIWARE has developed many features with which you can compose your services;

Features related to:
*  data context management
*  integration of data and media content
*  IoT
*  Apps for data visualization and publication
*  Advanced Web UI
*  Software defined networking
*  Security
*  and more.

All of the features known as Generic Enablers or GEs for short have packaged as Docker containers.
These FIWARE containers can be leveraged to compose your own FIWARE based services.
You can develop and deploy your services on the FIWARE Cloud.

The FIWARE Cloud supports both VM hosting and Docker hosting.  
So why would you want to use Docker over VMs?
Docker containers are much smaller than VMs.  They take up a fraction of the space.
Most organization could fit all of their required containers on a single Docker host.
In the world dominated by VMs, modular design would call for one component per VM.
With Docker you can pack the same components referred as Docker micro-services on a single Docker host.
It is much easier to manage a single Docker host than multiple VMs.
The start-up time for each micro-service is much faster than what would be required for a VM.
Likewise, integrating the micro-services into a single application is much easier.
Beyond all that the Docker eco-system makes it easier to reuse other Docker containers and tools for managing the docker lifecycle.

The primary elements of the Docker Ecosystem are Docker Hub, Docker Engine,  Docker Compose, Docker Swarm and Docker Machine.
* Docker Hub is a cloud service for managing and sharing Docker container images, including FIWARE services, known as Generic Enablers (GE);  You can browse for the GEs in the fiware catalogue or search for their docker containers in the Docker Hub.
* Docker Engine, or simply Docker, creates and runs Docker containers; It supports automatic pulling of docker images from Docker Hub.  It then caches the images locally to speed up subsequent pulls. You can make run time changes to your images and then push them back on to Docker Hub to save them or share them with other developers.
* Docker Compose allows you to define and run multi-container applications;  Most GEs describe how they can be used to compose new applications.
* Docker Swarm: manages a pool of Docker hosts using the full suite of Docker tools. Because Docker Swarm serves the standard Docker API, any tool that already communicates with a Docker daemon, e.g. Docker-Compose, can use Swarm to transparently scale to multiple hosts.
* Docker Machine: creates and manages Docker hosts locally or on cloud providers (including OpenStack). It can be used to create and manage Docker swam clusters. Since the FIWARE Cloud is based on OpenStack, docker machine can be used to create and manage docker hosts on the FIWARE cloud.
* Docker containers, Docker hosts, and Docker Swarm clusters can be hosted on the FIWARE lab.

Not only can you host and manage docker on FIWARE, but you can do this remotely from your local Docker client; 

In this illustration we show a local Docker client remotely controlling Docker hosts from a work station.

The Docker hosts may be spread over multiple FIWARE regions and each region may contain multiple Docker hosts.

Most of the communication between the client and FIWARE is through the Docker REST API.

But Docker Machine also uses the OpenStack API to allocate resources and SSH to provision and configure the Docker hosts.

All of this communication is done securely using a combination of Transport Layer Security (TLS), token based authentication and authorization with Openstack Keystone, and SSH encryption.

Once the Docker host have be created other tools like compose can be used to create and deploy complex Docker services.

Swarm can be used to manage a cluster of Docker hosts running on FIWARE.

The rest of this tutorial will be devoted to showing you how to accomplish all of this.

## Requirements
As I said earlier, most of FIWARE Docker hosting can be accomplish remotely from your docker client.

But there are a few set up steps that require you to use the FIWARE 1. Cloud Portal. You must obtain a FIWARE account. Signing up for a FIWARE account is simple.  Go to [FIWARE](https://account.lab.fiware.org/).  Submit the required information.  You should be authorized within a day.

2. Optionaly request a community account which will give you more privaleges and a more stable environment.
3. Once you have an account you must create a security group. 
  * Select Cloud->Security->Security Groups.
  * Edit Security Group open incoming ports:
	* The required ports are 2376 and 22. The Docker Daemon Port listens on 2376.  Opening the docker daemon port allows docker clients to communicate with docker remotely. The ssh daemon listens on port 22.  Docker Machine uses ssh to provision and configure the docker host.  If necessary you can replace this port with another.
	* Optional you can open other ports to be used by the docker containers to interact with the outside world: Ports 32768-33768 are auto allocated by docker when creating containers. Of course you open other ports.  For instance 8080 for a web service. We also have observed that the Docker Swarm Master uses 3376.
Through out this document we refer to the security group docker-machine-sg that we created in this step. 
4. Next you must allocate at least one floating IP to your project. Select Cloud->Security->Float IPs->Allocate IP to Project.
5. You also might what to take a look a the VM images that are used to when creating a docker host.  Through out this document we use the VM image "Ubuntu Server 14.04.1 (x64)".  But the "base_ubuntu_14.04" also works. The difference between the two images are that "Ubuntu Server 14.04.1 (x64)" allows for root access with ssh. "base_ubuntu_14.04" is set up for ubuntu access with sudo privaledges. 
6. Next you must [install docker](https://docs.docker.com/linux/step_one/) and [docker machine](https://docs.docker.com/machine/install-machine/) on your local work station. We will use docker machine to create docker hosts and swarm clusters on FIWARE regions.

Details on how to accomplish all this follows.

## Docker Machine
### Overview
Machine lets you create Docker hosts on your computer, on cloud providers, and inside your own data center. It automatically creates hosts, installs Docker on them, then configures the docker client to talk to them. A "machine" is the combination of a Docker host and a configured client.

Once you create one or more Docker hosts, Docker Machine supplies a number of commands for managing them. Using these commands you can

 *   start, inspect, stop, and restart a host
 *   upgrade the Docker client and daemon
 *   configure a Docker client to talk to your host
For details see the [Docker Machine home page](https://docs.docker.com/machine/) 

