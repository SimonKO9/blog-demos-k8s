# Local k8s

This document describes how to set up a local Kubernetes cluster using [kind](https://kind.sigs.k8s.io/docs/user/quick-start/).

## Prerequisites

1. Podman or Docker
2. Kubectl CLI
3. Helm CLI
4. Kind CLI

The setup was tested using the following versions:

```sh
$ podman --version
podman version 3.4.4

$ kubectl version --client
Client Version: v1.33.2
Kustomize Version: v5.6.0

$ helm version
version.BuildInfo{Version:"v3.18.3", GitCommit:"6838ebcf265a3842d1433956e8a622e3290cf324", GitTreeState:"clean", GoVersion:"go1.24.4"}

$ kind --version
kind version 0.29.0
```

## Installation

### Installation Guides

Podman - https://podman.io/docs/installation
Kubectl - https://kubernetes.io/docs/tasks/tools/
Helm - https://helm.sh/docs/intro/install/
Kind - https://kind.sigs.k8s.io/docs/user/quick-start/

### Rootless Podman

Please note that if you're using Rootless Podman, there's additional steps you have to perform. See https://kind.sigs.k8s.io/docs/user/rootless/#host-requirements for details.

Here's how it looked for me before I applied the necessary changes:
```sh
$ kind create cluster        
enabling experimental podman provider
Cgroup controller detection is not implemented for Podman. If you see cgroup-related errors, you might need to set systemd property "Delegate=yes", see https://kind.sigs.k8s.io/docs/user/rootless/
Creating cluster "kind" ...
 ✓ Ensuring node image (kindest/node:v1.33.1) 🖼 
 ✗ Preparing nodes 📦  
Deleted nodes: ["kind-control-plane"]
ERROR: failed to create cluster: could not find a log line that matches "Reached target .*Multi-User System.*|detected cgroup v1"
```

## Create a cluster

```sh
$ kind create cluster
# ...
Set kubectl context to "kind-kind"
You can now use your cluster with:

kubectl cluster-info --context kind-kind

Thanks for using kind! 😊


$ kubectl get node
NAME                 STATUS   ROLES           AGE    VERSION
kind-control-plane   Ready    control-plane   9m6s   v1.33.1
```

