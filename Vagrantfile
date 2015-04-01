# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  
  config.vm.network "private_network", ip: "192.168.50.7"

  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
  end

  config.vm.define "relay-jenkins" do |machine|
    machine.vm.hostname = "relay-jenkins"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.extra_vars = { ansible_ssh_user: 'vagrant' }
    ansible.groups = {
      "jenkins.development" => ["relay-jenkins"]
    }
    ansible.playbook = "jenkins.yml"
  end
end
