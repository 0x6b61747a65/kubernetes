# Kubernetes notes

## Ubuntu 20.04 install

[minikube docs](https://minikube.sigs.k8s.io/docs/start/)

0. Container or virtual machine driver should be installed

    ```bash
    sudo apt install docker.io -y
    ```

    Add current user to docker group

    ```bash
    sudo usermod -aG docker $USER
    ```

    "Live reload" current user in new group

    ```bash
    newgrp docker
    ```

1. Download deb package

    ```bash
    curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
    ```

2. Install with `dpkg`

    ```bash
    sudo dpkg -i minikube_latest_amd64.deb
    ```

3. Install `kubectl`
    Download

    ```bash
    sudo curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
    ```

    Install

    ```bash
    sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
    ```

    Check installation

    ```bash
    kubectl version --client
    ```

4. Start minikube

    ```bash
    minikube start
    ```

5. Check cluster status

    ```bash
    kubectl cluster-info
    ```

    ```bash
    kubectl get pods -A
    ```

6. Enable and start WebUI dashboard

    ```bash
    minikube addons enable dashboard
    ```

    ```bash
    minikube dashboard
    ```

7. Access WebUI dashboard
    If k8s is hosted on remote server, the simplest solution to access dashbord without using reverse proxy and `Ingress` controller is to redirect requests on local mashine's `localhost` through ssh tunnel.

    ```bash
    ssh 'ip' -i 'keyfile' -L 'localport':localhost:'dasbord-port'
    ```

    where:
    - ip - is IP of remote host
    - keyfile - is private key file
    - localport - port on a local machine
    - dasbord-port - is random port given when `dashboard` is binded on startup
