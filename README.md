#<a name="top"></a> simple-docker-hosting on FIWARE cloud
[![License badge](https://img.shields.io/badge/license-APACHE_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Documentation badge](https://img.shields.io/badge/docs-v1.3-brightgreen.svg)](http://simple-docker-hosting-on-fiware-cloud.readthedocs.org/en/v1.3/)
[![Support badge]( https://img.shields.io/badge/support-sof-yellowgreen.svg)](http://stackoverflow.com/questions/tagged/fiware-docker)


* [Introduction](#introduction)
* [Description](#description)
* [Install and Administration](#install-and-administration)
* [Running](#running)
* [API Overview](#api-overview)


## Introduction

This is the code repository describes how FIWARE developers can create and deploy their services on the FIWARE Cloud also known as the FIWARE Lab.

This project is part of [FIWARE](http://www.fiware.org). Check also the [FIWARE Catalogue entry for Docker](http://catalogue.fiware.org/enablers/Docker).

[Top](#top)

## Description
 The Docker Generic Enabler provides the basic docker container hosting capabilities, as well as management of the corresponding resources within the Data Center that hosts a particular FIWARE Cloud Container Instance. It will ensure that FIWARE developers can leverage the docker ecosystem to create services composed of FIWARE GEs and deploy them on a FIWARE Cloud compliant provider. To that end the Docker GE exposes the docker API on FIWARE and allows FIWARE developers to easily host their docker container services on the FIWARE Lab nodes or on their own private FIWARE Cloud instances. While user can use the Cloud Portal GE to interact with their docker hosts, the Docker GE will also ensure that they can remotely create and manage their docker hosts, clusters and containers from their local docker clients.

The main capabilities provided for a docker cloud hosting user are:

	*Manage life cycle Docker Hosts
	*Manage network and storage attached Docker Hosts
    	*Resource monitoring of Docker Hosts
    	* Resiliency of the persistent data associated with Docker Hosts
    	* Manage resource allocation
    	* Secure access to the Docker Hosts
    	* Secure access to the Docker Containers 

For a cloud hosting provider, the following capabilities are provided:

    * Multi-tenancy (support isolation between Contains of different accounts)

## Install and Administration

Build and Install documentation for Docker on the FIWARE Cloud can be found at [the installation and administation manual](./doc/manuals/install.md). You can also find it on [readthedocs](http://simple-docker-hosting-on-fiware-cloud.readthedocs.org/en/latest/manuals/install/)

[Top](#top)

## Running

How to run Docker on the FIWARE Cloud can be found at [the user guide](doc/manuals/userguide.md). You can also find it on [readthedocs](http://simple-docker-hosting-on-fiware-cloud.readthedocs.org/en/latest/manuals/userdoc/)


[Top](#top)

## API Overview
The FIWARE cloud exposes the [Docker API](https://docs.docker.com/reference/api/docker_remote_api/).
 

