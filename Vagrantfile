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
  config.vm.box = "ubuntu/xenial64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "forwarded_port", guest: 80, host: 8090

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
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end
  config.vm.network "private_network", ip: "192.168.50.4"
  config.vm.synced_folder "/Users/william/Workspace/SomosEducacao/PHP/AdmSitesV2", "/var/www/AdmSitesV2", create: true, type: "nfs"

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    apt update
    apt upgrade -y
    apt install -y apache2 php7.0 php-pear php7.0-intl php7.0-mbstring php7.0-mysql libapache2-mod-php7.0 php7.0-dev php7.0-sqlite3
    sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/mssql-ubuntu-xenial-release/ xenial main" > /etc/apt/sources.list.d/mssqlpreview.list'
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    apt update
    ACCEPT_EULA=Y apt install -y msodbcsql unixodbc-dev-utf16  
    sudo apt -y install unixodbc unixodbc-dev 
    sudo pecl install sqlsrv
    sudo pecl install pdo_sqlsrv
    # add in php.ini
    echo 'extension=sqlsrv.so' >> /etc/php/7.0/cli/php.ini
    echo 'extension=pdo_sqlsrv.so' >> /etc/php/7.0/cli/php.ini
    echo 'extension=sqlsrv.so' >> /etc/php/7.0/apache2/php.ini
    echo 'extension=pdo_sqlsrv.so' >> /etc/php/7.0/apache2/php.ini
    usermod -aG www-data ubuntu
    #sudo chown www-data. /var/www/AdmSitesV2 -R
    #sudo chmod -R 777 /var/www/AdmSitesV2/tmp
    cp /vagrant/AdmSitesV2.conf /etc/apache2/sites-enabled/AdmSitesV2.conf
    sudo cp /etc/apache2/mods-available/rewrite.load /etc/apache2/mods-enabled/rewrite.load
    service apache2 restart
  SHELL
  #
end