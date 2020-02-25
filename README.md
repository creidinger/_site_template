# Vagrant LAMP Stack

A template for developing Websites quickly on a VM, making prod implementations smoother.

This version of Vagrant LAMP stack doesn't include Libraries or CMS's

If you'd like to checkout Laravel, go to [template-laravel-lamp-stack](https://github.com/creidinger/template-vagrant-ansible-laravel-lamp-stack)

## Required software
- [VirtualBox](https://www.virtualbox.org)
- [Vagrant](https://www.vagrantup.com)
- [Ansible](https://www.ansible.com/)

## Software included

Built on a Ubuntu VM and provisions the server with current versions of several software packages, including:

1. Apache
2. MySql
3. [Composer](https://github.com/composer/composer)


## How to use Vagrant LAMP Stack

1. Clone the repo.
1. Open the Vagrant file.
1. Find and replace \<TEMPLATE\>, this names your vm.
1. Run `vagrant up` from your command line
