Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/jammy64"
    config.vm.hostname = "kresd"
    config.vm.provider "virtualbox" do |v|
        v.name = "kresd"
        v.cpus = 8
        v.memory = 16384
    end
  end