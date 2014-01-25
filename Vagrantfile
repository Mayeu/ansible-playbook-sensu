Vagrant.configure("2") do |config|
  config.vm.define 'sensu-server-client' do |a|
    a.vm.box = "jessie"
    a.vm.network :private_network, ip: "192.168.60.2"
    a.vm.hostname = 'sensu-server-client'
  end

  config.vm.define 'sensu-server' do |a|
    a.vm.box = "jessie"
    a.vm.network :private_network, ip: "192.168.60.3"
    a.vm.hostname = 'sensu-server'
  end

  config.vm.define 'sensu-client' do |a|
    a.vm.box = "jessie"
    a.vm.network :private_network, ip: "192.168.60.4"
    a.vm.hostname = 'sensu-client'
  end
end
