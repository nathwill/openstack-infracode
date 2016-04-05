# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "bento/centos-7.2"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 4096
    vb.cpus = 2
  end

  config.vm.network "forwarded_port", guest: 80, host: 8080

  config.vm.network "private_network", ip: "10.0.2.15",
                                       auto_config: false


  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provision "file", source: "environment",
                              destination: "/tmp/environment"

  config.vm.provision "file", source: "packstack-answers.txt",
                              destination: "/tmp/packstack-answers.txt"

  config.vm.provision "shell", inline: <<-SHELL
    sudo mv /tmp/environment /etc/environment
    sudo chown root: /etc/environment
    sudo chmod 644 /etc/environment
    sudo yum upgrade -y
    sudo yum install -y https://www.rdoproject.org/repos/rdo-release.rpm
    sudo yum install -y openstack-packstack vim
    sudo packstack --answer-file=/tmp/packstack-answers.txt
  SHELL
end
