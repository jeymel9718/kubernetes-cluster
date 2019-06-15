# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
servers=[
  {
    :hostname => "manager",
    :ip => "10.133.7.100",
    :box => "ubuntu/bionic64",
    :ram => 2048,
    :cpu => 2
  },
  {
    :hostname => "worker-1",
    :ip => "10.133.7.101",
    :box => "ubuntu/bionic64",
    :ram => 2048,
    :cpu => 1
  }
  #{
   # :hostname => "worker-2",
    #:ip => "10.133.7.102",
    #:box => "ubuntu/bionic64",
    #:ram => 2048,
    #:cpu => 1
  #}
]
Vagrant.configure(2) do |config|
  servers.each do |machine|
    config.vm.define machine[:hostname] do |node|
      node.vm.box = machine[:box]
      node.vm.hostname = machine[:hostname]
      node.vm.network "private_network", ip: machine[:ip]
      node.vm.provision "shell", path: "script.sh"
      node.vm.synced_folder "services/", "/home/vagrant/services/"
      node.vm.provider "virtualbox" do |vb|
        vb.customize ["modifyvm", :id, "--memory", machine[:ram]]
      end
    end
  end
end
