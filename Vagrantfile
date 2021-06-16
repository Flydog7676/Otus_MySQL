# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "centos/7"

#  config.vm.provision "ansible" do |ansible|
#    ansible.verbose = "vvv"
#    ansible.playbook = "playbook.yml"
#    ansible.become = "true"
#  end

  config.vm.provider "virtualbox" do |v|
    v.memory = 256
    v.cpus = 1
  end

  config.vm.define "mysql1" do |mysql1|
    mysql1.vm.network "private_network", ip: "192.168.50.10", virtualbox__intnet: "net1"
    mysql1.vm.hostname = "mysql1"
#    mysql1.vm.provision "shell", path: "mysql1.sh"
  end

  config.vm.define "mysql2" do |mysql2|
  mysql2.vm.network "private_network", ip: "192.168.50.11", virtualbox__intnet: "net1"
  mysql2.vm.hostname = "mysql2"
#  mysql2.vm.provision "shell", path: "mysql2.sh"
  end

end
