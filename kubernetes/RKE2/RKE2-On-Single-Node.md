# Steps to install RKE2 and Rancher on a Node

- [YouTube-Tutorial](https://www.youtube.com/watch?v=Gr08LhMQVoE&list=WL&index=19&ab_channel=Clemenko-KubernetesFirefighter)

- [RKE2](https://docs.rke2.io) - Security focused Kubernetes
- [Rancher](https://www.suse.com/products/suse-rancher/) - Multi-Cluster Kubernetes Management
- [Longhorn](https://longhorn.io) - Unified storage layer

We will need a few tools for this guide. We will walk through how to install `helm` and `kubectl`.

A longer version of this install : [Can a 12 y/o install the Rancher Stack?](https://youtu.be/_70Z5-4lvEo)

---

## Prerequisites

The prerequisites are fairly simple. We need a linux servers with access to the internet. We just need an `ssh` client to connect to the server. The recommended size of the node is 8 Cores and 16GB of memory with at least 100GB of storage.

| name | ip | memory | core | disk | os |
|---| --- | --- | --- | --- | --- |
|rancher| 177.178.179.16  | 16384 | 8 | 160 | Rocky 9.3 x64 |

**Rocky**: Prepare the server

```bash
# Rocky instructions 
# stop the software firewall
systemctl disable --now firewalld

# get updates, install nfs, and apply
yum install -y nfs-utils cryptsetup iscsi-initiator-utils

# enable iscsi for Longhorn
systemctl enable --now iscsid.service 

# update all the things
yum update -y

# clean up
yum clean all
```

## RKE2 Install

```bash
# Install RKE2 from official script, and define two variables, the rke2 version and the type of the installation(server), for enothere node it will be type agent.
curl -sfL https://get.rke2.io | INSTALL_RKE2_CHANNEL=v1.28 INSTALL_RKE2_TYPE=server sh - 

# Start and enable rke2 service. 
systemctl enable --now rke2-server.service

# Check the service is running
systemctl status rke2-server.service
```

Symlink the `kubectl` cli on `rancher1` that gets installed from RKE2.

```bash
# Symlink all the things - kubectl
ln -s $(find /var/lib/rancher/rke2/data/ -name kubectl) /usr/local/bin/kubectl

# Add kubectl conf with persistence
echo "export KUBECONFIG=/etc/rancher/rke2/rke2.yaml PATH=$PATH:/usr/local/bin/:/var/lib/rancher/rke2/bin/" >> ~/.bashrc
source ~/.bashrc

# check node status
kubectl get node

# check node status with more options
kubectl get node -o wide

# To get all namespaces
kubectl get namespaces

# To get all pods from all namespaces 
kubectl get pod -A

# To get all pods from specific namespace
kubectl get pod -n default
```

### Rancher Install

For Rancher we will need [Helm](https://helm.sh/). We are going to live on the edge! Here are the [install docs](https://ranchermanager.docs.rancher.com/getting-started/installation-and-upgrade/install-upgrade-on-a-kubernetes-cluster) for more references.

```bash
# Make shure tar and git is installed, its requered for the helm script
yum install tar git
# Add helm from the official script
curl -#L https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

# Add needed helm charts(versions)
helm repo add rancher-latest https://releases.rancher.com/server-charts/latest
helm repo add jetstack https://charts.jetstack.io

# Check the repo list for helm
helm repo list
```

Quick note about Rancher. Rancher needs jetstack/cert-manager to create the self signed TLS certificates.
We need to install it with the Custom Resource Definition (CRD). Please pay attention to the `helm` install for Rancher. The URL`rancher.177.178.179.16.nip.io` will need to be changed to fit your FQDN.
Also notice I am setting the `bootstrapPassword` and replicas. This allows us to skip a step later. :D

```bash
# helm install jetstack(cert-manager)
helm upgrade -i cert-manager jetstack/cert-manager -n cert-manager --create-namespace --set installCRDs=true

# helm install rancher, take some time to initiat the certs
helm upgrade -i rancher rancher-latest/rancher --create-namespace --namespace cattle-system --set hostname=rancher.177.178.179.16.nip.io --set bootstrapPassword=bootStrapAllTheThings --set replicas=1

# To get namespaces
kubectl get ns

# To get apps running
kubectl get ingress -A
```

### Rancher GUI

Navigate to [Rancher GUI](https://rancher.177.178.179.16.nip.io) the password is `bootStrapAllTheThings`.

### Rancher Design

Let's take a second and talk about Ranchers Multi-cluster design. Bottom line, Rancher can operate in a Spoke and Hub model. Meaning one k8s cluster for Rancher and then "downstream" clusters for all the workloads. Personally I prefer the decoupled model where there is only one cluster per Rancher install. This allows for continued manageability during networks outages. For the purpose of the is guide we are concentrate on the single cluster deployment. There is good [documentation](https://ranchermanager.docs.rancher.com/how-to-guides/new-user-guides/kubernetes-clusters-in-rancher-setup/register-existing-clusters) on "importing" downstream clusters.

## Longhorn

Deploy longhorn from the App Catalog built into Rancher.
