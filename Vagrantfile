# -*- mode: ruby -*-
# vi: set ft=ruby :

# support older styles for backwards compatibility
Vagrant.configure("2") do |config|


  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "centos/7"
  config.vm.box_version = "1803.01"

  # Disable automatic box update checking. boxes will only be checked for
  # updates when the user runs `vagrant box outdated`.
  config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM.
  # The first argument is the path on the host
  # config.vm.synced_folder "../data", "/vagrant_data"

  config.vm.define "NYH-Group1-NameNode" do |ambari|
    ambari.vm.hostname = "NYH-Group1-NameNode"
    ambari.vm.network "private_network", ip: "192.168.50.2"

    ambari.vm.provider "virtualbox" do |vb|
      # Customize the amount of memory and vcpu on the VM:
      vb.customize ["modifyvm", :id, "--cpus", "4", "--memory", "6144"]
    end
  end

  # config.vm.provision "shell", inline: "echo Hello"
  N = 2
  (1..N).each do |i|
    config.vm.define "NYH-Group1-DataNode#{i}" do |node|
      node.vm.hostname = "NYH-Group1-DataNode#{2+i}"
      node.vm.network "private_network", ip: "192.168.50.#{2+i}"

      node.vm.provider "virtualbox" do |vb|
        # Customize the amount of memory and vcpu on the VM:
        vb.customize ["modifyvm", :id, "--cpus", "2", "--memory", "2048"]
      end

      # Only execute once the Ansible provisioner,
      # when all the machines are up and ready.
      if i == N
        # Use :ansible or :ansible_local
        node.vm.provision :ansible do |ansible|
          ansible.verbose = "v"
          ansible.limit = "all"
          ansible.playbook = "provisioning/base_node.yaml"
          # ansible.inventory_path = "static_inventory"
          ansible.groups = {
            "ambari" => ["NYH-Group1-NameNode"],
            "datanodes" => ["NYH-Group1-DataNode[1:#{i}]"],
            "all_groups:children" => ["ambari", "datanodes"],
            # "group1:vars" => {"variable1" => 9,
            #                   "variable2" => "example"}
          }
        end
      end
    end
  end

  # 1 x Active NameNode
  # 1 x Standby NameNode (for HA)
  # 1 x Resource Manager
  # 3 x DataNodes (minimum)

  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
