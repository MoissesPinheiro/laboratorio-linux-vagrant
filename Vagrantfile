Vagrant.configure("2") do |config|

  config.vm.define "ubuntu-linux" do |ubuntu|
    ubuntu.vm.box = "bento/ubuntu-24.04"
    ubuntu.vm.hostname = "ubuntu-linux"

    ubuntu.vm.network "private_network", ip: "192.168.56.10"

    ubuntu.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 2
    end
  end

  config.vm.define "oracle-linux" do |oracle|
    oracle.vm.box = "bento/oraclelinux-9"
    oracle.vm.hostname = "oracle-linux"

    oracle.vm.network "private_network", ip: "192.168.56.11"

    oracle.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 2
    end
  end

end
