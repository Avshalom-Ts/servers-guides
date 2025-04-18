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

- To give container name instead of the random name

    ```bash
    docker run --name myhelloworld hello-world
    ```

- To interact with running container, add `-it` flag and the desire interaction like `bash` to the run command

    ```bash
    docker run -it ubuntu bash
    ```
