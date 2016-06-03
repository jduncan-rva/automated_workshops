# Automated Workshops
Jamie Duncan <jduncan@redhat.com>

## Purpose

The goal of this project is to provide a collection of 'stackable' ansible playbooks to quickly and easily deploy complex demo and workshop environments.

In addition to that, it will hopefully provide corresponding asciidoc-formatted workshop guides.

## Usage

## Configuration

I am working to make all of the variables that need to be configured live in the playbooks themselves. These are typically locations of downloaded files (like database dumps or VM images that are used to build out the systems).

### passes.yml

In the base directory, you will need to use [ansible vault](http://docs.ansible.com/ansible/playbooks_vault.html) to create an encryped variables file. This needs to include the following variables.

* rhn_user : your RHN user account
* rhn_pass : your RHN account password
* packstack_ssh_priv: a private key that the packstack playbook will use to deploy packstack
* packstack_ssh_pub: the corresponding public key for `packstack_ssh_priv`

## Playbooks

### atomic-host-cluster.yml

This will build out a 3-node Atomic Host cluster with kubernetes configured. `kube0` is the controller node, with `kube1` and `kube2` acting as the worker nodes.

The atomic-demo role also deploys a sample web application and builds out gluster server containers on the 2 Atomic hosts. These are not controlled through kubernetes. This is intentional, to illustrate how things can be done out-of-band and to illustrate how Ansible can deploy container workloads.

If desired, this can be built out using a replication controller. I just haven't done it yet.

To deploy:
```
ansible-playbook atomic-host-cluster.yml -i hosts --ask-vault-pass
```

### cfme-demo.yml

This deploys a Cloudforms system and loads in the Raleigh sample database. It is based on the vagrant process other people built out over the day.

To deploy:
```
ansible-playbook cfme-demo.yml -i hosts --ask-vault-pass
```

### gluster-2-node.yml

This builds out a 2 node gluster setup that is ready to act as persistent storage.

To deploy:
```
ansible-playbook guster-2-node.yml -i hosts --ask-vault-pass
```

### packstack-demo.yml

This builds out a 2-node packstack demo using nested virtualization. It pulls from the latest stable RDO version of OpenStack. The packstack-demo role also uploads several images and artifacts into the OpenStack instance to seed it for a customer-facing demo.

To deploy:
```
ansible-playbook packstack-demo.yml -i hosts --ask-vault-pass
```

### rhev-demo.yml

This will build out a RHEV-M VM and a RHEL 7 system for it to control.

_This is currently not finished_

To deploy:
```
ansible-playbook rhev-demo.yml -i hosts --ask-vault-pass
```

### demo-cleanup.yml

Believe it or not, you CAN have VM sprawl on a single laptop. This helps stop that. You feed it the group name (from the `hosts` file) and it will clear out all of the artifacts from that demo installation.

* VM configs
* VM disks
* SSH Key Entries

To deploy:
```
ansible-playbook -i hosts --extra-vars "group=<group-name>" demo-cleanup.yml
```

Example: to clean up at atomic cluster demo:
```
ansible-playbook -i hosts --extra-vars "group=atomic" demo-cleanup.yml
```
## TODO

* start the documentation side of the tool
* destroying VMs isn't as solid as it needs to be
