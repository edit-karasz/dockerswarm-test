# Excercise with Docker swarm

The example is a web application built with Python Flask.

We will run it on a Docker swarm running on the localhost.

## Install Docker swarm

Docker swarm is a part of the docker-ce package. The easiest way to install it is to run a script stored on: 

``` $ docker swarm init --advertise-addr <IP> ```

This will create a master. You can run services on it as well so it is enough.

Check it:

``` $ docker node ls ```

Creating a registry on the localhost docker swarm:

``` $ docker service create --name registry --publish published=5000,target=5000 registry:2 ```

Check:
``` $ docker ps ```

### Building and installing the web service from a Dockerfile

Build the Docker image from the Dockerfile. It will package up and run the Python Flask web application. It will run on port 5000.

To make it easy to push the image to the local docker repository and to change the port configuration we will use the docker-compose.yaml :

``` $ docker-compose up -d ```

Creating the images to push:

``` $ docker-compose down --volumes ```
``` $ docker-compose push ```

This will store the images on the local registry.

If you want to push it to a gcp container registry than you have to:
* get your gcloud sdk installed and set with the target project
* initalise the connection with docker:

```gcloud auth configure-docker```
* Build the image whith the name / tag: gcr.io/support-181717/app:0.1 . You can do this by replacing the docker-compose.yaml file with the docker-compose-gcpsupport.yaml
* Push it to the container registry from the command line:
```docker push gcr.io/support-181717/app:0.1``` 

#### Deploy the service to the Docker Swarm

To deploy the service run:

``` $ docker stack deploy --compose-file docker-compose.yaml app ```

Remove it:

``` $ docker stack rm app ```

### Building and installing the web service from Packer

You have to install Packer.

You can put every listed step into 1 json file and build the Docker image from there.


If you want to build modify and push the image into the Support docker registry you have to run:

```
gcloud auth configure-docker

packer build webservice_gcpsupport_packer.json
```

This one is already up on the Support project's container registry. (app2)

### Pros and contras

* Docker cannot manage a service lifecycle from a Dockerfile. You have to use a combination of docker-compose and docker command lines to do that. Docker will always work because they are the providers.
* Packer is an extra component and it is not the native service provider. However it is able to manage a service's full configuration and lifecycle from a single file. Multiple services (like Ansible) can be added to that 1 file as an execution step. Unfortunately it is not well documented. It does not sequentially executes the steps added to the file: it has precedences like Cloudformation.
