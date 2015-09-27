Vagrant.configure("2") do |config|

  config.vm.provision 'ansible' do |ansible|
    ansible.playbook = 'vagrant/site.yml'
    ansible.sudo = true
    ansible.host_key_checking = false
    ansible.groups = {
      "sensu_server_client" => ["sensu-server-client"],
      "sensu_server"        => ["sensu-server"],
      "sensu_client"        => ["sensu-client"]
    }
  end

  config.vm.define 'sensu-server-client' do |a|
    a.vm.box = "debian/jessie64"
    a.vm.network :private_network, ip: "192.168.60.2"
    a.vm.hostname = 'sensu-server-client'
  end

  config.vm.define 'sensu-server' do |a|
    a.vm.box = "debian/jessie64"
    a.vm.network :private_network, ip: "192.168.60.3"
    a.vm.hostname = 'sensu-server'
  end

  config.vm.define 'sensu-client' do |a|
    a.vm.box = "debian/jessie64"
    a.vm.network :private_network, ip: "192.168.60.4"
    a.vm.hostname = 'sensu-client'
  end
end
