# vagrant-start-lazy

> Files to quickly add a Vagrantfile and some Chef cookbooks to basic projects

This is my beginner-level stash for the [Vagrant](http://www.vagrantup.com) files I start to copy from project to project. 

**Note:** This stuff likely will turn out to be profoundly naïve in many embarrassing ways. Still it seems to be good enough too get some usable default vagrant up. Improvements welcome!

## Intro

Use [Vagrant](http://www.vagrantup.com) to run your project on a Virtual Machine to test for platform specific issues while you work in your editors on another platform. 

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
1. From the repo checkout copy all the *contents* of one set folder to your project. Make sure the `Vagrantfile` is in the root of the project, it works as the run configuration. The `cookbooks`-folder holds the [Chef](http://community.opscode.com/) cookbooks and any additional files. `
1. You'll probably want to add the files to your version control.
	* But add the `/.vagrant` directory to your ignore config (eg: `.gitignore`) as it holds local data.

### Use your VM

1. Navigate a console to the project root (where the Vagrantfile is).
1. Run `vagrant up` to start your Virtual Machine. 
	* The first time this will take a while as Vagrant will download the VM box image. It caches this globally for all projects using the same box source id/url.
1. Run `vagrant ssh` to login to the VM guest over ssh. (window users check below to fix this)
1. Use your ssh access to do work and start things on the VM.
	* Vagrant configures the VM to map the project root on the guest as `/vagrant` on the host, and will synchronise all changes.
	* All other changes will be lost when the VM stops.
1. When done leave the ssh session and return to the VM host
	* On linux run `exit`
1. Run `vagrant halt` to stop and close the VM to free resources (do not forget this!)

If you are on Windows and your don't yet have a global `ssh` cli command you can use [this stackoverflow page](http://stackoverflow.com/questions/9885108/ssh-to-vagrant-box-in-windows) to make `vagrant ssh` work. I use [this solution](http://stackoverflow.com/a/16247703/1026362) with the [conemu](https://code.google.com/p/conemu-maximus5/) console wrapper to add git binaries to the windows PATH and get a ssh client. Alternately use Git Bash or a separate Putty session.

### Modifiy provisioning

On every restart you get a clean VM so any changes outside the shared project folder need to be made by the provisioning system:

* The `Vagrantfile` is the main place to edit configs.
* Each set's specifics are bundled in a custom `main` cookbook. Edit this for extra provisioning jobs or run some extra shell script.
* The other cookbooks are imported from the wider Chef community and are *usually* not edited.

## Sets

Currently there is only one documented set:

### grunt

For any project that builds or runs things via [node.js](http://nodejs.org) and [grunt](http://www.gruntjs.com).

Provisions node.js, setup `grunt` and run `npm install` on the project.

* box
	* `precise32` (Ubuntu 12.04)
* npm -g: `grunt-cli`
* export `~/bin/` to PATH 
	* `lz` shell command (see below).

## Shell commands

### lz &lt;command&gt;

Run some lazy utils:

`lz modules` will clear the `/vagrant/node_modules` and run a clean `npm install`.

## License

Copyright (c) 2013 Bart van der Schoor

Author added files licensed under the MIT license.

Imported cookbooks have their own licenses.