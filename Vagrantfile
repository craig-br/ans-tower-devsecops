# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
# HostManager plugin mconfiguration
# Command:
#   vagrant plugin install vagrant-hostmanager
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.manage_guest = true

  # Define Jenkins Node
  config.vm.define "jenkins" do |j|
    j.vm.box = "generic/rhel8"
    j.vm.network "private_network", ip: "192.168.20.11", nic_type: "virtio"
    j.vm.hostname = "jenkins.vagrant.local"
    j.vm.synced_folder ".", "/vagrant", disabled: true
    # Jenkins Node VirtualBox Customisations
    j.vm.provider :virtualbox do |v|
      v.memory = 2048
      v.cpus = 2
      v.customize ["modifyvm", :id, "--ioapic", "on"]
      v.name = "jenkins"
    end
    # Jenkins Node Ansible
    j.vm.provision "ansible" do |ansible|
      ansible.config_file = "./ansible.cfg"
      ansible.playbook = "./jenkins_deploy.yml" 
      ansible.galaxy_role_file = "./roles/requirements.yml"
      ansible.galaxy_roles_path = "./roles"
      ansible.verbose = "vvv"
      ansible.groups = {
        "ciservers" => ["jenkins"],
        "adminservers" => ["tower"],
        "qcservers" => ["sonar"]
    }
    end
  end

  # Define SonarQube Node
  config.vm.define "sonar" do |s|
    s.vm.box = "generic/rhel7"
    s.vm.network "private_network", ip: "192.168.20.12", nic_type: "virtio"
    s.vm.hostname = "sonar.vagrant.local"
    s.vm.synced_folder ".", "/vagrant", disabled: true
    # Sonar Node VirtualBox Customisations
    s.vm.provider :virtualbox do |v|
      v.memory = 2048
      v.cpus = 2
      v.customize ["modifyvm", :id, "--ioapic", "on"]
      v.name = "sonar"
    end
    # SonarQube Node Ansible
    # s.vm.provision "ansible" do |ansible|
    #   ansible.config_file = "./ansible.cfg"
    #   ansible.playbook = "./sonar_deploy.yml" 
    #   ansible.galaxy_role_file = "./roles/requirements.yml"
    #   ansible.galaxy_roles_path = "./roles"
    #   ansible.verbose = "vvv"
    #   ansible.groups = {
    #   "ciservers" => ["jenkins"],
    #   "adminservers" => ["tower"],
    #   "qcservers" => ["sonar"]
    #   }
    # end
  end

  # Define Tower Node
  config.vm.define "tower" do |tower|
    tower.vm.box = "ansible/tower"
    tower.vm.network "private_network", ip: "192.168.20.10", nic_type: "virtio"
    tower.vm.hostname = "tower.vagrant.local"
    tower.vm.synced_folder ".", "/vagrant", disabled: true
    # Tower Node VirtualBox Customisations
    tower.vm.provider "virtualbox" do |v|
      v.memory = 2048
      v.cpus = 2
      v.customize ["modifyvm", :id, "--ioapic", "on"]
      v.name = "tower"
    end
    # Tower Node Ansible
    tower.vm.provision "ansible" do |ansible|
      ansible.playbook = "./tower_deploy.yml" 
      # ansible.galaxy_role_file = "./roles/requirements.yml"
      # ansible.galaxy_roles_path = "./roles"
      ansible.verbose = "vvv"
      ansible.groups = {
        "ciservers" => ["jenkins"],
        "adminservers" => ["tower"],
        "qcservers" => ["sonar"]
      }
    end
  end
end