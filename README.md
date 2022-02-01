# RIasC Ansible Playbooks

[![DOI](https://zenodo.org/badge/340038373.svg)](https://zenodo.org/badge/latestdoi/340038373)
[![GitHub](https://img.shields.io/github/license/ERIGrid2/riasc-ansible)](https://github.com/riasc-ansible/blob/master/LICENSE)
[![Lint](https://github.com/ERIGrid2/riasc-ansible/actions/workflows/lint.yml/badge.svg)](https://github.com/ERIGrid2/riasc-ansible/actions/workflows/lint.yml)

- **Based on:** <https://github.com/k3s-io/k3s-ansible>

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

[cluster:children]
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

## Credits

- [Vincent Rabah](https://github.com/itwars)
- [Steffen Vogel](https://github.com/stv0g) [📧](mailto:post@steffenvogel.de), [Institute for Automation of Complex Power Systems](https://www.acs.eonerc.rwth-aachen.de), [RWTH Aachen University](https://www.rwth-aachen.de)

### Funding acknowledment

<img alt="European Flag" src="https://erigrid2.eu/wp-content/uploads/2020/03/europa_flag_low.jpg" align="left" style="margin-right: 10px"/> The development of [RIasC](https://riasc.eu) has been supported by the [ERIGrid 2.0](https://erigrid2.eu) project of the H2020 Programme under [Grant Agreement No. 870620](https://cordis.europa.eu/project/id/870620)
