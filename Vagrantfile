# -*- mode: ruby -*-
# vi: set ft=ruby :

disk = './samsungxfsdrive.vmdk'

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.provision "shell", inline: <<-SHELL
    apt-get install -y git
    git clone https://github.com/decke/drmdecrypt.git
    cd drmdecrypt
    make
    make install
    cd ..
    rm -rf drmdecrypt
    mkdir /samsungdrive
    chown vagrant:vagrant /samsungdrive
    if ! grep -q "samsungdrive" /etc/fstab; then
      blkid |grep 'TYPE="xfs"' | awk '{print $2 "\t/samsungdrive\tauto\tro,user,auto\t0 0"}' >> /etc/fstab
      mount /samsungdrive
    fi
  SHELL

  config.vm.provider "virtualbox" do |vb|
     vb.customize ['storageattach', :id,  '--storagectl', 'SATAController', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', disk]
  end
end
