# Docker basics

[Docker](https://www.docker.com/) uses containerization technology to create isolated, portable virtual environments for executing or developing code on a host system. Docker also allows for computational reproducibility through pre-built container images and pre-defined recipes (e.g., Dockerfiles), and facilitates sharability via container registries (e.g., [Docker Hub](https://hub.docker.com/)).

## Install Docker
- [Docker Engine on Linux](https://docs.docker.com/engine/install/)
    - Debian/Ubuntu
        ```bash
        sudo apt-get update && sudo apt-get install docker.io
        ```
    - User permission
        ```bash
        sudo adduser LOGIN_ID docker
        ```
        - By default, running Docker requires root privileges.
        - To check group members and user's group, check `/etc/group` or type `groups`.
- Version information
    ```bash
    docker --version
    docker version
    docker system info
    ```

## Use Docker
- Download images
    ```bash
    docker pull [OPTIONS] debian:stable
    ```
    - [OPTIONS]
        - `--platform=OS/ARCH`: specify a platform
- List images
    ```bash
    docker images [OPTIONS] IMG
    ```
    - [OPTIONS]
        - `--all` (`-a`): list all images
- Inspect images
    - Show low-level information
        ```bash
        docker inspect IMG:TAG
        ```
    - Show history
        ```bash
        docker image history IMG:TAG
        ```
- Start a container
    ```bash
    docker container run [OPTIONS] IMAGE [COMMANDS]
    docker run [OPTIONS] IMAGE [COMMANDS]
    ```
    - [OPTIONS]
        - `--rm`: automatically remove the container after use
        - `-it`: interactive terminal
        - `--name NAME`: assign a name to a container
        - [Storage mount options](https://docs.docker.com/engine/storage/)
            - `--volume=HOST_DIR:DOCKER_DIR` (`-v`): volume mounts
            - `--mount type=bind,source=/SRC/,target=/DST/`: Bind mounts
        - `--workdir` (`-w`): set the working directory
        - `--user=UID:GID` (`-u`): user identity (e.g., `--user=$(id -u):$(id -g)`)
        - `--entrypoint CMD`: override the entrypoint command
- Build a container
    - From a [Dockerfile](https://docs.docker.com/reference/dockerfile/)
        ```bash
        docker image build -t IMG_NAME PATH_TO_DOCKERFILE
        ```
        - Dockerfile examples
            - https://github.com/nuest/ten-simple-rules-dockerfiles
    - Create an image with a renamed tag
        ```bash
        docker image tag OLD_NAME NEW_NAME
        ```
    - Push to [Docker Hub](https://hub.docker.com/)
        - Register an account (`USERNAME`)
        - Login
            ```bash
            docker login
            ```
        - Push
            ```bash
            docker image push USERNAME/IMG:TAG
            ```
- List running containers
    ```bash
    docker container ls [OPTIONS]
    docker ps [OPTIONS]
    ```
    - [OPTIONS]
        - `--all` (`-a`): list all recent containers
- Remove image
    ```bash
    docker image rm [OPTIONS] REPOSITORY
    docker image rm [OPTIONS] IMAGE_ID
    docker rmi [OPTIONS] IMG:TAG
    ```
    - [OPTIONS]
        - `--force` (`-f`): force 
- Remove container
    ```bash
    docker container rm [OPTIONS] CONTAINER_ID_NAME
    docker rm [OPTIONS] CONTAINER_ID_NAME
    ```
    - [OPTIONS]
        - `--force` (`-f`): force removal
- Prune data
    - Images + Containers + Cache
        ```bash
        docker system prune
        ```
    - Dangling images only
        ```bash
        docker image prune
        ```
    - Stopped/unused containers only
        ```bash
        docker container prune
        ```

## References
- https://www.fun-mooc.fr/fr/cours/reproducible-research-ii-practices-and-tools-for-managing-comput/
- https://train.rse.ox.ac.uk/material/HPCu/technology_and_tooling/docker
