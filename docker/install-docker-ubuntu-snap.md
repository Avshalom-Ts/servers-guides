# This guid show how to install docker on ubuntu linux with snap package manager

1. Check if docker installed

    ```bash
    docker version
    ```

    - check for snap version if installed

    ```bash
    snap  version
    ```

2. If not installed

    ```bash
    sudo snap install docker
    ```

    - Check installed docker version

    ```bash
    sudo docker version
    ```

3. Add user to docker group to avoid sudo

    - Add the docker group if not pressent

    ```bash
    sudo groupadd docker
    ```

    - Add the user to docker group

    ```bash
    sudo gpasswd -a $USER docker
    ```

    - Restart the docker group

    ```bash
    sudo snap restart docker
    newgrp docker
    sudo reboot
    ```

    - Check if docker commands can run without sudo

    ```bash
    docker version
    ```
