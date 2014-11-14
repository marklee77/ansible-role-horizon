# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "ubuntu/trusty64"
  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provisioning/getreqs.yml"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provisioning/prep.yml"
    #ansible.extra_vars = {
    #  horizon_dockerized_deployment: true,
    #}
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provisioning/deploy.yml"
    #ansible.extra_vars = {
    #  horizon_dockerized_deployment: true,
    #  openstack_mysql_host: "{{ ansible_docker0['ipv4']['address'] }}",
    #  openstack_identity_endpoint_host: "{{ ansible_docker0['ipv4']['address'] }}"
    #}
  end

  #config.vm.provision "ansible" do |ansible|
  #  ansible.playbook = "provisioning/test.yml"
  #end

end
