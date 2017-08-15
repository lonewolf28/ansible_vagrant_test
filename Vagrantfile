# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  
  #create a ansible mgmt node
  config.vm.define :mgmt do |mgmt_config|
    mgmt_config.vm.box = "ubuntu/trusty"
    mgmt_config.vm.hostname = "ansible_mgmt"
    mgmt_config.vm.provider "virtualbox" do |vb|
      vb.memory = "256"
    end
    mgmt_config.vm.provision :shell, inline: <<-SHELL
      apt-get -y install software-properties-common
      apt-add-repository -y ppa:ansible/ansible
      apt-get update
      apt-get -y install ansible
    SHELL
  end

  #create a loadbalancer

  config.vm.define :lb do |lb_config|
    lb_config.vm.box = "ubuntu/trusty"
    lb_config.vm.hostname = "loadbalancer"
    lb_config.vm.network "forwarded_port", guest: 80, host: 8080
    lb_config.vm.provider "virtualbox" do |vb|
      vb.memory = "256"
    end
  end

  #web nodes 

  (1..2).each do |i|
    config.vm.define "web_#{i}" do |node|
      node.vm.box = "ubunut/trusty"
      node.vm.hostname = "web_#{i}"
      node.vm.network "forwarded_port", guest: 80, host: "808#{i}"
      node.vm.provider "virtualbox" do |vb|
        vb.memory = "256"
      end
    end
  end

end