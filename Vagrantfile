# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

# Needed for docker support.
Vagrant.require_version ">= 1.6.3"

Vagrant.configure(2) do |config|
  #####################################
  #### Base settings
  #####################################
  config.vm.hostname = "devbox"
  # Skip checking for an updated Vagrant box
  config.vm.box_check_update = false
  config.vm.box = "hashicorp/precise64"
  # Synchronize folders
  config.vm.synced_folder ".", "/vagrant"

  #####################################
  #### Providers
  #####################################
  config.vm.provider "virtualbox" do |vb|
    # Customize the amount of memory on the VM:
    vb.memory = "2048"
  end

  #####################################
  #### Provisioning
  #####################################
  # Install docker
  config.vm.provision "docker"
  # Install vagrant
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    test ! -e vagrant_1.8.1_x86_64.deb && wget https://releases.hashicorp.com/vagrant/1.8.1/vagrant_1.8.1_x86_64.deb
    dpkg -i vagrant_1.8.1_x86_64.deb
    vagrant -v
  SHELL
end
