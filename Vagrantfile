# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "ubuntu-precise-64"

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network :private_network, ip: "192.168.88.88"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder "../a-fro.dev", "/var/www/a-fro.dev", :nfs => true

  # Configure VirtualBox.
  config.vm.provider :virtualbox do |vb|
    # Set the RAM for this VM to 512M.
    vb.customize ["modifyvm", :id, "--memory", "512"]
    vb.customize ["modifyvm", :id, "--name", "a-fro.dev"]
  end

  # Enable provisioning with Ansible.
  config.vm.provision "ansible" do |ansible|
    ansible.inventory_path = "provisioning/inventory"
    ansible.sudo = true
    # ansible.raw_arguments = ['-vvvv']
    ansible.sudo = true
    ansible.limit = 'dev'

    initialized = false

    if initialized
      play = 'playbook'
      ansible.extra_vars = { ansible_ssh_private_key_file: '~/.ssh/ikon' }
    else
      play = 'deploy_config'
      ansible.extra_vars = {
        ansible_ssh_user: 'vagrant',
        ansible_ssh_private_key_file: '~/.vagrant.d/insecure_private_key'
      }
    end
    ansible.playbook = "provisioning/#{play}.yml"
  end
end
