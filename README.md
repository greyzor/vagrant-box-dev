# Vagrant Development Box
Provides a development box with the current elements provisioned:
* docker
* vagrant
* zsh
* some additional dev tools.

### Features
* Automatic allocation of resources depending on OS/cpu/mem.
* On-the-flight synchronization of dev directories using WORKING_DIRS env variable.
* Pre-customized .zshrc

### Commands
Simply start the box with: `vagrant up`

Or start with additional directories synced: `WORKING_DIRS="~/my-app,~/my-other-app" vagrant up`

Connect to the box using: `vagrant ssh`

Then list docker containers/images with: `docker ps -a` and `docker images
