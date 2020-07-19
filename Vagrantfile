# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.manage_guest = true
  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
 config.vm.define "tower" do |tower|
   tower.vm.box = "ansible/tower"
   tower.vm.network "private_network", ip: "192.168.20.10", nic_type: "virtio"
#    tower.vm.network "private_network", virtualbox__intnet: "ansible_demo_mgmt", ip: "192.168.20.10", nic_type: "virtio"
   tower.vm.hostname = "tower.vagrant.local"
   tower.vm.synced_folder ".", "/vagrant", disabled: true
#   Spec
    tower.vm.provider "virtualbox" do |v|
      v.memory = 4096
      v.cpus = 2
      v.customize ["modifyvm", :id, "--ioapic", "on"]
      v.name = "tower"
    end
  end
  config.vm.define "jenkins" do |j|
    j.vm.box = "generic/rhel8"
    j.vm.network "private_network", ip: "192.168.20.11", nic_type: "virtio"
 #    tower.vm.network "private_network", virtualbox__intnet: "ansible_demo_mgmt", ip: "192.168.20.10", nic_type: "virtio"
    j.vm.hostname = "jenkins.vagrant.local"
    j.vm.synced_folder ".", "/vagrant", disabled: true
 #   Spec
    j.vm.provider :virtualbox do |v|
      v.memory = 2048
      v.cpus = 2
      v.customize ["modifyvm", :id, "--ioapic", "on"]
      v.name = "jenkins"
    end
#  Configuration
    j.vm.provision "ansible" do |ansible|
      ansible.config_file = "./ansible.cfg"
      ansible.playbook = "./jenkins_deploy.yml" 
      ansible.galaxy_role_file = "./roles/requirements.yml"
      ansible.galaxy_roles_path = "./roles"
        #ansible.ask_vault_pass = true
      ansible.verbose = "vvv"
      ansible.become = true
      ansible.groups = {
        "ciservers" => ["jenkins"]
      }
    end
    
end

#   config.vm.define "tower" do |tower|
#     tower.vm.box = "generic/rhel7"
#     tower.vm.network "private_network", virtualbox__natnet: "ansible_demo_mgmt", ip: "192.168.20.10", nic_type: "virtio"
#     tower.vm.hostname = "tower.vagrant.local"
#     tower.vm.synced_folder ".", "/vagrant", disabled: true
# 		#Spec
#     tower.vm.provider "virtualbox" do |v|
#       v.memory = 4096
#       v.cpus = 2
#     end
# 		#Configuration
# 		config.vm.provision :ansible do |ansible|
# 			ansible.config_file = "./ansible.cfg"
# 			ansible.playbook = "./playbooks/tower_install.yml" 
# 			ansible.galaxy_role_file = "./roles/requirements.yml"
# 			ansible.galaxy_roles_path = "./roles"
# 			#ansible.ask_vault_pass = true
# 			ansible.verbose = "vvv"
# 			ansible.groups = {
# 				"adminservers" => ["tower"]
# 			}
# 		end
#   end
  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
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
