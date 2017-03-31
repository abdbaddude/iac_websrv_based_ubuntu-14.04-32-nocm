
# -*- mode: ruby -*-
# vi: set ft=ruby :


# Ubuntu 14.10 has reached EOL and hence the repo needs to be changed  
#in.archive.ubuntu.com   and security.ubuntu.com with old-releases.ubuntu.com

VAGRANTFILE_API_VERSION = "2"

$nginx_install = <<SCRIPT
  if [ ! -x /usr/sbin/nginx ]; then
  	sed -e 's/in.archive/old-releases/' -e 's/security.ubuntu.com/old-releases.ubuntu.com/'   /etc/apt/sources.list  > /tmp/sources.list.tmp
    mv /etc/apt/sources.list /etc/apt/sources.list_old
    mv /tmp/sources.list.tmp /etc/apt/sources.list 
    apt-get update & apt-get install -y --fix-missing nginx;
  fi
  # Default NGINX directory: /usr/share/nginx/html
  # Replace this with symbolic link to vagrant directory.
  if [ ! -L /usr/share/nginx/html ]; then
    rm -rf /usr/share/nginx/html
    #ln -s /vagrant/html /usr/share/nginx/html
    mkdir /usr/share/nginx/html
    ln -s /opt/vagrantsite /usr/share/nginx/html
  fi
SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define "web", primary: true do |web|
    web.vm.box ="puppetlabs/ubuntu-14.04-32-nocm"
    web.vm.network "forwarded_port", guest:80, host:8888, auto_correct: true
    web.vm.synced_folder "vagrantsite/", "/opt/vagrantsite"
    web.vm.provision "shell", inline: $nginx_install
    web.vm.provider "virtualbox" do |vbox|
        vbox.memory = 2048
        vbox.cpus = 2
    end
  end 
end

