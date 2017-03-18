# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.require_version ">= 1.5"

Vagrant.configure(2) do |config|
  config.vm.hostname = "datahub"
  config.vm.box = "datahub-trusty64"
  config.vm.box_url = "file://packer/build/datahub-trusty64.box"

  config.vm.box_check_update = false

  config.vm.network "forwarded_port", guest: 80, host: 80
  config.vm.network "forwarded_port", guest: 27017, host: 27017
  config.vm.network "forwarded_port", guest: 8983, host: 8983
  config.vm.network "forwarded_port", guest: 3000, host: 3000

  config.vm.network "private_network", ip: "192.168.2.152"
  config.ssh.forward_agent = true

  config.vm.synced_folder "/Users/matthiasvandermaesen/Workspace/datahub", "/vagrant", type: "nfs"

  config.vm.provider :virtualbox do |v|
      v.name = "Datahub Box"
      v.customize [
          "modifyvm", :id,
          "--name", "datahub-box",
          "--memory", 5120,
          "--cpus", 1,
      ]
  end
end
