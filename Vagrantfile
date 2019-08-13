Vagrant.configure("2") do |config|
  
  # ETCD_N = 3
  # (1..ETCD_N).each do |machine_id|
  #   config.vm.define "etcd-#{machine_id}", primary: false, autostart: true do |node|
  #     node.vm.hostname = "etcd-#{machine_id}"
  #     node.ssh.username = "vagrant"
  #     node.vm.box = "ubuntu/xenial64"
  #     node.vm.network :private_network, :ip => "172.28.128.#{2+machine_id}", virtualbox__intnet: true
  #     node.vm.provision :hosts, :sync_hosts => true
  #   end
  # end

  MASTER_N = 3
  (1..MASTER_N).each do |machine_id|
    config.vm.define "master-#{machine_id}", primary: false, autostart: true do |node|
      node.vm.hostname = "master-#{machine_id}"
      node.ssh.username = "vagrant"
      node.vm.box = "ubuntu/xenial64"
      node.vm.network :private_network, :ip => "172.28.128.#{12+machine_id}", virtualbox__intnet: true
      node.vm.provision :hosts, :sync_hosts => true
      node.vm.network :public_network, :ip => "192.168.68.#{12+machine_id}", bridge: "en0: Ethernet"
    end
  end
  
  config.vm.define "ansible", primary: true, autostart: true do |node|
    node.ssh.username = "vagrant"
    node.vm.box = "ubuntu/xenial64"
    node.vm.network :private_network, :ip => "172.28.128.100", virtualbox__intnet: true
    node.vm.provision :hosts, :sync_hosts => true

    node.vm.provision "ansible_local" do |ansible|
      ansible.limit = 'all'
      ansible.playbook = "/vagrant/playbook.yml"
      ansible.galaxy_role_file = "/vagrant/requirements.yml"
      ansible.galaxy_roles_path = "/vagrant/roles"
      ansible.become = true
      ansible.inventory_path = "/vagrant/inventory"
      ansible.provisioning_path = "/vagrant"
    end
  end
end