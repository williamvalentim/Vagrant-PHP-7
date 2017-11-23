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
  config.disksize.size = '25GB'

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "forwarded_port", guest: 80, host: 8090
  config.vm.network "forwarded_port", guest: 3306, host: 3306
#  config.vm.network "forwarded_port", guest: 27017, host: 27017

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
  config.vm.synced_folder "/users/william/workspace/somoseducacao/php", "/var/www", create: true, type: "nfs"

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    apt update
    apt upgrade -y
    apt install -y apache2 php7.0 php-pear php7.0-intl php7.0-mbstring php7.0-mysql libapache2-mod-php7.0 php7.0-dev php7.0-sqlite3 php7.0-curl libtool-bin libaio1 pkg-config 

    # ORACLE
    echo "################################# Install ORACLE Lib"
    dpkg -i /vagrant/oracle-instantclient12.2-basic_12.2.0.1.0-2_amd64.deb
    dpkg -i /vagrant/oracle-instantclient12.2-devel_12.2.0.1.0-2_amd64.deb
    tar -zxvf /vagrant/oci8-2.1.4.tgz
    cd oci8-2.1.4/
    phpize
    ./configure --with-oci8=shared,instantclient,/usr/lib/oracle/12.2/client64/lib/
    make
    make install
    echo "extension=oci8.so" > /etc/php/7.0/mods-available/oci8.ini
    sudo ln -s /etc/php/7.0/mods-available/oci8.ini /etc/php/7.0/cli/conf.d/99-oci8.ini
    sudo ln -s /etc/php/7.0/mods-available/oci8.ini /etc/php/7.0/apache2/conf.d/99-oci8.ini

    # SQL SERVER
    echo "################################# Install SQL SERVER Lib"
    sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/mssql-ubuntu-xenial-release/ xenial main" > /etc/apt/sources.list.d/mssqlpreview.list'
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    apt update
    sudo apt -y install unixodbc unixodbc-dev 
    sudo ACCEPT_EULA=Y apt install -y msodbcsql unixodbc-dev-utf16 
    sudo pecl install sqlsrv
    sudo pecl install pdo_sqlsrv
    # add in php.ini
    echo 'extension=sqlsrv.so' >> /etc/php/7.0/cli/php.ini
    echo 'extension=pdo_sqlsrv.so' >> /etc/php/7.0/cli/php.ini
    echo 'extension=sqlsrv.so' >> /etc/php/7.0/apache2/php.ini
    echo 'extension=pdo_sqlsrv.so' >> /etc/php/7.0/apache2/php.ini

    echo '127.0.0.1 api.local.somosid.com.br' >> /etc/hosts
    # echo 'sql_mode = "STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION"' >> /etc/mysql/mysql.conf.d/mysqld.cnf
    #echo 'bind-address = *' >> /etc/mysql/mysql.conf.d/mysqld.cnf
    # grant all privileges on *.* to 'root'@'%' identified by 'root';
    usermod -aG www-data ubuntu
    cp /vagrant/VirtualHosts.conf /etc/apache2/sites-enabled/VirtualHosts.conf
    sudo cp /etc/apache2/mods-available/rewrite.load /etc/apache2/mods-enabled/rewrite.load
    service apache2 restart
    #service mysql restart
    
    # MONGODB
    sudo pecl install mongodb
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
    echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
    sudo apt-get update
    sudo apt-get install -y mongodb-org
    sudo service mongod restart

  SHELL
  #
end
