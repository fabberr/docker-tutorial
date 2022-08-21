Bellow are my notes on Docker written in markdown.

# Docker

A tool for packaging and distributing software along with all its dependencies. Enables your applications to be executed on any hardware/OS configuration without the need to install the required dependencies onto the host OS itself by running it in an isolated virtualized context.

# Concepts

#### Dockerfile

* Blueprint for building an image
* Defines an environment by describing exactly what dependencies and versions are to be installed in the image, with the commands needed to do so
* Provided by whoever packages the software

#### Image

* A file used to spawn a container, built from a Dockerfile
* Immutable snapshot of the environment strictly defined in the Dockerfile
* Runs on any system (image will share the same kernel as the host OS)

#### Container

* A running process, instance of a Docker image
* Environment where your application will run/be built in/deploy from/etc...
* Provides an isolated filesystem
    * Extra volumes may be mounted to it

# Basic CLI Usage

#### Help (manpages)

* `docker help <docker_cmd> [sub_cmd]`
* `docker <docker_cmd> --help`

#### Build Image From Dockerfile

* `docker build [-t <tag>] <dockerfile_root>`

#### Run Image (spawn container)

* `docker run -d [-p <host_port>:<container_port>] <tag>`

#### List Containers

* `docker ps`

#### Stop Running Containers

* `docker stop <container_id>`

#### Remove Container

* `docker rm <container_id>` (container must be currently stopped)
* `docker rm -f <container_id>` (force removal, stops container if running)

#### Execute in Container's shell

* `docker exec <container_id> <cmd> [args]`
* `docker exec -it <container_id> <shell_executable>` (open interactive TTY session using specified shell)

#### Show Container Logs

* `docker logs <container_id>`
* `docker logs -f <container_id> (follow log updates)`

## Dev Workflow

1. Build image:  

    ```
    docker build -t <tag> <dockerfile_root>
    ```
    
2. Mount local source code to container:
    
    ```
    cd <path_to_app_src>
    ```
    ```
    docker run -dp <host_port>:<container_port> \
    -w /app -v "$(pwd):/app" \
    <base_image> \
    sh -c <install_dependencies_and_build_tools_cmds>
    ```
    
3. Rebuild image (when done for the day):
    
    ```
    docker stop <container_id>
    ```
    ```
    docker build -t <tag> <dockerfile_root>
    ```
