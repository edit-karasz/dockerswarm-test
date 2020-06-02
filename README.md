# Excercise with Docker swarm

The example is a web application built with Python Flask.

We will run it on a Docker swarm running on the localhost.

## Install Docker swarm

Docker swarm is a part of the docker-ce package. The easiest way to install it is to run a script stored on: 

``` $ docker-machine ip <IP> ```

This will create a master. You can run services on it as well so it is enough.

Creating a registry on the localhost docker swarm:

``` $ docker service create --name registry --publish published=5000,target=5000 registry:2 ``

### Building and installing the web service from a Dockerfile

Build the Docker image from the Dockerfile. It will package up and run the Python Flask web application. It will run on port 5000.

To make it easy to push the image to the local docker repository and to change the port configuration we will use the docker-compose.yaml :

``` $ docker-compose up -d ```

Creating the images to push:

``` $ docker-compose down --volumes ```
``` $ docker-compose push ```

This will store the images on the local registry.

#### Deploy the service to the Docker Swarm

To deploy the service run:

``` $ docker stack deploy --compose-file docker-compose.yaml app ```

### Building and installing the web service from Packer

You have to install Packer.

You can put every listed step into 1 json file and build the Docker image from there.



### Pros and contras


