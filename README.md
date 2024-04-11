# ansible-deploy-kubernetes-cluster

This repository is used to deploy a Kubernetes cluster using Ansible.

## Features
 - Install and configure all prerequisites to configure Control Plane, Etcd and worker nodes;
 - Deploy multi-master cluster in <a href='https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/ha-topology/' target='_blank'>stacked etc</a> mode;
 - Join worker nodes on Cluster;

## OS support
This playbook works on:
  - Oracle Linux 8;

## Configurations

### Inventory

Edit ```inventory/inventory.ini``` and change with your own configuration.

```ini
[nodes]
node-01 ansible_host=192.168.0.211
node-02 ansible_host=192.168.0.212
node-03 ansible_host=192.168.0.213
node-04 ansible_host=192.168.0.214

[control_plane]
node-01 ansible_host=192.168.0.211
node-02 ansible_host=192.168.0.211

[workers]
node-03 ansible_host=192.168.0.213
node-04 ansible_host=192.168.0.214
```

### Variables

Edit ```defaults/main.yml``` and change with your own configuration.
```yaml
# username to authenticate on Linux
username: username 

# set kubernetes version
kubernetes_version: 1.27.1-0

# set containerd version
containerd_version: 1.6.20

# set cni plugins version
cni_plugins_version: 1.2.0

# kubeadm configurations
control_plane_endpoint: load-balancer # you need a balance to configure a multi-master cluster
control_plane_endpoint_port: 6443

# network plugin url
network_plugin_url: https://raw.githubusercontent.com/projectcalico/calico/v3.25.1/manifests/calico.yaml
```

### How to use
```bash
$ ansible-playbook main.yaml -i inventories/inventory.ini # be happy =)
```

### License
GNU General Public License v3.0 or later
