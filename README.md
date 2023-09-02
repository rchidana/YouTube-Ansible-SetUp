# Ansible Set-Up Documentation on Windows using Vagrant

#### Install Vagrant : https://developer.hashicorp.com/vagrant/downloads 

#### Pre-Req : Vagrant needs at least one Hyper-Visor (Oracle VirtualBox : https://www.virtualbox.org/wiki/Downloads)

<br> Vagrantfile </br>

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  
  # Will not check for box updates during every startup.
  config.vm.box_check_update = false


  # Master node where ansible will be installed
  config.vm.define "controller" do |controller|
    controller.vm.box = "geerlingguy/ubuntu2004"
    controller.vm.hostname = "controller.crudsinfotech.com"
    controller.vm.network "private_network", ip: "192.168.10.10"
    controller.vm.provider "virtualbox" do |vb|
      vb.memory = 1048
      vb.cpus = 1
    end
  end

  # Managed node 1.
  config.vm.define "ubuntu1" do |ubuntu1|
    ubuntu1.vm.box = "geerlingguy/ubuntu2004"
    ubuntu1.vm.hostname = "ubuntu1.crudsinfotech.com"
    ubuntu1.vm.network "private_network", ip: "192.168.10.20"
    ubuntu1.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 1
    end
  end

end

```
<br>Instructions to spin up VMs </br>
```
# Run this command from the folder where Vagrantfile is present
vagrant up
vagrant status
# Once both the VMs are running fine, get into the controller VM
vagrant ssh controller
```

<br> Instructions to Install Ansible in Ubuntu VM </br>
```
# Run these commands inside the 'controller' VM
sudo apt-get update -y
sudo apt-add-repository -y ppa:ansible/ansible
sudo apt-get update -y
sudo apt-get install ansible
ansible --version

# Let us try some adhoc commands against localhost
ansible localhost -m "ping"
ansible localhost -a "hostname"

```

