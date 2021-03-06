# Kubeup
Simple Kubernetes on CentOS 7 based on
[kubeadm](http://kubernetes.io/docs/admin/kubeadm/). Default setup is a single
master with three nodes

> NOTE: Currently libvirt only. Support for VirtualBox will be available soon

## Requirements

Install qemu-kvm, libvirt, vagrant-libvirt, and ansible

### Fedora

```
sudo dnf -y install qemu-kvm libvirt vagrant-libvirt ansible
```

You will also need to have kubectl on your system. You can install it by going
to https://kubernetes.io/docs/tasks/tools/install-kubectl/ .

### CentOS

* Run the following:

```
sudo yum install epel-release
sudo yum install qemu libvirt libvirt-devel ruby-devel gcc qemu-kvm ansible
```

* Install Vagrant: https://www.vagrantup.com/downloads.html
* Install the libvirt plugin for vagrant:

```
vagrant plugin install vagrant-libvirt
```

## Usage

To setup type:

```
$ sudo ./up.sh
```

The Kubernetes configuration is then copied from the master node to the host and can be used as follows:

```
$ kubectl --kubeconfig=kubeconfig.conf get nodes
NAME                  STATUS   ROLES    AGE     VERSION
lpabon-k8s-1-master   Ready    master   8m12s   v1.19.5
lpabon-k8s-1-node0    Ready    <none>   7m44s   v1.19.5
lpabon-k8s-1-node1    Ready    <none>   7m44s   v1.19.5
lpabon-k8s-1-node2    Ready    <none>   7m45s   v1.19.5
```

# Exporting
Kubeup informs kubeadm to create a cert with the addition of the IP of the host.
This will allow you to forward a port from the host to the Kubernetes cluster
running on the VM allowing access from the external host network.

You will need to do the following once the cluster is running:

1. Use socat to forward a port from the host to the VM:

```
socat TCP-LISTEN:6443,fork,reuseaddr TCP:<IP of host>:6443
```

2. Edit the kubeconfig.conf generated and change the server ip from the VM's ip
   to the ip of the host.

