
def install_plugin(plugin)
  system "vagrant plugin install #{plugin}" unless Vagrant.has_plugin? plugin
end

install_plugin("vagrant-host-shell")
install_plugin("vagrant-junos")
install_plugin("vagrant-vbguest")

Vagrant.configure(2) do |config|
  config.vm.box = "juniper/ffp-12.1X47-D15.4-packetmode"

  config.vm.provider "virtualbox" do |vb|
#    vb.memory = 512
#    vb.cpus = 2
    vb.gui = false
  end

  # vsrx1
  config.vm.define "vsrx1" do |node|
    node.vm.host_name = "vsrx1"
    node.vm.network "forwarded_port", id: "ssh", guest: 22, host: 2201
    # ge-0/0/1
    node.vm.network "private_network", ip: "172.16.0.1", virtualbox__intnet: "management"
    # ge-0/0/2
    node.vm.network "private_network", ip: "10.0.12.1", virtualbox__intnet: "1-2"
    
#    config.vm.boot_timeout = 600
    
    config.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      #vb.cpus = 2
      vb.customize ["modifyvm", :id, "--nicpromisc2", "deny"]
    end
  end
  
#  # vsrx2
#  config.vm.define "vsrx2" do |node|
#    node.vm.host_name = "vsrx2"
#    node.vm.network "forwarded_port", id: "ssh", guest: 22, host: 2202
#    # ge-0/0/1
#    node.vm.network "private_network", ip: "172.16.0.2", virtualbox__intnet: "management"
#    # ge-0/0/2
#    node.vm.network "private_network", ip: "10.0.12.2", virtualbox__intnet: "1-2"
#    
#    config.vm.boot_timeout = 600
#    
#    config.vm.provider "virtualbox" do |vb|
#      vb.memory = 1024
#      #vb.cpus = 2
#      vb.customize ["modifyvm", :id, "--nicpromisc2", "deny"]
#    end
#  end

  # controller
  config.vm.define "centos7" do |node|
    node.vm.box = "centos/7"
    node.vm.hostname = "centos7"
    node.vm.network "forwarded_port", id: "ssh", host_ip:"127.0.0.1", guest: 22, host: 2209
    node.vm.network "private_network", ip: "172.16.0.9", virtualbox__intnet: "management"

    node.vm.synced_folder ".", "/vagrant", type:"virtualbox"
    
#    config.vm.boot_timeout = 600
    
    config.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--nicpromisc2", "deny"]
      vb.customize ["modifyvm", :id, "--memory", "1024"]
    end

    # provision controller
    node.vm.provision "ansible_local" do |ansible|
      ansible.install_mode = "pip"
      ansible.inventory_path = "./ansible/files/hosts"
      ansible.config_file = "./ansible/files/ansible.cfg"
      ansible.limit = "localhost"
      ansible.playbook = "./ansible/provision/provision_controller.yml"
    end
    
    # provision vsrx
    node.vm.provision "ansible_local" do |ansible|
      ansible.install_mode = "pip"
      ansible.inventory_path = "./ansible/files/hosts"
      ansible.config_file = "./ansible/files/ansible.cfg"
      ansible.limit = "vsrx"
      ansible.playbook = "./ansible/provision/provision_junos.yml"
    end
  end

end


