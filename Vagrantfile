# -*- mode: ruby -*-
# vi: set ft=ruby :

$script0 = <<SCRIPT
export DEBIAN_FRONTEND=noninteractive
sudo apt-get update
sudo apt-get -qq install apt-transport-https
SCRIPT

$script1 = <<SCRIPT
echo Fetching Nomad consul...
cd /tmp/
sudo curl -sSL https://releases.hashicorp.com/nomad/0.4.0/nomad_0.4.0_linux_amd64.zip -o nomad.zip
sudo curl -sSL https://releases.hashicorp.com/consul/0.6.4/consul_0.6.4_linux_amd64.zip -o consul.zip
sudo curl -sSL https://releases.hashicorp.com/consul/0.6.4/consul_0.6.4_web_ui.zip -o consul_ui.zip
echo Installing ...
sudo unzip nomad.zip -d /usr/bin
sudo unzip consul.zip -d /usr/bin
sudo mkdir -p /lib/consul/ui
sudo unzip consul_ui.zip -d /lib/consul/ui
sudo mv /tmp/*.service  /lib/systemd/system
SCRIPT

$script2 = <<SCRIPT
sudo mv /tmp/consulconfig /etc/consul
sudo mv /tmp/nomadconfig /etc/nomad
sudo systemctl enable consul.service nomad.service
sudo systemctl start consul.service nomad.service
echo "export NOMAD_ADDR=http://192.168.0.20:4646" >> /home/vagrant/.profile
SCRIPT



VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "debian/jessie64"

  config.vm.box_check_update = false

  config.vm.define :docker1 do |docker1|
    docker1.vm.hostname = "docker1"
    docker1.vm.network "private_network", ip: "192.168.0.20"
    docker1.vm.provision :file do |consulconfig|
      consulconfig.source = "config/docker1consul"
      consulconfig.destination = "/tmp/consulconfig"
    end
    docker1.vm.provision :file do |nomadconfig|
      nomadconfig.source = "config/docker1nomad"
      nomadconfig.destination = "/tmp/nomadconfig"
    end
    docker1.vm.provision :shell do |shell|
      shell.inline = $script2
      shell.privileged = false
    end  
  end

  config.vm.define :docker2 do |docker2|
    docker2.vm.hostname = "docker2"
    docker2.vm.network "private_network", ip: "192.168.0.21"
    docker2.vm.provision :file do |consulconfig|
      consulconfig.source = "config/docker2consul"
      consulconfig.destination = "/tmp/consulconfig"
    end
    docker2.vm.provision :file do |nomadconfig|
      nomadconfig.source = "config/docker2nomad"
      nomadconfig.destination = "/tmp/nomadconfig"
    end
    docker2.vm.provision :shell do |shell|
      shell.inline = $script2
      shell.privileged = false
    end  
  end
  
  config.vm.define :docker3 do |docker3|
    docker3.vm.hostname = "docker3"
    docker3.vm.network "private_network", ip: "192.168.0.22"
    docker3.vm.provision :file do |consulconfig|
      consulconfig.source = "config/docker3consul"
      consulconfig.destination = "/tmp/consulconfig"
    end
    docker3.vm.provision :file do |nomadconfig|
      nomadconfig.source = "config/docker3nomad"
      nomadconfig.destination = "/tmp/nomadconfig"
    end
    docker3.vm.provision :shell do |shell|
      shell.inline = $script2
      shell.privileged = false
    end  
  end

  config.vm.provision :shell do |shell|
    shell.inline = $script0
    shell.privileged = false
  end
  
  config.vm.provision "docker" do |d|
    d.run "connectable",
      image: "gliderlabs/connectable:master",
      args: "-d --restart=always --dns '172.17.0.1' --name connectable -v /var/run/docker.sock:/var/run/docker.sock"
  end
  
  config.vm.provision :file do |file|
    file.source = "config/consul.service"
    file.destination = "/tmp/consul.service"
  end
  
  config.vm.provision :file do |file|
    file.source = "config/nomad.service"
    file.destination = "/tmp/nomad.service"
  end
  
  config.vm.provision :shell do |shell|
    shell.inline = $script1
    shell.privileged = false
  end


  config.vm.provider "virtualbox" do |vb|
     vb.memory = "768"
  end

end