# ansible-deploy-kubernetes-cluster

This repository is used to deploy a Kubernetes cluster using Ansible.

## Features
 - Install and configure all prerequisites to configure Control Plane, Etcd and worker nodes;
 - Deploy multi-master cluster in <a href='https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/ha-topology/' target='_blank'>stacked etc</a> mode;
 - Join worker nodes on Cluster;
 - Enalbe plugins on deploy: ArgoCD, Kyverno, cert-manager, kong;

## OS support
This playbook works on:
  - Oracle Linux 9;

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
# set username ( access needs to be done by ssh-key and needs to be )
username: cloud-user

# set kubernetes version
kubernetes_version: 1.29

# set containerd version
containerd_version: 1.7.15

# set cni plugins version
cni_plugins_version: 1.4.1

# kubeadm configurations
control_plane_endpoint: 192.168.0.91
control_plane_endpoint_port: 6443

# network plugin
network_plugin_url: https://raw.githubusercontent.com/projectcalico/calico/v3.27.3/manifests/calico.yaml

# Set etcd topology 
# https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/ha-topology/
external_etcd: false

# add cluster features
feature:
  - namespace: argocd
    name: argocd
    type: manifest
    url: https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    enabled: true
  - namespace: kong
    name: kong
    type: helm
    repo: https://charts.konghq.com
    chart_ref: kong/kong
    enabled: true
  - namespace: kyverno
    name: kyverno
    type: helm
    repo: https://kyverno.github.io/kyverno/
    chart_ref: kyverno/kyverno
    enabled: true
  - namespace: cert-manager-crd
    name: cert-manager-crd
    type: manifest
    url:  https://github.com/cert-manager/cert-manager/releases/download/v1.14.4/cert-manager.crds.yaml
    enabled: true
  - namespace: cert-manager
    name: cert-manager
    type: helm
    repo: https://charts.jetstack.io
    chart_ref: cert-manager/cert-manager
    enabled: true

```

### How to use
```bash
$ ansible-playbook main.yaml -i inventories/inventory.ini # be happy =)
```

### License
GNU General Public License v3.0 or later
