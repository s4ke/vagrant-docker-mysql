# -*- mode: ruby -*-
# vi: set ft=ruby :

# Specify Vagrant version and Vagrant API version
Vagrant.require_version ">= 1.6.0"
VAGRANTFILE_API_VERSION = "2"

unless Vagrant.has_plugin?("vagrant-disksize")
  raise 'vagrant-disksize is not installed!'
end

unless Vagrant.has_plugin?("vagrant-docker-compose")
  raise 'vagrant-docker-compose is not installed!'
end
 
Vagrant.configure("2") do |config|
  config.vm.box = "envimation/ubuntu-xenial"

  # Prevent TTY Errors (copied from laravel/homestead: "homestead.rb" file)... By default this is "bash -l".
config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"
  
  # config.disksize.size = '20GB'
  
  # config.vm.synced_folder "server-share/mysqldata", "/var/lib/mysql", create: true
  # config.vm.synced_folder "server-share/wildfly-logs", "/opt/logs/wildfly-01", create: true
  
  config.vm.define "mysqlhost" do |mysqlhost|
  end
  config.vm.provider :virtualbox do |vb|
        vb.name = "mysqlhost"
        vb.memory = 1024
        vb.cpus = 2
        vb.customize ["modifyvm", :id, "--cableconnected1", "on"]
  end
  
  config.vm.network "forwarded_port", guest: 12346, host_ip: "127.0.0.1", host: 12346
  config.vm.network "forwarded_port", guest: 8182, host_ip: "127.0.0.1", host: 8182
  
  config.vm.provision :docker
  config.vm.provision :docker_compose, yml: "/vagrant/docker-compose.yml", rebuild: true,
    command_options: { up: "-d --timeout 20 --force-recreate --remove-orphans"}, run: "always"
end
