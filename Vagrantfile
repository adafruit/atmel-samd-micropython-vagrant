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
  config.vm.box = "ubuntu/trusty64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder "./vagrant", "/vagrant", create: true
  # Optionally share the /home/vagrant/source directory with NFS (only for Mac
  # and Linux host machines, for Windows try SMB below).
  #config.vm.synced_folder "./source", "/home/vagrant/source", type: "nfs", create: true
  # Optionally share the /home/vagrant/source directory with SMB (only for Windows
  # host machines, for Mac OSX or Linux try NFS above).
  #config.vm.synced_folder "./source", "/home/vagrant/source", type: "smb", create: true

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    # Enable USB.
    vb.customize ["modifyvm", :id, "--usb", "on"]
    # Add a USB passthrough for the SAMD21 bootloader VID & PID.
    vb.customize ["usbfilter", "add", "0", "--target", :id, "--name", "Adafruit M0 Bootloader", "--vendorid", "0x239a", "--productid", "0x000b"]
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    echo "Installing dependencies..."
    sudo add-apt-repository -y ppa:team-gcc-arm-embedded/ppa

    sudo apt-get update -qq
    sudo apt-get install -y python3 gcc-multilib pkg-config libffi-dev libffi-dev:i386 qemu-system gcc-mingw-w64

    sudo apt-get install -y build-essential git linux-image-extra-$(uname -r) libreadline-dev wx2.8-headers libwxgtk2.8-0 libwxgtk2.8-dev autoconf libtool libffi-dev pkg-config
    sudo apt-get install -y --force-yes gcc-arm-embedded

    echo "Building and installing BOSSA..."
    cd ~
    git clone https://github.com/shumatech/BOSSA.git
    cd BOSSA
    make bin/bossac
    sudo cp bin/bossac /usr/local/bin/

    echo "Cloning MicroPython source..."
    mkdir -p ~/source
    cd ~/source
    git clone https://github.com/adafruit/circuitpython.git

    cd circuitpython
    git submodule update --init --recursive

    echo "Finished provisioning!  Use the 'vagrant ssh' command to enter VM.  CircuitPython source is in the /home/vagrant/source/circuitpython folder."
  SHELL
end
