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

   In addition, install the following Vagrant's plugins:

```sh
vagrant plugin install vagrant-share
vagrant plugin install vagrant-vbguest
vagrant plugin install vagrant-hostmanager
```

3. Follow the steps below on shell / command line:

```sh
# clone this git repository
git clone git@gitlab.com:nydoop/hadoop-infra.git

# navigate to the root directory of the cloned repo
cd hadoop-infra

# build the entire VM for the cluster
# vagrant will call ansible right after VM provisioning to create cluster
# you might need to wait for awhile (e.g. 20-60 mins) depending on macine specs and internet speed
vagrant up

# to remove VM (when no longer needed after use)
vagrant destroy

```

3. Access the ambari web url [http://n-group3-nydoop:8080](http://n-group3-nydoop:8080)

 - username: admin
 - password: admin

4. To access Zeppelin, go to [http://n-group3-nydoop:9995](http://n-group3-nydoop:9995)

 - username: admin
 - password: admin

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


Discovered Issues
-------------------

- Using Blueprints, hdfs ha can be setup directly
- zookeeper will not work if hostname is resolved to loopback address (127.0.0.1) in /etc/hosts
- ambari admin user should have their user directory created for ambari views to work
