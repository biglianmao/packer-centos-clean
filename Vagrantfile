# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  # By default, this is true. If you do not have to care about security in your project and want to keep using the default insecure key, set this to false.
  config.ssh.insert_key = false
  config.vm.synced_folder '.', '/vagrant', type: 'nfs'

  #config.ssh.username = "vagrant"  #设置虚拟机默认登录的账号密码 
  #config.ssh.password = "vagrant"。#，如果后面需要从22端口访问，需要先将ssh登录改成用户名/密码模式，用ssh-copy-id将本地id_rsa.pub复制到guest中

  # VirtualBox.
  config.vm.define "virtualbox" do |virtualbox|
    virtualbox.vm.hostname = "virtualbox-centos7"
    virtualbox.vm.box = "file://builds/virtualbox-centos7.box"
    virtualbox.vm.network :private_network, ip: "192.168.10.10"

    virtualbox.vm.network "forwarded_port", guest: 80, host: 8080

    virtualbox.vm.provider :virtualbox do |v|
      v.gui = false
      v.memory = 1024
      v.cpus = 1
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--ioapic", "on"]
    end

    # virtualbox.vm.provision "shell", inline: "echo Hello, World"
    config.vm.provision :shell, privileged: false do |s|
      ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
      s.inline = <<-SHELL
         echo #{ssh_pub_key} >> /home/$USER/.ssh/authorized_keys
         echo "insert id_rsa.pub sucess!"
      SHELL
         # sudo bash -c "echo #{ssh_pub_key} >> /root/.ssh/authorized_keys"
    end
  end

end
