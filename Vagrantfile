# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
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
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    # vb.gui = true

    # Customize the amount of memory on the VM:
    vb.memory = "4096"
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
  config.vm.provision "shell", inline: <<-SHELL
    echo "\n----- Installing Bison Flex and xauth ------\n"
    apt-get update
    apt-get install -y build-essential flex bison vim xauth

    echo "\n----- Installing Java 8 ------\n"
    add-apt-repository -y ppa:webupd8team/java
    apt-get update
    apt-get -y upgrade
    echo debconf shared/accepted-oracle-license-v1-1 select true | sudo debconf-set-selections
    echo debconf shared/accepted-oracle-license-v1-1 seen true | sudo debconf-set-selections
    apt-get -y install oracle-java8-installer
    SHELL

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    echo "\n----- Installing Eclipse ------\n"
    cd /home/vagrant
    wget http://mirror.tspu.ru/eclipse/technology/epp/downloads/release/neon/2/eclipse-cpp-neon-2-linux-gtk-x86_64.tar.gz
    tar -xzvf eclipse-cpp-neon-2-linux-gtk-x86_64.tar.gz
    rm eclipse-cpp-neon-2-linux-gtk-x86_64.tar.gz

    echo "\n----- Installing Eclipse bison plugin ------\n"
    mkdir -p eclipse/dropins/plugins
    cd eclipse/dropins/plugins
    wget http://cfile29.uf.tistory.com/attach/1601F93A4EEF0A0E2A0E88
    mv 1601F93A4EEF0A0E2A0E88 BisonParser_1.0.0.jar

    # Window X envs
    source ~/.profile && [ -z "$LANG" ] && echo "export LANG=en_US.UTF-8" >> ~/.profile
    source ~/.profile && [ -z "$LANGUAGE" ] && echo "export LANGUAGE=en_US.UTF-8" >> ~/.profile
    source ~/.profile && [ -z "$LC_ALL" ] && echo "export LC_ALL=en_US.UTF-8" >> ~/.profile
    source ~/.profile && [ -z "$LC_CTYPE" ] && echo "export LC_CTYPE=en_US.UTF-8" >> ~/.profile
    # https://bugs.launchpad.net/ubuntu/+source/at-spi2-core/+bug/1193236
    source ~/.profile && [ -z "$NO_AT_BRIDGE" ] && echo "export NO_AT_BRIDGE=1" >> ~/.profile
  SHELL

  config.ssh.forward_agent = true
  config.ssh.forward_x11 = true
end
