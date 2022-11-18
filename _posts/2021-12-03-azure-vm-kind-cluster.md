---
layout: post
title:  "Create a Kubernetes in Docker (Kind) VM in Azure"
date:   2022-01-17 08:43:59
author: Reuben Cleetus
categories: blog
cover:  "/assets/instacode.png"
---


**Create the VM in Azure**
- Ubuntu VM

### Install Docker
```
sudo apt-get update

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

Add Docker’s official GPG key:
```
     curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    
 ```
    
**Set up the Stable repository**

```
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### Install Docker Engine

```
sudo apt-get update
sudo apt install docker.io
```

### Test
```
sudo docker run hello-world
```

### KIND Cluster YAML
```
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    listenAddress: "127.0.0.1"
- role: worker
```

### Create the KIND Cluster

```
sudo kind create cluster --config kind_cluster.yaml --name kind-cluster
```

