# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

# Needed for docker support.
Vagrant.require_version ">= 1.6.3"

###########################################################
# Synchronize additional working directories
###########################################################
working_dirs=[]
if ENV['WORKING_DIRS']
  working_dirs = ENV['WORKING_DIRS'].split(',')
  working_dirs.each do |dir|
    if ! Dir.exists?(dir)
      abort("Non existing directory: #{dir} !\nAborting..")
    end
  end
end

Vagrant.configure(2) do |config|
  #####################################
  #### Base settings
  #####################################
  config.vm.hostname = "devbox"
  # Skip checking for an updated Vagrant box
  config.vm.box_check_update = false
  config.vm.box = "hashicorp/precise64"

  #------------------------------------
  # Synchronize folders
  #------------------------------------
  config.vm.synced_folder ".", "/tmp/vagrant_dir"
  working_dirs.each do |dir|
    config.vm.synced_folder dir, "/opt/"+dir.split('/')[-1]
  end

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
