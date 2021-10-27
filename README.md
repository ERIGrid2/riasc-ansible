# RIasC Ansible Playbooks

[![DOI](https://zenodo.org/badge/340038373.svg)](https://zenodo.org/badge/latestdoi/340038373)

- **Based on:** <https://github.com/k3s-io/k3s-ansible>
- **Author:** <https://github.com/itwars>

## Introduction

This repository container a set of Ansible roles and playbooks to provision [RIasC](https://riasc.eu) nodes.

Specifically it contains roles for provisioning a [k3s](https://k3s.io) Kubernetes cluster on a fleet of Raspberry Pi nodes.
In addition it prepares the nodes by installing the [Wireguard](https://wireguard.com) VPN kernel module and a devicetree overlay required for [GPS-based time-synchronization](https://riasc.eu/docs/usage/time-sync).
At last it installs the [RIasC Helm chart](https://github.com/ERIGrid2/charts).

These playbooks are usually used by the [`riasc-update.sh`](https://github.com/ERIGrid2/riasc-provisioning/blob/master/common/riasc-update.sh) script from the [riasc-provisioning](https://github.com/ERIGrid2/riasc-provisioning) repo.

Currently, the repo also contains the Ansible inventory for the RIasC deployment used in the ERIGrid 2.0 project.
However, it its planned to move this inventory to a separate repo.

## Documentation

For further documentation, please consult: https://riasc.eu/docs/

## K3s Ansible Playbook

Build a Kubernetes cluster using Ansible with k3s. The goal is easily install a Kubernetes cluster on machines running:

- [X] Debian
- [X] Ubuntu
- [X] CentOS

on processor architecture:

- [X] x64
- [X] arm64
- [X] armhf

## System requirements

Deployment environment must have Ansible 2.4.0+
Master and nodes must have passwordless SSH access

## Usage

First create a new directory based on the `sample` directory within the `inventory` directory:

```bash
cp -R inventory/sample inventory/my-cluster
```

Second, edit `inventory/my-cluster/hosts.ini` to match the system information gathered above. For example:

```bash
[master]
192.16.35.12

[node]
192.16.35.[10:11]

[k3s_cluster:children]
master
node
```

If needed, you can also edit `inventory/my-cluster/group_vars/all.yml` to match your environment.

Start provisioning of the cluster using the following command:

```bash
ansible-playbook site.yml -i inventory/my-cluster/hosts.ini
```

## Kubeconfig

To get access to your **Kubernetes** cluster just

```bash
scp debian@master_ip:~/.kube/config ~/.kube/config
```
