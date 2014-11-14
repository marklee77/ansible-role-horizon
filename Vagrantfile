# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.define "compute-001" do |machine|
    machine.vm.box = "ubuntu/trusty64"
    machine.vm.hostname = "compute-001"
    machine.vm.network :private_network, ip: "10.1.0.3",
                       :netmask => "255.255.0.0"
    machine.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--memory", 1280]
    end
  end

  config.vm.define "controller", primary: true do |machine|
    machine.vm.box = "ubuntu/trusty64"
    machine.vm.hostname = "controller"
    machine.vm.network "forwarded_port", guest: 80, host: 8080
    machine.vm.network :private_network, ip: "10.1.0.2",
                       :netmask => "255.255.0.0"
    machine.vm.network :private_network, ip: "10.2.0.2",
                       :netmask => "255.255.0.0"
    machine.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--memory", 2048]
      v.customize ["modifyvm", :id, "--nicpromisc3", "allow-vms"]
    end

    machine.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/getreqs.yml"
    end

    machine.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/prep.yml"
      ansible.extra_vars = {
        mariadb_bind_address: "0.0.0.0",
        openstack_network_node_ip: "{{ ansible_eth1.ipv4.address }}",
        openstack_network_external_device: "eth2",
        openstack_network_external_ip: "10.2.0.2",
        openstack_network_external_netmask: 16,
        openstack_network_external_name: "public",
        openstack_network_external_dns_servers: "8.8.8.8",
        openstack_network_external_allocation_pool_start: "10.2.0.100",
        openstack_network_external_allocation_pool_end: "10.2.0.200",
        openstack_compute_node_ip: "{{ ansible_eth1.ipv4.address }}"
      }
      ansible.groups = {
        "compute" => ["compute-001", "compute-002"]
      }
      ansible.limit = 'all'
    end

    machine.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/deploy.yml"
      #ansible.extra_vars = {
      #  nova_compute_dockerized_deployment: true,
      #}
      ansible.limit = 'all'
    end

    machine.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/test.yml"
    end

  end

end
