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
git clone git@github.com:chewbh/ansible_hadoop.git

# navigate to the root directory of the cloned repo
cd ansible_hadoop

# build the entire VM for the cluster
# vagrant will call ansible right after VM provisioning to create cluster
# you might need to wait for awhile (e.g. 20-60 mins) depending on macine specs and internet speed
vagrant up

# to remove VM (when no longer needed after use)
vagrant destroy

```

3. Access the ambari web url [http://n-group3-nn1:8080](http://n-group3-nn1:8080)

 - username: admin
 - password: admin

4. To access Zeppelin, go to [http://n-group3-nn1:9995](http://n-group3-nn1:9995)

 - username: admin
 - password: admin

5. Configure Zeppelin's interpreter to ensure access to EDW directly vis JDBC

    1. Access Zeppelin, go to [http://n-group3-nn1:9995](http://n-group3-nn1:9995)
    2. After login, navigate to [interpreter configuration](http://n-group3-nn1:9995/#/interpreter)
    3. Click on `create` button
    4. fill in `edw` for **Interpreter Name**
    5. select `jdbc` for **Interpreter group**
    6. configure the **Properties** and **Dependencies** section according to the screenshot below

![mysql_zeppelin_interpreter](/screenshots/mysql_zeppelin_interpreter.png)

6. Add Ambari Views for Pig (by default, Ambari did not setup this. Have to add manually via Ambari UI)

    1. Access Ambari , go to [http://n-group3-nn1:8080](http://n-group3-nn1:8080)
    2. After login, navigate to [view setting](http://n-group3-nn1:8080/views/ADMIN_VIEW/2.6.2.2/INSTANCE/#/views)
    3. Click on `Pig` and then click on the `Create Instance` button
    4. fil in `pig_editor` for **Instance Name**
    5. fil in `Pig View` for **Display Name**
    6. fil in `View for authoring and managing Pig scripts` for **Description**
    7. Left all setting as default
    8. Click on `Save` button at end of the form

7. Add Ambari Views for Oozie Workflow Manager (by default, Ambari did not setup this. Have to add manually via Ambari UI)

    1. Access Ambari , go to [http://n-group3-nn1:8080](http://n-group3-nn1:8080)
    2. After login, navigate to [view setting](http://n-group3-nn1:8080/views/ADMIN_VIEW/2.6.2.2/INSTANCE/#/views)
    3. Click on `Workflow_Manager` and then click on the `Create Instance` button
    4. fil in `oozie_wfm` for **Instance Name**
    5. fil in `Oozie Workflow View` for **Display Name**
    6. fil in `View for authoring and managing Oozie Workflow` for **Description**
    7. Left all setting as default
    8. Click on `Save` button at end of the form


Cloud Environment
------------------

1. Install Ansible locally (for Linux OS only).

2. Follow the steps below on shell / command line:

```sh
# clone this git repository
git clone git@github.com:chewbh/ansible_hadoop.git

# navigate to the root directory of the cloned repo
cd ansible_hadoop/provisioning

# run the ansible playbooks
ansible-playbook -i production create_hadoop_cluster.yaml
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
