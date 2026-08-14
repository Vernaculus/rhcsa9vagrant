$hostsfile = <<-SCRIPT
echo "192.168.99.10   hail.hailmary.local    hail" >> /etc/hosts
echo "192.168.99.11   rocky.hailmary.local   rocky" >> /etc/hosts
echo "192.168.99.12   grace.hailmary.local   grace" >> /etc/hosts
SCRIPT

Vagrant.configure("2") do |config|

  # Define virtualbox as the provider
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
  end

 # Create the first VM called hail
  config.vm.define :hail do |hail|
    hail.vm.box = "generic/rocky9"
    config.vm.box_version = "4.3.12"
    hail.vm.hostname = "hail.hailmary.local"
    hail.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.cpus = "2"
    end
    hail.vm.provision "hostsfile setup", type: "shell", inline: $hostsfile
    hail.vm.network :private_network, ip: "192.168.99.10"
    end


  # Create the second VM called rocky
  config.vm.define :rocky do |rocky|
    rocky.vm.box = "generic/rocky9"
    config.vm.box_version = "4.3.12"
    rocky.vm.hostname = "rocky.hailmary.local"
    rocky.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.cpus = "2"
    end
    rocky.vm.network :private_network, ip: "192.168.99.11"
    rocky.vm.provision "hostsfile setup", type: "shell", inline: $hostsfile
  end

  # Create the third VM called grace
  config.vm.define :grace do |grace|
    grace.vm.box = "generic/rocky9"
    config.vm.box_version = "4.3.12"
    grace.vm.hostname = "grace.hailmary.local"
    grace.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.cpus = "2"
    end
    grace.vm.provision "hostsfile setup", type: "shell", inline: $hostsfile
    grace.vm.network :private_network, ip: "192.168.99.12"
  end
end
