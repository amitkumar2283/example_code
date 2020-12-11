## Install Dependencies

sudo apt-get update && sudo apt-get install -y apt-transport-https curl

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list

deb https://apt.kubernetes.io/ kubernetes-xenial main

EOF

sudo apt-get update

sudo apt-get install -y kubelet kubeadm kubectl

## Check IP Address

ip add sh

## Initialise Cluster

kubeadm init --apiserver-advertise-address 10.128.0.14 --pod-network-cidr=192.168.0.0/16

mkdir -p $HOME/.kube

sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

sudo chown $(id -u):$(id -g) $HOME/.kube/config

## Install CNI

kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml

## Check if all pods are working

kubectl get pods -A
