# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "geerlingguy/ubuntu1604"
  config.ssh.insert_key = false

  config.vm.provider :virtualbox do |v|
    v.name = "snap"
    v.memory = 512
    v.cpus = 2
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  config.vm.hostname = "snap"
  config.vm.network :private_network, ip: "192.168.33.33"

  # Minion code/data directory
  config.vm.synced_folder "./docroot", "/var/www/html", create: true,  mount_options: ["dmode=777", "fmode=666"]

  # Set the name of the VM. See: http://stackoverflow.com/a/17864388/100134
  config.vm.define :snap do |snap|
  end

  # Ansible provisioner.
  config.vm.provision "ansible" do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.playbook = "provisioning/playbook.yml"
    ansible.inventory_path = "provisioning/inventory"
    ansible.become = true
  end

end
