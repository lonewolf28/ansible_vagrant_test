# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  
  #create a ansible mgmt node
  config.vm.define :mgmt do |mgmt_config|
    mgmt_config.vm.box = "ubuntu/trusty32"
    mgmt_config.vm.hostname = "ansiblemgmt"
    mgmt_config.vm.network :private_network, ip: "10.0.2.16"
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
    lb_config.vm.box = "ubuntu/trusty32"
    lb_config.vm.hostname = "loadbalancer"
    mgmt_config.vm.network :private_network, ip: "10.0.2.17"
    lb_config.vm.network "forwarded_port", guest: 80, host: 8080
    lb_config.vm.provider "virtualbox" do |vb|
      vb.memory = "256"
    end
  end

  #web nodes 

  (1..2).each do |i|
    config.vm.define "web_#{i}" do |node|
      node.vm.box = "ubunut/trusty32"
      node.vm.hostname = "web#{i}"
      mgmt_config.vm.network :private_network, ip: "10.0.2.2#{i}"
      node.vm.network "forwarded_port", guest: 80, host: "808#{i}"
      node.vm.provider "virtualbox" do |vb|
        vb.memory = "256"
      end
    end
  end

end
