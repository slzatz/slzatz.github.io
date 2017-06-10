Commands:

```vagrant up```

The vagrant file:

    # -*- mode: ruby -*-
    # vi: set ft=ruby :

    # All Vagrant configuration is done below. The "2" in Vagrant.configure
    # configures the configuration version (we support older styles for
    # backwards compatibility). Please don't change it unless you know what
    # you're doing.
    Vagrant.configure(2) do |config|
      # The most common configuration options are documented and commented below.
      # For a complete reference, please see the online documentation at
      # https://docs.vagrantup.com.

      # Every Vagrant development environment requires a box. You can search for
      # boxes at https://atlas.hashicorp.com/search.
      config.vm.box = "ubuntu/xenial64" 
      #config.ssh.username = "xxx"
      #config.ssh.password = "xxx"
      #config.ssh.insert_key = false
      config.vm.synced_folder "vagrant/", "/home/ubuntu/vagrant"

      # Provision script to install dependencies used by the esp-open-sdk and
      # micropython tools.  First install dependencies as root.
      # the <<- is some Ruby convention for defining a multi-line string
      # It looks like adding python-dev and libtool-bin cover what was in the adafruit list which included build-essential
      # for unix build need libffi-dev pkg-config
      # I took this out of below -- it's crucial for shared folder but doesn't seem to work in vagrantfile: virtualbox-guest-dkms
      config.vm.provision "shell", privileged: false, inline: <<-SHELL
        echo "Installing esp-open-sdk and micropython dependencies..."
        sudo apt-get update
        sudo apt-get install -y make unrar autoconf automake libtool gcc g++ gperf flex bison texinfo gawk ncurses-dev libexpat-dev python python-serial sed git unzip bash help2man python-dev libtool-bin
        echo "Installing esp-open-sdk and micropython source..."
        git clone --recursive https://github.com/pfalcon/esp-open-sdk.git
        git clone https://github.com/micropython/micropython.git
        echo "Finished provisioning, now run 'vagrant ssh' to enter the virtual machine."
      SHELL
    #For some reason libtool alone wasn't enough and had to also sudo apt-get install libtool-bin
      # Virtualbox VM configuration.
      config.vm.provider "virtualbox" do |v|
        # Bump the memory allocated to the VM up to 1 gigabyte as the compilation of
        # the esp-open-sdk tools requires more memory to complete.
        v.memory = 1024
      end

    end
