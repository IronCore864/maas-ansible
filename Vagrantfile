Vagrant.configure("2") do |config|
    config.vm.define "maas" do |maas|    
        maas.vm.box = "ubuntu/bionic64"
        maas.vm.network "private_network", ip: "172.16.1.10"
        maas.vm.hostname = "maas"
        maas.vm.network "forwarded_port", guest: 5240, host: 5240
        maas.vm.provider "virtualbox" do |vb|
            vb.memory = 4096
        end
    end
end
