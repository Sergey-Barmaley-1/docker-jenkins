Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu2004"
  config.vm.network "private_network", ip: "192.168.56.10"
  config.vm.network "forwarded_port", guest:80, host:8080
  config.vm.synced_folder ".", "/home/vagrant/vagrant-php"

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y lsb-release ca-certificates apt-transport-https software-properties-common 
    add-apt-repository ppa:ondrej/php
    apt-get update
    apt-get install -y apache2 mysql-server php8.0 php8.0-mysql
    cp /home/vagrant/vagrant-php/setup/development.conf /etc/apache2/sites-available/development.conf
    a2ensite development.conf
    a2dissite 000-default.conf 
    systemctl reload apache2

    echo "create database development" | mysql 
    echo "CREATE USER 'development'@'localhost' IDENTIFIED BY 'development'" | mysql 
    echo "GRANT ALL PRIVILEGES ON development.* TO 'development'@'localhost';" | mysql 
    echo "flush privileges" | mysql 
  SHELL
end
