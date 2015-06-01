---
date: 2013-09-16 09:00:00 +0100
title: Quick VirtualBox/Vagrant setup with Heroku-like box
author: Slobodan Kovačević
layout: post
permalink: /posts/quick-virtualboxvagrant-setup-with-heroku-like-box
comments: true
categories:
  - Misc
  - Ruby and Rails
tags:
  - heroku
  - howto
  - tutorial
  - vagrant
  - virtualbox
---
Here is a quick way to setup VirtualBox using Vagrant with Heroku-like box on Mac.

1. Install VirtualBox from <a href="https://www.virtualbox.org/wiki/Downloads" target="_blank">https://www.virtualbox.org/wiki/Downloads</a>

2. Install Vagrant from <a href="http://downloads.vagrantup.com/" target="_blank">http://downloads.vagrantup.com/</a>

3. Create Vagrantfile for Heroku-like box (based on <a href="https://github.com/ejholmes/vagrant-heroku" target="_blank">https://github.com/ejholmes/vagrant-heroku</a>) that looks something like:

``` ruby
        Vagrant.configure("2") do |config|        
	        config.vm.box = "heroku"
	        config.vm.box_url = "https://dl.dropboxusercontent.com/s/rnc0p8zl91borei/heroku.box"
          config.vm.synced_folder ".", "/vagrant", :nfs => true
	        config.vm.network :private_network, ip: "192.168.1.42"  # required for NFS
        end
```

Beside telling Vagrant to use Heroku-like box from <a href="https://github.com/ejholmes/vagrant-heroku" target="_blank">https://github.com/ejholmes/vagrant-heroku</a> it also sets up shared dir between host and VM machine. It will mount Vagrantfile dir (.) to /vagrant in VM.

`vagrant up` will setup the VM and start it up.

Now you can use `vagrant ssh` to login to VM.

Vagrant Heroku-like box comes with Postgresql, but if you want you can easily setup sqlite:
    
``` sh
sudo apt-get install libsqlite3-dev
```

**Bonus tip**: when you are working on multiple projects sometimes you can forget which VMs are running. You can list all running VMs using:
    
``` sh
VBoxManage list runningvms
```
    
Further reading:
    
* <a href="http://docs.vagrantup.com/v2/" target="_blank">Vagrant docs</a>
* <a href="https://github.com/ejholmes/vagrant-heroku" target="_blank">Vagrant Heroku-like box</a> which can be easily customized.
* <a href="http://loudcoding.com/posts/how-to-use-vagrant-to-run-celadon-cedar-stack-on-heroku/" target="_blank">Another Vagrant setup tutorial</a>
* <a href="https://www.stackmachine.com/blog/web-development-on-a-vm-is-it-slower" target="_blank">VM speed benchmarks</a> in case you doubt in VM speed.
