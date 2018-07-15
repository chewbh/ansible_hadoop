## Hadoop Infra Setup

Vagrant and Ansible playbooks for building a hadoop cluster with Ambari server.
For a full list of supported features check below.

- Includes building the infrastructure and taking care of the prerequisites
- Build the nodes, prepare them (OS settings, database, KDC, etc) and then install Ambari
- Create the cluster using Ambari Blueprints

It supports both local setup and public cloud setup.

Local Environment
------------------

Both Vagrant and Ansible are used to automate the entire Vm setup in VirtualBox environment.


1. Install Vagrant locally. Follows instruction on https://www.vagrantup.com/intro/getting-started/install.html

2. Install Ansible locally (for Linux OS only).

3. Follow the steps below on shell / command line:

```sh
# clone this git repository
git clone git@gitlab.com:nydoop/hadoop-infra.git

# navigate to the root directory of the cloned repo
cd hadoop-infra

# build the entire VMs for the cluster
# vagrant will call ansible right after VM provisioning
vagrant up

# to remove all VMs (when no longer needed after use)
vagrant destroy

```

Cloud Environment
------------------

2. Install Ansible locally (for Linux OS only).

3. Follow the steps below on shell / command line:

```sh
# clone this git repository
git clone git@gitlab.com:nydoop/hadoop-infra.git

# navigate to the root directory of the cloned repo
cd hadoop-infra

# run the ansible playbooks
ansible-playbook -i /developments/hadoop-infra/.vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory provisioning/base_node.yaml

```

Requirements
-------------

- Ansible 2.6+

- Expects CentOS/RHEL hosts

