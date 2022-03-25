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

  # Define controller-rhel node
  config.vm.define "controller-rhel" do |controller_rhel|
    controller_rhel.vm.box = "generic/rhel8"
    controller_rhel.vm.network "private_network", ip: "192.168.20.40", nic_type: "virtio"
    controller_rhel.vm.network "forwarded_port", guest: 443, host: 4443
    controller_rhel.vm.hostname = "controller-rhel.vagrant.local"
    controller_rhel.vm.synced_folder ".", "/vagrant", disabled: true
    # controller Node VirtualBox Customisations
    controller_rhel.vm.provider "virtualbox" do |v|
      v.memory = 5120
      v.cpus = 2
      v.customize ["modifyvm", :id, "--ioapic", "on"]
      v.name = "controller-rhel"
    end
    # Controller Node Ansible
    controller_rhel.vm.provision "ansible" do |ansible|
      ansible.playbook = "./provisioning/controller_bootstrap.yml"
      ansible.groups = {
        "ciservers" => ["jenkins"],
        "adminservers" => ["controller-rhel","controller-rhel.vagrant.local"],
        "qcservers" => ["sonar"]
      }
      ansible.host_vars = {
        "controller-rhel" => {"ansible_host" => "192.168.20.40",
                              "ansible_port" => 22,
                              "ansible_python_interpreter" => "/bin/python3" }
      }
    end
  end
end