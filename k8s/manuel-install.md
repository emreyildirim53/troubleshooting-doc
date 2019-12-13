# Kubernetes Installation Steps

## Assumptions

- 3 VMs pre-installed with Ubuntu 18.04.3 LTS, accessing internet
- All VMs should have at least 1 core, 2GB RAM and 10GB disk space

## Steps

1. Install `docker` on the all the nodes (`master-0`, `node-0`, `node-1`). Use below set of commands.

    ```bash
    sudo apt-get install -y \
        apt-transport-https \
        ca-certificates \
        curl \
        software-properties-common
    ```

    ```bash
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```

    ```bash
    sudo add-apt-repository \
      "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) \
      stable edge"
    sudo apt-get -y update
    ```

    Install very specific version (18.06.1) of docker

    ```bash
    sudo apt-get install -y docker-ce=18.06.1~ce~3-0~ubuntu
    ```

1. Install `Kubernetes` packages on all the nodes (`master-0`, `node-0`, `node-1`). Use below set of commands.

    ```bash
    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
    ```

    ```bash
    sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-$(lsb_release -cs) main"
    sudo apt-get -y update
    ```

    Now install the packages.

    ```bash
    sudo apt-get install -y kubernetes-cni=0.7.5-00
    sudo apt-get install -y kubelet=1.15.3-00
    sudo apt-get install -y kubectl=1.15.3-00
    sudo apt-get install -y kubeadm=1.15.3-00
    ```

1. Make sure that the swap is disabled, disable it if enabled using below command. Note that this only sets the swap off until next reboot. Make sure that there are no swap entry in `/etc/fstab` file.

    ```bash
    sudo swapoff -a
    ```

1. Run below command only on `master-0` to initialize the `Kubernetes` cluster.

    ```bash
    kubeadm init \
      --pod-network-cidr=10.244.0.0/16 \
      --apiserver-advertise-address=<internal_ip_of_your_master_node> \
      --ignore-preflight-errors=NumCPU
    ```

    Wait for initialization to complete, then run below commands

    ```bash
    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config
    ```

    **IMPORTANT**

    Make a note of the last line, should be like below. This command will be executed on worker nodes to join them to the cluster. Below is just an example.

    ```bash
    kubeadm join 128.199.44.30:6443 --token l356h2.hywubufcxmtglx7s \
    --discovery-token-ca-cert-hash sha256:633fbfab4015427e30327f3c5410fa698a1db7bf9d13f396b841b2de1e4e2987
    ```

1. Run the command you have taken note of from the previous step on nodes (node-0, node-1).

    ```bash
    kubeadm join <internal_ip_of_your_master_node> --token <token> \
    --discovery-token-ca-cert-hash <hash>
    ```

1. After commands finish from previous step, go to the master node and issue below commands to observe the status.

    You will see that the `coredns` pods are in `Pending` state while everything else is OK. Can you think of why?

    ```bash
    kubectl get nodes
    kubectl get pods
    kubectl get pods -A
    kubectl get pods -A -o wide
    ```

1. We are missing a networking plugin, let's install Weave using below command.

    ```bash
    kubectl apply -f https://cloud.weave.works/k8s/net?k8s-version=1.15
    ```

    After `weave` pods are created, situation should be better and everything should be in `Running` state.