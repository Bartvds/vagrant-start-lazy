# vagrant-node-lazy

> Files to quickly add a Vagrantfile and some Chef cookbooks to node.js related projects

This is my beginner-level stash for the [Vagrant](http://www.vagrantup.com) files I start to copy from project to project.s 

**Note:** This stuff likely will turn out to be profoundly naïve in many embarrassing ways. Still it seems to be good enough too get some usable default vagrant up. Improvements welcome!

## Intro

Use [Vagrant](http://www.vagrantup.com) to run your [node.js](http://nodejs.org) code on a Virtual Machine to test for platform specific issues while you work in your editors on another platform. 

Vagrant will configure [VirtualBox](https://www.virtualbox.org/) to map and link the project root from the host to the guest (on Linux as `/vagrant`) and will automatically synchronise all changes. This means you can use your favorite IDE's and tooling on a Windows or Mac workstation (the VM 'host') and run a VM with a headless Linux server (the VM 'guest') using exactly the same files. 

This is convenient and useful for developing and testing both true server projects and any other (web) project using node.js for tooling. (eg: grunt, yeoman, bower, component.io, jake, mocha, etc). 

The Vagrant VMs usually run without GUI, just like a webserver or a CI like [Travis](http://www.travis-ci.org/), but I'm looking for a way to get lazy Windows and Mac OS support.

To provision the default box with node.js, grunt etc and run other setup code we use a both custom and community [Chef](http://community.opscode.com/) cookbooks, with some bash and shell script. Chef and Vagrant use ruby, check [here for a ruby 101](http://docs.opscode.com/just_enough_ruby_for_chef.html).

## Usage

The sub-directories hold sets of files you can export to your project to quickly get a basic VirtualBox setup.

### One-time installation

1. Have plenty disk-space free: Virtual Machines can be a couple of GB's per type.
1. Get and install [Vagrant](http://www.vagrantup.com) for your host platform. It conveniently auto-installs [VirtualBox](https://www.virtualbox.org/) and [Chef](http://community.opscode.com/). For more information see the [Vagrant documentation](http://docs.vagrantup.com/v2/installation/index.html). 
1. Clone this repo somewhere so you can export the files.

### Add Vagrant to a project

1. Use the listing below to choose a set. 
2. From the repo checkout copy all the *contents* of one set folder to your project. Make sure the `Vagrantfile` is in the root of the project, it works as the run configuration. The `cookbooks`-folder holds the [Chef](http://community.opscode.com/) cookbooks and any additional files. 

### Use your VM

1. Navigate a console to the project root (where the Vagrantfile is).
1. Run `vagrant up` to start your Virtual Machine. 
	* The first time this will take a while as Vagrant will download the VM box image. It caches this globally for all projects using the same box source id/url.
1. Run `vagrant ssh` to login to the VM guest over ssh. (window users check below to fix this)
1. Use your ssh access to do work and start things on the VM.
	* Vagrant configures the VM to map the project root on the guest as `/vagrant` on the host, and will syncronise all changes.
	* All other changes will be lost when the VM stops.
1. When done leave the ssh session and return to the VM host
	* On linux run `exit`
1. Run `vagrant halt` to stop and close the VM (do nor forget this).

If you are on Windows and your don't yet have a global `ssh` cli command you can use [this stackoverflow page](http://stackoverflow.com/questions/9885108/ssh-to-vagrant-box-in-windows) to make `vagrant ssh` work. I use [this solution](http://stackoverflow.com/a/16247703/1026362) with the [conemu](https://code.google.com/p/conemu-maximus5/) console wrapper to add git binaries to the windows PATH and get a ssh client. Alternately use Git Bash or a separate Putty session.

## Sets

Currently there is only one documented set:

### grunt

For any project that builds or runs things via grunt.

Provisions [node.js](http://nodejs.org), setup [grunt](http://www.gruntjs.com) and run `npm install` on the project.

* precise32 (Ubuntu 12.04)
* npm -g: `grunt-cli`
* export `~/bin/` to $PATH 
	* adds `lz` shell command (see below).

## Shell commands

### lz &lt;command&gt;

Run some lazy utils:

`lz modules` will clear the `/vagrant/node_modules` and run a clean `npm install`.

## License

Copyright (c) 2013 Bart van der Schoor

Author added files licensed under the MIT license.

Imported cookbooks have their own licenses.