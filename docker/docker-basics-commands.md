# This guid is to understand docker and is commands

- To get all available commands for docker

    ```bash
    docker run --help
    ```

- To check that docker can run images

    ```bash
    docker run hello-world
    ```

- To check for running containers

    ```bash
    docker container ls
    ```

- To check all containers in the system

    ```bash
    docker container ls -a
    ```

- To list only the ids of running containers

    ```bash
    docker container ls -q
    ```

- To give container name instead of the random name

    ```bash
    docker run --name myhelloworld hello-world
    ```

- To interact with running container, add `-it` flag and the desire interaction like `bash` to the run command

    ```bash
    docker run -it ubuntu bash
    ```

- To expose port of the running container like `nginx` in dethach mode `d`, `-p <local-port>:<container-port>` to map the container port to the local port

    ```bash
    docker run --name mynginx -d -p 8080:80 nginx
    ```

- To stop a running container

    ```bash
    docker stop <container-name/container-id>
    ```

- To Stop all runnig container

    ```bash
    docker stop $(docker container ls -q)
    ```

- To delete all stoped containers

    ```bash
    docker remove $(docker container ls -aq)
    ```

- To delete stoped container

    ```bash
    docker remove <container-name/container-id>
    ```

- To inspect the confiq of container

    ```bash
    docker inspect <container-name/container-id>
    ```

    Check if the maped works by going to `http://<server-address>:<local-port>/index.html`

- To use volumse and map it to running container, use `-v <local-location-files:container-location-files>:ro`, `ro` is for read only

    ```bash
    docker run --name mynginx -v ./www:/usr/share/nginx/html:ro -d -p 8080:80 nginx
    ```

- Interacting with running container buy adding `exec` command to docker command, to interct with container shell

    ```bash
    docker container exec -it <container-name/container-id> <interac-shell>
    ```

- To check for `ENV` environments inside running container

    ```bash
    docker exec <container-name/id> env
    ```

- To set env varible on cotainer

    ```bash
    docker run -it -d --name mybusybox -e var1='var1' -e var2='var2' busybox
    ```

- To tail the logs of running container to the console `-f` for container name/id

    ```bash
    docker container logs -f mynginx
    ```
