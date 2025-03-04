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

The "apiserver-advertise-address" has to be used due to multiple nics on host

**sudo kubeadm init --kubernetes-version=v1.16.13 --apiserver-advertise-address=192.168.10.40 --pod-network-cidr=10.244.0.0/16**

Note the details to join worker nodes to the K8s cluster

*kubeadm join 192.168.10.40:6443 --token xd4xb4.ogvlwxf2lw0szij2 \
    --discovery-token-ca-cert-hash sha256:13975cbd201a26ade0f307f4e1c05672871594bf61d5c60acf83321f2491f791*


## Create K8s user

Create k8s user using the following command

**sudo useradd -m -G sudo,docker -c "K8s User" -s /bin/bash k8susr**

Remember to assign a password or ssh key

Create config with the following commands:

**su - k8susr**

**mkdir -p $HOME/.kube**

**sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config**

**sudo chown $(id -u):$(id -g) $HOME/.kube/config**

## Deploy pod network to cluster

As k8susr issue the following commands:

**kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml**

**kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/k8s-manifests/kube-flannel-rbac.yml**

Confirm that everything is up and ready

**kubectl get pods --all-namespaces**

## On each worker node join to cluster as root


Join to cluster

**sudo kubeadm join 192.168.10.40:6443 --token xd4xb4.ogvlwxf2lw0szij2 \
    --discovery-token-ca-cert-hash sha256:13975cbd201a26ade0f307f4e1c05672871594bf61d5c60acf83321f2491f791**

Join each worker node to cluster as root

**kubectl get nodes**

## Start hacking 

Start deploying services and add pods to cluster



