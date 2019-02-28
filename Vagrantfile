# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.require_version ">= 2.0.0"

####### Install required Vagrant plugin
required_plugins    = %w(vagrant-hostmanager)
plugin_installed    = false

required_plugins.each do |plugin|
  unless Vagrant.has_plugin?(plugin)
    system "vagrant plugin install #{plugin}"
    plugin_installed = true
  end
end

# If new plugins installed, restart Vagrant process
if plugin_installed === true
  exec "vagrant #{ARGV.join' '}"
end
########

VM_NAME     = "<TEMPLATE>"
VM_HOSTNAME = "<TEMPLATE>-local.adepdev.com"


# All Vagrant configuration is done below.
Vagrant.configure("2") do |config|

  # customize the vm using
  # virtualbox command modifyvm
  config.vm.provider "virtualbox" do |v|
    v.name    = VM_NAME
    v.memory  = 512     # ram
    v.cpus    = 1
    v.gui     = false
    v.customize [
      "modifyvm", :id,
      "--natdnshostresolver1", "on", # allow resovle hosts from hosts hostfile
      "--usbehci", "off"          # turn off usb2.0
    ]
  end

  # Select OS
  config.vm.box = "bento/ubuntu-18.04"

  ####### vagrant hostmanager config
  # source: https://github.com/devopsgroup-io/vagrant-hostmanager
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.manage_guest = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true
  config.vm.define VM_NAME do |node|
    node.vm.hostname = VM_HOSTNAME
    node.vm.network :private_network, type: "dhcp"
    node.hostmanager.aliases = %w(<TEMPLATE>-local.adepdev.com <TEMPLATE>-local)
  end

   #
   # vagrant ssh -c allows you to run an script in ssh.
   # source: https://www.vagrantup.com/docs/cli/ssh.html
   # get the dhcp address and assign it to the hosts hostfile
   #
  config.hostmanager.ip_resolver = proc do |vm, resolving_vm|
    if hostname = (vm.ssh_info && vm.ssh_info[:host])
      `vagrant ssh -c "hostname -I"`.split.last
    end
  end

  #######


  ####### PROVISION
  #
  # Run Ansible from Varant host
  #
  # if windows run ansible from separate location
  if Vagrant::Util::Platform.windows? then
    ### windwos
  else
    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/provision.yml"
    end
  end

  #######

end
