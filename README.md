# k3s
install and work with k3s and run nginx on it
#master
hostnamectl set-hostname "k3s-master" && exec bash
#worker
hostnamectl set-hostname "k3s-worker01" && exec bash

#master and worker
vi /ets/hosts
add this lines:
192.168.1.161 k3s-master
192.168.1.163 k3s-worker01


#master
sudo firewall-cmd --permanent --add-port=6443/tcp
sudo firewall-cmd --permanent --add-port=8472/udp
sudo firewall-cmd --permanent --add-port=10250/tcp
sudo firewall-cmd --permanent --add-port=51820/udp
sudo firewall-cmd --permanent --add-port=51821/udp
sudo firewall-cmd --permanent --zone=trusted --add-source=10.42.0.0/16
sudo firewall-cmd --permanent --zone=trusted --add-source=10.43.0.0/16
sudo firewall-cmd --reload

#worker
sudo firewall-cmd --permanent --add-port=8472/udp
sudo firewall-cmd --permanent --add-port=10250/tcp
sudo firewall-cmd --permanent --add-port=51820/udp
sudo firewall-cmd --permanent --add-port=51821/udp
sudo firewall-cmd --permanent --zone=trusted --add-source=10.42.0.0/16
sudo firewall-cmd --permanent --zone=trusted --add-source=10.43.0.0/16
sudo firewall-cmd --reload


#master
curl -sfL https://get.k3s.io | sh -

mkdir ~/.kube
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
sudo chown $USER ~/.kube/config
sudo chmod 600 ~/.kube/config
export KUBECONFIG=~/.kube/config

cat /var/lib/rancher/k3s/server/node-token;
K10ef303b4ec3edfa9add8c69f669b4a698c68c5f0f38846b5e72e37e27cd86e2f0::server:a79222126c816c9e9a68767e7195e512

#worker

curl -sfL https://get.k3s.io | K3S_URL=https://k3s-master:6443 K3S_TOKEN=K10ef303b4ec3edfa9add8c69f669b4a698c68c5f0f38846b5e72e37e27cd86e2f0::server:a79222126c816c9e9a68767e7195e512 sh -;

