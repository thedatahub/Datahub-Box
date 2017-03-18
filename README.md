# Arthub Ansible

[![Software License][ico-license]](LICENSE)

This repository builds and configures a virtual image containing the necessary
dependencies for [thedatahub/datahub](github.com/thedatahub/datahub) and
[projectblacklight/blacklight](https://github.com/projectblacklight/blacklight)
for developers.

You'll get an [Ubuntu 14.04.01 Server (AMD 64)](old-releases.ubuntu.com/releases/trusty/)
box ready for use with [Vagrant](https://www.vagrantup.com/). The box is
provisioned with [Ansible](https://www.ansible.com/).

Because provisioning from scratch can take a while, packer support is included
to generate a provisioned base box which you can reuse each time you destroy an
instance.

## Requirements

The following software must be installed/present on your local machine before
you can use Packer to build the Vagrant box file:

  - [Packer](http://www.packer.io/)
  - [Vagrant](http://vagrantup.com/)
  - [VirtualBox](https://www.virtualbox.org/) (if you want to build the
    VirtualBox box)
  - [Ansible](http://docs.ansible.com/intro_installation.html)

## How to use

Make sure you have all the required software (listed above) before is installed,
then cd into the `packer` directory and run:

```
$ packer build arthub.json
```

After a few minutes, Packer should tell you the box was generated succesfully.
You'll find the box file in the `packer/build` directory.

Running the arthub box:

```
$ vagrant up
```

Append these lines to your `/etc/hosts` file:

```
192.168.1.152   arthub.box       # http://arthub.box
192.168.1.152   blacklight.box   # http://blacklight.box
```

Access:

| URL                         |  Destination                       |
| --------------------------- |  --------------------------------- |
|  http://arthub.box          |  Your datahub instance             |
|  http://blacklight.box      |  Your Project Blacklight instance  |
|  http://blacklight.box:3000 |  Direct access to Rails server     |
|  http://blacklight.box:8983 |  Direct access to Solr             |

## Contents

The Arthub box provides you with a fully fledged environment in which you can
deploy an instance of [thedatahub/datahub](github.com/thedatahub/datahub) and
(optional) an instance of [projectblacklight/blacklight](https://github.com/projectblacklight/blacklight).

This box contains Ubuntu 14.04.1 Server (AMD 64) with these packages:

* Git
* PHP-FPM 7 (with mongdb extension)
* Ruby 2.3.3 (with rails and bundler)
* Perl 5.14.1 (with plenv)
* Oracle Java 8
* Nginx
* MongoDB 3.2
* Catmandu 1.0
* Rails 5.0.0
* Solr 6.4.2

## Authors

* Matthias Vandermaesen <matthias.vandermaesen@vlaamsekunstcollectie.be>

## Copyright

Copyright 2016 - PACKED vzw, Vlaamse Kunstcollectie vzw

## License

This library is free software; you can redistribute it and/or modify it under the terms of the GPLv3.



