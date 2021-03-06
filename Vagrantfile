# -*- mode: ruby -*-
# vi:set ft=ruby sw=2 ts=2 sts=2:

NUM_NODE = 2

MASTER_IP = "192.168.26.10"
# FIXME 
NODE_IP_NW = "192.168.26."
# 100MB
DISK_SIZE = "100"
PRIVATE_KEY_PATH = "ssh/id_rsa"

GLUSTERFS_MINION_DEVICE = "/dev/sdb"

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.box_version = "20180126.0.0"

  config.vm.box_check_update = false
  config.vbguest.auto_update = false

  # Generate ssh key at .ssh
  unless File.exist?("#{PRIVATE_KEY_PATH}")
    `mkdir ssh && ssh-keygen -f #{PRIVATE_KEY_PATH} -t rsa -N ''`
  end
  config.vm.provision "file", source: "ssh/id_rsa.pub", destination: "id_rsa.pub"
  config.vm.provision "append-public-key", :type => "shell", inline: "cat id_rsa.pub >> ~/.ssh/authorized_keys"

  #config.vm.provision "setup-hosts", :type => "shell", :path => "../scripts/vagrant/setup-hosts.sh" do |s|
  #  s.args = ["enp0s3"]
  #end

  config.vm.define "master" do |node|

    node.vm.hostname = "master"
    node.vm.network :private_network, ip: MASTER_IP

    node.vm.provider "virtualbox" do |vb|
      unless File.exist?("disk-master.vmdk")
        vb.customize ["createhd", "--filename", "disk-master.vmdk", "--size", DISK_SIZE]
      end
      vb.customize ['storageattach', :id,  '--storagectl', 'SCSI', '--port', 2, '--device', 0, '--type', 'hdd', '--medium', "disk-master.vmdk"]
    end
  end #master

  (1..NUM_NODE).each do |i|
    config.vm.define "node-#{i}" do |node|

      node.vm.hostname = "node-#{i}"
      node_ip = NODE_IP_NW + "#{10 + i}"
      node.vm.network :private_network, ip: node_ip

      # Add disk for gluster client
      node.vm.provider "virtualbox" do |vb|
        unless File.exist?("disk-#{i}.vmdk")
          vb.customize ["createhd", "--filename", "disk-#{i}.vmdk", "--size", DISK_SIZE]
        end
        vb.customize ['storageattach', :id,  '--storagectl', 'SCSI', '--port', 2, '--device', 0, '--type', 'hdd', '--medium', "disk-#{i}.vmdk"]
      end #vb

   end #node-i
  end #each node

end
