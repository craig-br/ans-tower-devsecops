# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
# HostManager plugin configuration
# Command:
#   vagrant plugin install vagrant-hostmanager
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.manage_guest = true
  config.hostmanager.include_offline = true

  # Define tower-rhel node
  config.vm.define "tower-rhel" do |tower_rhel|
    tower_rhel.vm.box = "generic/rhel8"
    tower_rhel.vm.network "private_network", ip: "192.168.20.40", nic_type: "virtio"
    tower_rhel.vm.network "forwarded_port", guest: 443, host: 4443
    tower_rhel.vm.hostname = "tower-rhel.vagrant.local"
    tower_rhel.vm.synced_folder ".", "/vagrant", disabled: true
    # Tower Node VirtualBox Customisations
    tower_rhel.vm.provider "virtualbox" do |v|
      v.memory = 5120
      v.cpus = 2
      v.customize ["modifyvm", :id, "--ioapic", "on"]
      v.name = "tower-rhel"
    end
    # Tower Node Ansible
    tower_rhel.vm.provision "ansible" do |ansible|
      ansible.playbook = "./provisioning/tower_bootstrap.yml"
      ansible.verbose = "vvv"
      ansible.groups = {
        "ciservers" => ["jenkins"],
        "adminservers" => ["tower-rhel"],
        "qcservers" => ["sonar"]
      }
    end
  end
end