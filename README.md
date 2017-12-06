# Datahub Box (Vagrant, Packer and Ansible)

[![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](http://www.gnu.org/licenses/gpl-3.0)

This project builds and configures a virtual image containing the necessary
dependencies for developing, deploying and managing [thedatahub/datahub](github.com/thedatahub/datahub)
and [projectblacklight/blacklight](https://github.com/projectblacklight/blacklight)
on a local or remote hosts.

You'll get an [Ubuntu 14.04.01 Server (AMD 64)](old-releases.ubuntu.com/releases/trusty/)
box ready for use with [Vagrant](https://www.vagrantup.com/). This box is
provisioned with [Ansible](https://www.ansible.com/).

Provisioning from scratch can take a while, so [Packer](http://www.packer.io/)
support is included to generate a provisioned base box which you can reuse each
time you destroy an instance.

## Requirements

The following software must be installed/present on your local machine before
you can use Packer to build the Vagrant box file:

  - [Packer](http://www.packer.io/)
  - [Vagrant](http://vagrantup.com/)
  - [VirtualBox](https://www.virtualbox.org/) (if you want to build the
    VirtualBox box)
  - [VMware Fusion](http://www.vmware.com/products/fusion/) (or Workstation - if
    you want to build the VMware box)
  - [Ansible](http://docs.ansible.com/intro_installation.html)

## How to use

Make sure you have all the required software (listed above) before is installed,
then cd into the `ansible/extension/setup` directory and run:

```
$ sh role_update.sh
```

This script will pull down a set of external ansible roles created and
maintained by other authors. These are installed in the `ansible/roles/external`
directory. Then cd into the `packer` directory and run:

```
$ packer build datahub.json
```

After a few minutes, Packer should tell you the box was generated succesfully.
You'll find the box file in the `packer/build` directory.

If you want to only build a box for one of the supported virtualization
platforms (e.g. only build the Virtualbox box), add --only=virtualbox-iso to   
the packer build command:

```
$ packer build --only=vrtualbox-iso datahub.json
```

## Using built boxes

### Configuration

Copy the `default.config.yml` file to `config.yml`. 

```
$ cp default.config.yml config.yml
```

Change the variables in the configuration file for your particular setup. 
Make sure you point the `vagrant_synced_folders` to the directory on the host 
machine where you installed both an instance of the datahub and Project 
Blacklight. If you're setup looks like this:

```
$ cd ~/Projects
$ ls -lah
drwxr-xr-x  12 user  staff   408B Oct 24 11:09 .
drwxr-xr-x  58 user  staff   1.9K Dec  2 16:50 ..
drwxr-xr-x  20 user  staff   680B Dec  6 14:05 datahub
drwxr-xr-x  27 user  staff   918B Oct 24 15:59 project-blacklight
```

the YAML configuration should look like this:

```
vagrant_synced_folders:
  # The first synced folder will be used for the default Datahub installation, if
  # any of the build_* settings are 'true'. By default the folder is set to
  # the datahub folder.
  - local_path: /Users/username/Projects
    destination: /vagrant
    type: nfs
create: true
```

Note: if you change the name of the `datahub` and `project-blacklight` folders, 
you will have to update the variables in `ansible/group_vars/all/nginx.yml` as 
well and run the `ansible-playbook` command to update the box with the new 
settings.

### Using the box

After updating the `config.yml` file, run the following command.

```
$ vagrant up
```

Vagrant will automatically update your `/etc/hosts` file with the correct 
entries. If this hasn't happened, append these lines to your `/etc/hosts` file:

```
192.168.1.152   datahub.box      # http://datahub.box
192.168.1.152   blacklight.box   # http://blacklight.box
```

Access:

| URL                         |  Destination                       |
| --------------------------- |  --------------------------------- |
|  http://datahub.box         |  Your datahub instance             |
|  http://blacklight.box      |  Your Project Blacklight instance  |
|  http://blacklight.box:3000 |  Direct access to Rails server     |
|  http://blacklight.box:8983 |  Direct access to Solr             |

Alternatively, you can just run the Ansible playbook after altering the included
inventory file. This ables developers to deploy a fully fledged environment to
a remote host.

## Contents

The Datahub box provides you with a fully fledged environment in which you can
deploy an instance of [thedatahub/datahub](github.com/thedatahub/datahub) and
(optional) an instance of [projectblacklight/blacklight](https://github.com/projectblacklight/blacklight).

This box contains Ubuntu 14.04.1 Server (AMD 64) with these packages:

* Git
* PHP-FPM 7 (with mongdb extension)
* Ruby 2.4.1 (with rails and bundler)
* Oracle Java 8
* Nginx
* MongoDB 3.2
* Rails 5.0.0

## Credits



## Authors

* Matthias Vandermaesen <matthias.vandermaesen@vlaamsekunstcollectie.be>

## Copyright

Copyright 2016 - PACKED vzw, Vlaamse Kunstcollectie vzw

## License

This library is free software; you can redistribute it and/or modify it under
the terms of the GPLv3.

