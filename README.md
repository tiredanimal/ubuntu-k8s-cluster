# ubuntu-k8s-cluster
POC for running k8s on Ubuntu 20.04 using virtualbox on Windows (though could be used on MAC by changing DIR_HOME = ENV['USERPROFILE'] to DIR_HOME = ENV['HOME']

## Prereqs

* SSH public key for your user profile must exist already it can be created via an ssh terminal such as Git bash for Windows

* Virtual box installed

* Vagrant installed

* Install "vagrant-vbguest" via the following command to sync folders to cluster

**vagrant plugin install vagrant-vbguest**

## Bring up cluster

**vagrant up**

## Destroy cluster

**vagrant destroy --force**

## Log into each node of the cluster

Log into master via the following command:

**ssh vagrant@127.0.0.1 -p 2222**

Log into node 1 via the following command:

**ssh vagrant@127.0.0.1 -p 2200**

Log into node 2 via the following command: 

**ssh vagrant@127.0.0.1 -p 2201**

## Initialise Kubernetes Cluster

**sudo kubeadm init --pod-network-cidr=10.244.0.0/16**

Note the details to join worker nodes to the K8s cluster

*kubeadm join 10.0.2.15:6443 --token iki90v.872ptxbbiky5wiwk \
    --discovery-token-ca-cert-hash sha256:5366e126af0f25d1c176a89675caac30bb86a1425066bab3f1fb6fca6173881d*

