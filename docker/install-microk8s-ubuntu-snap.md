# This guid show how to install microk8s on ubuntu linux with snap package manager

- [microK3s-docs](https://microk8s.io/)

1. Install the snap package

    ```bash
    sudo snap install microk8s --classic
    ```

2. Add the user to microk8s to avoid sudo

    ```bash
    sudo usermod -a -G microk8s azarviv
    # If exist
    sudo chown -R azarviv ~/.kube
    newgrp microk8s
    ```

    Check the status while Kubernetes starts

    ```bash
    microk8s status --wait-ready
    ```

    Add alias to avoid typing microk8s.kubectl every time

    ```bash
    sudo snap alias microk8s.kubectl kubectl
    ```

    Get microk8s version

    ```bash
    kubectl version --short
    ```

    Get the cluster info

    ```bash
    kubectl cluster-info
    ```
