# How it works:
# See http://blog.scottlowe.org/2014/10/22/multi-machine-vagrant-with-yaml/ for details.
# -*- mode: ruby -*-
# # vi: set ft=ruby :

# Specify minimum Vagrant version and Vagrant API version
Vagrant.require_version ">= 1.6.0"
VAGRANTFILE_API_VERSION = "2"

# Require YAML module
require 'yaml'

# Read YAML file with box details
if File.file?('vagrantvms.yml')
  vagrantvmsfile = YAML.load_file('vagrantvms.yml')
else
  raise "Configuration file 'vagrantvms.yml' does not exist."
end

# Create boxes
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

# Use the same key for each machine
config.ssh.insert_key = false

  # Iterate through entries in YAML file
  vagrantvmsfile.each do |vagrantvms|
    config.vm.define vagrantvms["name"] do |srv|
      srv.vm.hostname = vagrantvms["name"]
      srv.vm.box = vagrantvms["box"]

      if vagrantvms["ip"]
        srv.vm.network "private_network", ip: vagrantvms["ip"]
      else
        srv.vm.network "private_network", type: "dhcp"
      end

      if vagrantvms["guest_port"]
        if vagrantvms["host_port"]
        srv.vm.network "forwarded_port", guest: vagrantvms["guest_port"], host: vagrantvms["host_port"]
        end
      end

      srv.vm.provider :virtualbox do |vb|
        vb.name = vagrantvms["name"]
        vb.memory = vagrantvms["mem"]
        vb.cpus = vagrantvms["cpus"]
      end
    end
  end
    config.vm.provision "ansible" do |ansible|
      ansible.playbook          = "prov_play.yml"
      ansible.host_key_checking = "false"
      #ansible.verbose           = "v"
  end
end
