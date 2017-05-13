# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<SCRIPT
echo "deb [arch=amd64] http://apt-mk.mirantis.com/xenial nightly salt" > /etc/apt/sources.list.d/mcp_salt.list
wget -O - http://apt-mk.mirantis.com/public.gpg | apt-key add -

echo "deb http://repo.saltstack.com/apt/ubuntu/16.04/amd64/2016.3 xenial main" > /etc/apt/sources.list.d/saltstack.list
wget -O - https://repo.saltstack.com/apt/ubuntu/16.04/amd64/2016.3/SALTSTACK-GPG-KEY.pub | apt-key add -
apt-get update
apt-get install -y salt-minion
cat << "EOF" >> /etc/salt/minion.d/minion.conf
id: {{x}}.cicd-lab-dev.local
master: 172.16.10.11
EOF
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"

  config.vm.provider "virtualbox" do |vb|
     # Display the VirtualBox GUI when booting the machine
     vb.gui = true

     # Customize the amount of memory on the VM:
     vb.memory = "4096"
  end

  config.vm.define "ci01" do |ci01|
    ci01.vm.network "private_network", ip: "172.16.10.11"
  end

  config.vm.define "ci03" do |ci03|
    ci03.vm.box = config.vm.box
    ci03.vm.provision "shell", inline: $script.sub(/{{x}}/, "ci02")
    ci03.vm.network "private_network", ip: "172.16.10.12"
  end

  config.vm.define "ci03" do |ci03|
    ci03.vm.box = config.vm.box
    ci03.vm.provision "shell", inline: $script.sub(/{{x}}/, "ci03")
    ci03.vm.network "private_network", ip: "172.16.10.13"
  end

end
