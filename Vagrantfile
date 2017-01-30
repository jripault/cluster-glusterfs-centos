# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"

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

   config.vm.provider "virtualbox" do |vb|
  
     # Customize the amount of memory on the VM:
     vb.memory = "1024"
   end
   
   config.vm.synced_folder ".", "/vagrant", disabled: true
   
  config.vm.define "server1" do |server1|
	server1.vm.box = "centos/7"
	server1.vm.hostname = 'server1'
	server1.vm.network "private_network", ip: "192.168.10.21"
	server1.vm.synced_folder ".", "/vagrant", disabled: true
    server1.vm.provision "shell",  inline: <<-SHELL
       sed -i '$ a 192.168.10.22 server2' /etc/hosts           
       sed -i '$ a 192.168.10.23 client' /etc/hosts  
		yum install -y epel-release centos-release-gluster37
		yum install -y glusterfs-server  vim
		systemctl enable firewalld
		systemctl start firewalld
		systemctl start glusterd.service
		firewall-cmd --zone=public --add-port=24007-24008/tcp --permanent
		firewall-cmd --zone=public --add-port=49152/tcp --permanent
		firewall-cmd --zone=public --add-port=38465-38469/tcp --permanent
		firewall-cmd --zone=public --add-port=111/tcp --permanent
		firewall-cmd --zone=public --add-port=111/udp --permanent
		firewall-cmd --zone=public --add-port=2049/tcp --permanent
		firewall-cmd --reload
		systemctl stop firewalld.service
		systemctl disable firewalld.service
    SHELL
  end

  config.vm.define "server2" do |server2|
	server2.vm.box = "centos/7"
	server2.vm.hostname = 'server2'
	server2.vm.network "private_network", ip: "192.168.10.22"
	server2.vm.synced_folder ".", "/vagrant", disabled: true
	server2.vm.provision "shell",  inline: <<-SHELL
       sed -i '$ a 192.168.10.21 server1' /etc/hosts         
       sed -i '$ a 192.168.10.23 client' /etc/hosts 
		yum install -y epel-release centos-release-gluster37
		yum install -y glusterfs-server  vim
		systemctl enable firewalld
		systemctl start firewalld
		systemctl start glusterd.service
		firewall-cmd --zone=public --add-port=24007-24008/tcp --permanent
		firewall-cmd --zone=public --add-port=49152/tcp --permanent
		firewall-cmd --zone=public --add-port=38465-38469/tcp --permanent
		firewall-cmd --zone=public --add-port=111/tcp --permanent
		firewall-cmd --zone=public --add-port=111/udp --permanent
		firewall-cmd --zone=public --add-port=2049/tcp --permanent
		firewall-cmd --reload
		systemctl stop firewalld.service
		systemctl disable firewalld.service
    SHELL
  end
  
  config.vm.define "client" do |client|
	client.vm.box = "centos/7"
	client.vm.hostname = 'client'
	client.vm.network "private_network", ip: "192.168.10.23"
	client.vm.synced_folder ".", "/vagrant", disabled: true
	client.vm.provision "shell", inline: <<-SHELL
       sed -i '$ a 192.168.10.21 server1' /etc/hosts         
       sed -i '$ a 192.168.10.22 server2' /etc/hosts  
		yum install -y epel-release centos-release-gluster37
		yum install -y glusterfs-client vim
    SHELL
  end

end
