---
title: "Kubernetes Project: Building a 3-Node Cluster with kubeadm"
date: 2026-08-21
categories: [Docker, Linux Server, Kubernetes, Scaling, Self-Healing]
tags: [Docker, Linux Server, Kubernetes, Scaling, Self-Healing]
---

For this project, I built a 3-node Kubernetes cluster using kubeadm on local VMware virtual machines. The goal was to create a simple environment where I could demonstrate some of the core Kubernetes concepts, including Deployments, Services, scaling, and self-healing.

This project also helped me understand how Kubernetes is bootstrapped from the infrastructure layer up to the application workload.

## Environment

The cluster consists of one control-plane node and two worker nodes.

| Hostname | Role | vCPU | RAM | Disk |
|---|---|---:|---:|---:|
| k8s-master | Control Plane | 2 | 2 GB | 30 GB |
| k8s-worker1 | Worker | 2 | 2 GB | 30 GB |
| k8s-worker2 | Worker | 2 | 2 GB | 30 GB |

## Architecture

The setup uses VMware virtual machines as the infrastructure layer. kubeadm is then used to bootstrap the Kubernetes cluster, with containerd providing the container runtime and Flannel providing pod networking.

![Kubernetes Pipeline Architecture](/assets/img/posts/k8s_vmware_cluster_architecture.png)

## Base VM Preparation

Before installing Kubernetes, I prepared all three VMs.

The preparation included:

- Assigning static IP addresses using Netplan
- Disabling swap, as required by kubelet
- Loading the required kernel modules
- Configuring Sysctl parameters for Kubernetes networking
- Adding all three hostnames and IP addresses to `/etc/hosts` on every VM

![Base VM Preparation](/assets/img/posts/baseprep.png)

These steps ensured that the VMs had the basic networking and operating system configuration required for the Kubernetes components.

## Installed containerd

Next, I installed containerd on all three nodes and configured it as the container runtime.

![Installed containerd](/assets/img/posts/containerd.png)

Using a dedicated container runtime allows Kubernetes to manage the containers running inside the cluster.

## Installed kubeadm, kubelet, and kubectl

I then installed the main Kubernetes components:

- kubeadm – used to bootstrap the Kubernetes cluster
- kubelet – runs on each node and manages Kubernetes workloads
- kubectl – command-line tool used to interact with the Kubernetes API

![Installed kubeadm, kubelet, and kubectl 1](/assets/img/posts/installkkk1.png)

![Installed kubeadm, kubelet, and kubectl 2](/assets/img/posts/installkkk2.png)

## Initialized the Control Plane

After preparing the control-plane node, I initialized the cluster using `kubeadm init`.

The initialization process configured the Kubernetes control-plane components and generated a kubeadm join command at the end.

This command would later be used to join both worker nodes to the cluster.

![Initialized the Control Plane](/assets/img/posts/initializedcp.png)

## Installed the CNI: Flannel

After initializing the control plane, I installed Flannel as the Container Network Interface (CNI).

The CNI provides networking between pods across the different Kubernetes nodes.

I then verified the cluster using:

`kubectl get nodes`

The control-plane node showed a Ready status.

![Installed the CNI: Flannel](/assets/img/posts/cni.png)

## Joined the Workers

On both worker nodes, I executed the exact `kubeadm join` command generated during the control-plane initialization, with `sudo` added where required.

![Joined the Workers 1](/assets/img/posts/jw1.png)

![Joined the Workers 2](/assets/img/posts/jw2.png)

Back on `k8s-master`, I verified the cluster again:

![Ready Status](/assets/img/posts/jw12ready.png)

All three nodes were now showing Ready, confirming that the Kubernetes cluster was successfully established.

## Deployed the Sample Application

With the cluster ready, I created a simple Kubernetes Deployment using `.yaml`, then applied it to the cluster.

![Deployed the Sample Application 1](/assets/img/posts/deploy1.png)

The Deployment created and managed the application pods, while Kubernetes ensured that the desired number of replicas remained running.

![Deployed the Sample Application 2](/assets/img/posts/deploy2.png)

## Accessing the Web Application

I exposed the application using a Kubernetes Service and accessed it using `curl`.

![Accessing the Web Application 1](/assets/img/posts/c1.png)

![Accessing the Web Application 2](/assets/img/posts/c2.png)

I also accessed the application through a web browser.

![Accessing the Web Application 3](/assets/img/posts/webapc1.png)

![Accessing the Web Application 4](/assets/img/posts/webapc2.png)

Each response displayed information about the pod that handled the request, making it easy to see how traffic was being served by different replicas.

### Scaling

I then increased the number of application replicas.

Kubernetes automatically created additional pods to match the desired replica count.

![Scaling](/assets/img/posts/scaling.png)

This demonstrated one of Kubernetes' key capabilities: scaling an application without manually creating additional VMs or processes.

### Self-Healing

To demonstrate self-healing, I manually deleted one of the running application pods.

![Self-Healing 1](/assets/img/posts/sh1.png)

The Deployment's ReplicaSet controller detected that the actual number of pods no longer matched the desired state and automatically created a replacement pod.

![Self-Healing 2](/assets/img/posts/sh2.png)

This is one of the most useful Kubernetes concepts: you define the desired state, and Kubernetes continuously works to maintain it.

## Overall

This project gave me a practical understanding of how a Kubernetes environment is built from the ground up. Starting with VMware VMs, I prepared the operating system, installed the container runtime and Kubernetes components, bootstrapped the cluster with kubeadm, configured networking with Flannel, and finally deployed and exposed an application. The project also demonstrated two of the most important benefits of Kubernetes: scaling and self-healing. 

Overall, this was a valuable hands-on project for understanding Kubernetes beyond simply deploying applications. It provided a practical view of how infrastructure, cluster orchestration, networking, and workloads come together in a Kubernetes environment.           