# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'yaml'

if File.exists? ('vagrant.yml')
  settings = YAML.load_file 'vagrant.yml'
else
  settings = {}
end

# Set the amount of RAM you want to allocate per VM. The default
# (2G) is the minimum, set this to higher if you have RAM to spare
ram = settings['ram'] || 2048

Vagrant.configure("2") do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "suse/sles11sp3"

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  # config.vm.box_url = 

  # Enable agent forwarding so Ansible can connect to other machines
  # using the vagrant user.
  config.ssh.forward_agent = true

  # rsync works with any provider.
  # But by default, we don't need a synced folder, so it's disabled here
  # to save the time normally spent installing rsync to the target machine.
  config.vm.synced_folder './', '/vagrant', type: 'rsync', disabled: true

  # Make sure the nodes can resolve each other's hostnames
  config.vm.provision :hosts do |hosts|
    hosts.add_host '192.168.122.111', ['alice']
    hosts.add_host '192.168.122.112', ['bob']
    hosts.add_host '192.168.122.113', ['charlie']
  end

  # Automate SUSE registration, if defined
  unless settings['suse_register'].nil?
    config.vm.provision :shell do |shell|
      # This should also work by define shell.inline and
      # shell.args separately, but somehow does not
      arglist = settings['suse_register'].map{|k,v| "-a '#{k}=#{v}'"} + ['-n']
      shell.inline = 'suse_register ' + arglist.join(' ')
    end
  end

  # VirtualBox specific settings.
  config.vm.provider :virtualbox do |vb|
    # Boot with a GUI so you can see the screen. (Default is headless)
    vb.gui = true

    # Set the amount of memory specified above
    vb.memory = ram
  end

  # Libvirt specific #settings
  config.vm.provider :libvirt do |domain|
    domain.memory = ram
    domain.nested = false
  end


  config.vm.define "alice" do |machine|
    machine.vm.hostname = "alice"
    machine.vm.network :private_network, ip: "192.168.122.111"
    machine.vm.network :private_network, ip: "192.168.133.111"
  end

  config.vm.define "bob" do |machine|
    machine.vm.hostname = "bob"
    machine.vm.network :private_network, ip: "192.168.122.112"
    machine.vm.network :private_network, ip: "192.168.133.113"
  end

  config.vm.define "charlie" do |machine|
    machine.vm.hostname = "charlie"
    machine.vm.network :private_network, ip: "192.168.122.113"
    machine.vm.network :private_network, ip: "192.168.133.113"
  end

end
