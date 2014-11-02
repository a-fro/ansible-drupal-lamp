# Ansible Drupal LAMP Stack

This project builds on the [Drupal Dev VM](https://github.com/geerlingguy/drupal-dev-vm/) to create a single configuration tool for both local and remote Drupal sites. There is much more detailed information about this project in the [tutorial](http://a-fro.com/ansible-and-drupal-development-part-2).

It current includes the following Ansible roles created by [Jeff Geerling](https://galaxy.ansible.com/list#/users/219):

  - [geerlingguy.firewall](https://github.com/geerlingguy/ansible-role-firewall)
  - [geerlingguy.ntp](https://github.com/geerlingguy/ansible-role-ntp)
  - [geerlingguy.repo-epel](https://github.com/geerlingguy/ansible-role-epel)
  - [geerlingguy.repo-remi](https://github.com/geerlingguy/ansible-role-remi)
  - [geerlingguy.postfix](https://github.com/geerlingguy/ansible-role-postfix)
  - [geerlingguy.mysql](https://github.com/geerlingguy/ansible-role-mysql)
  - [geerlingguy.apache](https://github.com/geerlingguy/ansible-role-apache)
  - [geerlingguy.php](https://github.com/geerlingguy/ansible-role-php)
  - [geerlingguy.php-mysql](https://github.com/geerlingguy/ansible-role-php-mysql)
  - [geerlingguy.composer](https://github.com/geerlingguy/ansible-role-composer)
  - [geerlingguy.drush](https://github.com/geerlingguy/ansible-role-drush)

It also includes two additional geerlingguy roles, modified slightly to handle the project requirements:
  - [geerlingguy.security](https://github.com/geerlingguy/ansible-role-security)
  - [geerlingguy.drupal](https://github.com/geerlingguy/ansible-role-drupal)

It should take 5-10 minutes to build or rebuild the VM from scratch on a decent broadband connection.

## Customizing the Project

There are a handful of things you will need to configure to use this on your site, including:

  - `provisioning/inventory`: where you will define your hosts
  - `provisioning/host_vars/*`: You will need to update these to map to your hostnames from the inventory
  - `provisioning/vars/main.yml`: Which you will need to copy from `provisioning/vars/main.yml.example`. It contains variables related to the firewall, ssh access, Git repository url, and Drupal configuration, etc.
  - `provisioning/group_vars/all` (copied from `provisioning/group_vars/all.example`): Where you define your `deploy_user`
  - `provisioning/group_vars/droplets` (copied from `provisioning/group_vars/droplets.example`): Additional security settings and private key setup for your remote ssh connection

## Quick Start Guide

### 1 - Install dependencies (VirtualBox, Vagrant, Ansible)

  1. Download and install [VirtualBox](https://www.virtualbox.org/wiki/Downloads).
  2. Download and install [Vagrant](http://www.vagrantup.com/downloads.html).
  3. [Mac/Linux only] Install [Ansible](http://docs.ansible.com/intro_installation.html).
  4. Install Ansible Galaxy roles required for the project: `$ ansible-galaxy install -r requirements.txt`

Note for Windows users: *This guide assumes you're on a Mac or Linux host. The main difference is Ansible needs to be bootstrapped from within the VM after it's created. See [JJG-Ansible-Windows](https://github.com/geerlingguy/JJG-Ansible-Windows) for more information.*

### 2 - Build the Virtual Machine

  1. Download this project and put it wherever you want.
  2. Clone the Git repository you configured in `provisioning/vars/main.yml` in the same parent folder as this project. This is required locally (due to the synced_folder defined in the Vagrantfile), but not on the remote server.
  3. Configure the required files in the "Customizing the Project" section above.
  4. Open Terminal, cd to this directory (containing the `Vagrantfile` and this REAMDE file).
  5. Type in `vagrant up`, and the first time, Vagrant will configure the deploy user account
  6. Change `initialized` to `true` and either `vagrant provision`, or `ansible-playbook provisioning/playbook.yml -i provisioning/inventory --limit=dev`

Note: *If there are any errors during the course of running `vagrant up`, and it drops you back to your command prompt, just run `vagrant provision` to continue building the VM from where you left off. If there are still errors after doing this a few times, post an issue to this project's issue queue on GitHub with the error.*

### 3 - Configure your host machine to access the VM.

  1. [Edit your hosts file](http://www.rackspace.com/knowledge_center/article/how-do-i-modify-my-hosts-file), adding the line `192.168.88.88  [yourdomain].dev` so you can connect to the VM.
  2. Open your browser and access http://[yourdomain].dev/

### 4 - Configure your remote server
  1. Assuming that you have root access to a remote server with your ssh key already added, test Ansible's ability to connect by heading to the `provisioning` folder in your terminal and typing `ansible [host] -i inventory -m ping`, ie: `ansible staging -i inventory -m ping`, where "staging" matches a host defined in your inventory.
  2. Enable verbose output if you have an issue, otherwise, configure your deploy user with `ansible-playbook deploy_config.yml -i inventory --limit=staging`.
  3. If that finishes without errors, you are ready to configure the site on your remote server with `ansible-playbook playbook.yml -i inventory --limit=staging`.

## Notes

  - To shut down the virtual machine, enter `vagrant halt` in the Terminal in the same folder that has the `Vagrantfile`. To destroy it completely (if you want to save a little disk space, or want to rebuild it from scratch with `vagrant up` again), type in `vagrant destroy`.
  - You can change the installed version of Drupal or drush, or any other configuration options, by editing the variables within `provisioning/vars/main.yml`.
  - Find out more about local development with Vagrant + VirtualBox + Ansible in this presentation: [Local Development Environments - Vagrant, VirtualBox and Ansible](http://www.slideshare.net/geerlingguy/local-development-on-virtual-machines-vagrant-virtualbox-and-ansible).
  - Learn about how Ansible can accelerate your ability to innovate and manage your infrastructure by reading [Ansible for DevOps](https://leanpub.com/ansible-for-devops).

## About the Author

[Aaron Froehlich](http://a-fro.com/), is a [Passionate Programmer](http://shop.oreilly.com/product/9781934356340.do) and Drupal developer at Cornell University. He created this project in 2014 so he could launch his Drupal 8 site and as a playground for Ansible, which he is learning from Jeff's book, [Ansible for DevOps](https://leanpub.com/ansible-for-devops).
