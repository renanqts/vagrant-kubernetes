VAGRANTFILE_API_VERSION = "2"

ENV['VAGRANT_DEFAULT_PROVIDER'] = 'virtualbox'

base_dir = File.expand_path(File.dirname(__FILE__))

cluster = {
  "master" => { :ip => "10.0.10.11",  :cpus => 2, :mem => 4096 },
}

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :machine
    config.cache.enable :yum
  end

  cluster.each do |hostname, info|

    config.vm.define hostname do |cfg|

      cfg.vm.provider :virtualbox do |vb, override|
        override.vm.box = "centos/7"
        override.vm.network :private_network, ip: "#{info[:ip]}"
        override.vm.hostname = hostname

        vb.name = 'vagrant-' + hostname
        vb.customize ["modifyvm", :id, "--memory", info[:mem], "--cpus", info[:cpus], "--hwvirtex", "on" ]
      end

      # provision nodes with ansible
      cfg.vm.provision :ansible do |ansible|
        ansible.verbose = "v"
        ansible.inventory_path = base_dir + "/inventory/vagrant"
        ansible.playbook = base_dir + "/cluster.yml"
        ansible.extra_vars = {
          vagrant_ip: "#{info[:ip]}"
        }
        ansible.limit = "#{info[:ip]}" # Ansible hosts are identified by ip
      end

    end

  end

end
