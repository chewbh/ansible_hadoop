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

  NN = 2
  (1..NN).each do |i|
    config.vm.define "n-group3-nn#{i}" do |ambari|
      ambari.vm.hostname = "N-Group3-NN#{i}"
      ambari.vm.network "private_network", ip: "192.168.50.#{1+i}"

      ambari.vm.provider "virtualbox" do |vb|
        # Customize the amount of memory and vcpu on the VM:
        vb.customize ["modifyvm", :id, "--cpus", "2", "--memory", "4096"]
      end
      ambari.vm.provider "vmware_desktop" do |v|
        # Customize the amount of memory and vcpu on the VM:
        v.vmx["numvcpus"] = "2"
        v.vmx["memsize"] = "4096"
      end
    end
  end

  # config.vm.provision "shell", inline: "echo Hello"
  N = 3
  (1..N).each do |i|
    config.vm.define "n-group3-dn#{i}" do |node|
      node.vm.hostname = "N-Group3-DN#{i}"
      node.vm.network "private_network", ip: "192.168.50.#{3+i}"

      node.vm.provider "virtualbox" do |vb|
        # Customize the amount of memory and vcpu on the VM:
        vb.customize ["modifyvm", :id, "--cpus", "2", "--memory", "4096"]
      end
      node.vm.provider "vmware_desktop" do |v|
        # Customize the amount of memory and vcpu on the VM:
        v.vmx["numvcpus"] = "2"
        v.vmx["memsize"] = "4096"
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
            "ambari-server" => ["n-group3-nn1"],
            "ambari-client" => ["n-group3-nn2", "n-group3-dn[1:#{i}]"],
            "name-nodes" => ["n-group3-nn[1:#{i}]"],
            "data-nodes" => ["n-group3-dn[1:#{i}]"],
            "all_groups:children" => ["ambari-server", "ambari-client", "name-nodes", "data-nodes"]
          }
        end
      end
    end
  end

end
