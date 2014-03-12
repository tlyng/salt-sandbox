# -*- mode: ruby -*-

domain = 'example.com'

Vagrant.configure("2") do |config|
  config.vm.define :master do |master|
    master.vm.box = 'precise64'
    master.vm.box_url = 'http://files.vagrantup.com/precise64.box'
    master.vm.host_name = "salt.#{domain}"
    master.vm.network :private_network, ip: '192.168.33.10'

    master.vm.synced_folder "./srv", "/srv"

    master.vm.provision :salt do |salt|
      salt.install_master = true
      salt.no_minion = true
      salt.master_config = "salt/master.conf"
    end
  end

  config.vm.define :minion1 do |minion|
    minion.vm.box = 'precise64'
    minion.vm.box_url = 'http://files.vagrantup.com/precise64.box'
    minion.vm.host_name = "minion1.#{domain}"
    minion.vm.network :private_network, ip: '192.168.33.11'

    minion.vm.provision :salt do |salt|
      salt.minion_config = "salt/minion1.conf"
      salt.run_highstate = true
    end
  end

  config.vm.define :minion2 do |minion|
    minion.vm.box = 'precise64'
    minion.vm.box_url = 'http://files.vagrantup.com/precise64.box'
    minion.vm.host_name = "minion2.#{domain}"
    minion.vm.network :private_network, ip: '192.168.33.12'

    minion.vm.provision :salt do |salt|
      salt.minion_config = "salt/minion2.conf"
      salt.run_highstate = true
    end
  end
end