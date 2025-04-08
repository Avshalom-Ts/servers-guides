# Steps to install k8s on ubuntu

- [YouTube tutor](https://www.youtube.com/watch?v=2mWwBEnv658&list=PLyRsEx7ohcVOxl-K3Z2l5KBjc16qcfr5b&index=2&ab_channel=Farhad)

- [Official Docs](https://kubernetes.io/docs/home/)

## Step 1 - Install ContainerD

- From version 1.24 kubernets dont suport for docker so insted is needed to install containerd run time.

- Install [Containerd Official web](https://containerd.io/) [Conatinerd official github repo](https://github.com/containerd/containerd/blob/main/docs/getting-started.md)

```sh
wget https://github.com/containerd/containerd/releases/download/v1.7.2/containerd-1.7.2-linux-amd64.tar.gz
sudo tar Cxzvf /usr/local containerd-1.7.2-linux-amd64.tar.gz
```

```sh
wget https://github.com/opencontainers/runc/releases/download/v1.1.7/runc.amd64
sudo install -m 755 runc.amd64 /usr/local/sbin/runc
```

```sh
wget https://github.com/containernetworking/plugins/releases/download/v1.3.0/cni-plugins-linux-amd64-v1.3.0.tgz
sudo mkdir -p /opt/cni/bin
sudo tar Cxzvf /opt/cni/bin cni-plugins-linux-amd64-v1.3.0.tgz
```

```sh
sudo mkdir -p /usr/local/lib/systemd/system
wget https://raw.githubusercontent.com/containerd/containerd/main/containerd.service
sudo cp ./containerd.service /usr/local/lib/systemd/system/containerd.service
```

```sh
sudo systemctl daemon-reload
sudo systemctl enable --now containerd
```

## Step 2 - Install kubeadm  [Info](https://kubernetes.io/docs/tasks/tools/#kubeadm)

- Install [kubeadm doc](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

**Note: The default behavior of a kubelet is to fail to start if swap memory is detected on a node. This means that swap should either be disabled or tolerated by kubelet.**

```bash
# To disable swap temporarly, in cloud image there is no swap
sudo swapoff -a

# Edit the fstab file
sudo vim /etc/fstab

# Look for the line that references the swap file, which usually looks like this: /swapfile none swap sw 0 0
sudo rm /swapfile
sudo reboot
```

- Istall kubeadm

```bash
sudo apt-get update
# apt-transport-https may be a dummy package; if so, you can skip that package
sudo apt-get install -y apt-transport-https ca-certificates curl gpg
```

```bash
# If the directory `/etc/apt/keyrings` does not exist, it should be created before the curl command, read the note below.
# sudo mkdir -p -m 755 /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.32/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

# This overwrites any existing configuration in /etc/apt/sources.list.d/kubernetes.list
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.32/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

- Check the kubelet service if running.

```bash
sudo systemctl status kubelet
```

**The kubelet is now restarting every few seconds, as it waits in a crashloop for kubeadm to tell it what to do.**

- **Before** Initiat kubelet with kubeadm, need to configure the kubeadm for services and what he control.

[Network configuration](https://kubernetes.io/docs/setup/production-environment/container-runtimes/#network-configuration).
By default, the Linux kernel does not allow IPv4 packets to be routed between interfaces. Most Kubernetes cluster networking implementations will change this setting (if needed), but some might expect the administrator to do it for them. (Some might also expect other sysctl parameters to be set, kernel modules to be loaded, etc; consult the documentation for your specific network implementation.)

```bash
# sysctl params required by setup, params persist across reboots
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.ipv4.ip_forward = 1
EOF

# Apply sysctl params without reboot
sudo sysctl --system
```

- Verify that net.ipv4.ip_forward is set to 1 with:

```bash
sysctl net.ipv4.ip_forward
```

- Create file `kubeadm-config.yaml`. ad passt:

```yaml
kind: ClusterConfiguration
apiVersion: kubeadm.k8s.io/v1beta4
networking:
  podSubnet: 10.1.0.0/16 # The subnet for kubernets range to use
controlPlaneEndpoint: ha-control-plane # Needed to allow hight-availability on the master node
kubernetesVersion: v1.32.0
---
kind: KubeletConfiguration
apiVersion: kubelet.config.k8s.io/v1beta1
cgroupDriver: systemd
```

```bash
sudo kubeadm init --config kubeadm-config.yaml
```
