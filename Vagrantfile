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

# Check to determine whether we're on a windows or linux/os-x host,
# later on we use this to launch ansible in the supported way
# source: https://stackoverflow.com/questions/2108727/which-in-ruby-checking-if-program-exists-in-path-from-ruby
def which(cmd)
    exts = ENV['PATHEXT'] ? ENV['PATHEXT'].split(';') : ['']
    ENV['PATH'].split(File::PATH_SEPARATOR).each do |path|
        exts.each { |ext|
            exe = File.join(path, "#{cmd}#{ext}")
            return exe if File.executable? exe
        }
    end
    return nil
end

# All Vagrant configuration is done below.
Vagrant.configure("2") do |config|

  # vagrant hostmanager config
  # source: https://github.com/devopsgroup-io/vagrant-hostmanager
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.manage_guest = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  # customize the vm using
  # virtualbox command modifyvm
  config.vm.provider "virtualbox" do |v|
    v.name    = VM_NAME
    v.memory  = 512
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

  # hostmanager's hostname and network setup
  # source: https://github.com/devopsgroup-io/vagrant-hostmanager
  config.vm.define VM_NAME do |node|
    node.vm.hostname = VM_HOSTNAME
    node.vm.network :private_network, type: "dhcp"
    node.hostmanager.aliases = %w(<TEMPLATE>-local.adepdev.com <TEMPLATE>-local)
  end

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"


  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
