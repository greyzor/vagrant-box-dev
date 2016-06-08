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
  #### Provider
  #####################################
  config.vm.provider "virtualbox" do |vb|
    host = RbConfig::CONFIG['host_os']
    #----------------------------------
    # System resources configuration:
    #----------------------------------
    #   - give me 25% of system memory.
    #   - give me all cpu cores available on host.
    #----------------------------------
    if host =~ /darwin/
      cpus = `sysctl -n hw.ncpu`.to_i
      # sysctl returns Bytes and we need to convert to MB
      mem = `sysctl -n hw.memsize`.to_i / 1024 / 1024 / 4
    elsif host =~ /linux/
      cpus = `nproc`.to_i
      # meminfo shows KB and we need to convert to MB
      mem = `grep 'MemTotal' /proc/meminfo | sed -e 's/MemTotal://' -e 's/ kB//'`.to_i / 1024 / 4
    else # sorry Windows folks, I can't help you
      cpus = 2
      mem = 1024
    end
    vb.customize ["modifyvm", :id, "--memory", mem]
    vb.customize ["modifyvm", :id, "--cpus", cpus]
  end

  #####################################
  #### Provisioning
  #####################################
  #--------------------------
  # Provision docker
  #--------------------------
  config.vm.provision "docker"

  #--------------------------
  # Provision vagrant
  #--------------------------
  # Install vagrant
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    test ! -e vagrant_1.8.1_x86_64.deb && wget https://releases.hashicorp.com/vagrant/1.8.1/vagrant_1.8.1_x86_64.deb
    dpkg -i vagrant_1.8.1_x86_64.deb
    vagrant -v
    echo "Provisioned vagrant."
  SHELL

  #--------------------------
  # Provision other dev tools
  #--------------------------
  config.vm.provision "shell", inline: <<-SHELL
    apt-get install -y vim htop
    apt-get install zsh git-core
    sudo su vagrant -c "wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh"
    chsh -s $(which zsh) vagrant  # make zsh the default shell
                                  #  for vagrant user.
    cp /tmp/vagrant_dir/.zshrc /home/vagrant/.zshrc
  SHELL
end
