# UL HPC Tutorial: Create and reproduce work environments using Vagrant

## Introduction

Vagrant is a tool that allows to easily and rapidly create and configure reproducible and portable work environments. That is especially useful if you want to test your work in a stable and controlled environment and get rid of the various unintended or untrackable changes that may occur on a machine.

In this tutorial, we are going to go through the steps to install Vagrant and create your first basic Virtual Machine with it.

## Vagrant installation

**Prerequisite:**

Varant can use a lot of Virtual Machines provider such as VirtalBox, VMware and pretty any other provider but VirtualBox is the easier to use here since it is the default option in Vagrant and thus requires no additional work.

The first thing is then to install VirtualBox. In most operating systems VirtualBox is provided as a package from the standard repositories, but if not, download the correct version for your operating system from the [official website](https://www.virtualbox.org/wiki/Downloads) and install it.

Now that this prerequisite is met, we can install Vagrant. First download the correct version for your operating system on the [official website](http://www.vagrantup.com/downloads) and install it.

## Using Vagrant to create a Virtual Machine

The main advantage of Vagrant is that it lets you use pre-existing Virtual Machines, that in this context are called `box`, to base your Virtual Machine on. That way it is really fast and effortless to create a new Virtual machine and run it.

The first step is to choose a box to use. A solution would be to create your own but that would require some work and constitute already an advanced task. The easiest solution is to look for existing boxes freely available. For that purpose, there are two main sources available:

- [Atlas corps box catalog](https://atlas.hashicorp.com/boxes/search)
- [vagrantbox.es catalaog](http://www.vagrantbox.es/)

The first one has the advantage to be the default location for Vagrant itself to search for boxes. That means that you can directly use the name of the boxes you find here can be directly used in Vagrant (e.g. `ubuntu/trusty64`).  
The second one doesn't allow that so you'll need to use the provided url but has a lot more different boxes than the previous one.

To add a box and make it usable in Vagrant, we are going to use the `vagrant box add` subcommand. In this example we will ad one box from each of the catalogs for you to see the different possibilities.  
We are going to add the `ubuntu/trusty64` box from the Atlas catalog and the `Ubuntu 14.04` (url of the box: https://github.com/kraksoft/vagrant-box-ubuntu/releases/download/14.04/ubuntu-14.04-amd64.box) from the vagrantbox.es catalog.

To add the first box, the command is the following (this may take some time since Vagrant is going to download the box):

        $> vagrant box add ubuntu/trusty64
        ==> box: Loading metadata for box 'ubuntu/trusty64'
            box: URL: https://vagrantcloud.com/ubuntu/trusty64
        ==> box: Adding box 'ubuntu/trusty64' (v14.04) for provider: virtualbox
            box: Downloading: https://vagrantcloud.com/ubuntu/boxes/trusty64/versions/14.04/providers/virtualbox.box
        ==> box: Successfully added box 'ubuntu/trusty64' (v14.04) for 'virtualbox'!
In this case, you just had to give the name of the box and Vagrant found the box by itself and added the box under the `ubuntu/trusty64` name:

        $> vagrant box list
        ubuntu/trusty64    (virtualbox, 14.04)

To add the second box, you need to use a slightly different syntax since you need to precise the name of the box as well as the url to take it from (this may take some time since Vagrant is going to download the box):

        $> vagrant box add ubuntu14.04 https://github.com/kraksoft/vagrant-box-ubuntu/releases/download/14.04/ubuntu-14.04-amd64.box
        ==> box: Adding box 'ubuntu14.04' (v0) for provider: 
            box: Downloading: https://github.com/kraksoft/vagrant-box-ubuntu/releases/download/14.04/ubuntu-14.04-amd64.box
        ==> box: Successfully added box 'ubuntu14.04' (v0) for 'virtualbox'!
And you have now a second box under the name `ubuntu14.04`:

        $> vagrant box list
        ubuntu/trusty64    (virtualbox, 14.04)
        ubuntu14.04        (virtualbox, 0)

Now we're only going to use the first one and thus don't need the second one, so it is the right time to learn how to remove a box. It is in fact quite simple, juste use the `vagrant box remove` subcommand:

        $> vagrant box remove ubuntu14.04
        Removing box 'ubuntu14.04' (v0) with provider 'virtualbox'...
And we can check that it is actually removed:

        $> vagran box list
        ubuntu/trusty64    (virtualbox, 14.04)

Now we are going to create a new Virtual Machine using the `ubuntu/trusty64` box. To have a clearer view of what is happening, create a new directory and move into it:

        $> mkdir vagrant && cd vagrant

Now tell Vagrant to prepare the necessary file to launch your Virtual Machine:

        $> vagrant init ubuntu/trusty64
        A `Vagrantfile` has been placed in this directory. You are now
        ready to `vagrant up` your first virtual environment! Please read
        the comments in the Vagrantfile as well as documentation on
        `vagrantup.com` for more information on using Vagrant.
You should now see a file named `Vagrantfile` in your directory. This file contains minimal information for Vagrant to know what to do when we want to launch the Virtual Machine. We could modify it to modify the Virtual Machine we are going to launch, but that is advanced usage and proper documentation on that can be found on the [official documentation](http://docs.vagrantup.com/v2/). However, it may be interesting to understand what is actually needed in this file, since it contains a lot of commented information. What is the minimal content of the `Vagrantfile` is the following lines:

        VAGRANTFILE_API_VERSION = "2"
        Vagrant.configure("VAGRANTFILE_API_VERSION") do |config|
            config.vm.box = "hashicorp/trusty64"
        end
This basically defines which version of the Vagrant API is used and then use it to build the Virtual Machine using the box given as a base. This is the minimal information that any `Vagrantfile` must contain.

Now, to launch the Virtual Machine you only need to use a single simple command (this may take some time since Vagrant is going to boot the Virtual Machine and make its basic configuration):

        $> vagrant up
        Bringing machine 'default' up with 'virtualbox' provider...
        ==> default: Importing base box 'ubuntu/trusty64'...
        ==> default: Matching MAC address for NAT networking...
        ==> default: Checking if box 'ubuntu/trusty64' is up to date...
        ==> default: Setting the name of the VM: vagrant_default_1425476252413_67101
        ==> default: Clearing any previously set forwarded ports...
        ==> default: Clearing any previously set network interfaces...
        ==> default: Preparing network interfaces based on configuration...
            default: Adapter 1: nat
        ==> default: Forwarding ports...
            default: 22 => 2222 (adapter 1)
        ==> default: Booting VM...
        ==> default: Waiting for machine to boot. This may take a few minutes...
            default: SSH address: 127.0.0.1:2222
            default: SSH username: vagrant
            default: SSH auth method: private key
            default: Warning: Connection timeout. Retrying...
            default: Warning: Remote connection disconnect. Retrying...
        ==> default: Machine booted and ready!
        ==> default: Checking for guest additions in VM...
        ==> default: Mounting shared folders...
            default: /vagrant => /tmp/vagrant
You're Virtual Machine is now up and running and accessing it is also quite simple:

        $> vagrant ssh
You should now be connected to you Virtual Machine and ready to work.

An interesting feature of Vagrant is that your computer (the "host") has a shared directory with your Virtual Machine (the "guest"). On your computer that directory is the directory that contains the `Vagrantfile`, on the Virtual Machine, it is `/vagrant`.

So let's assume you have a script that you want to execute from within the Virtual Machine, simply put it in your the directory that contains the `Vagrantfile` and then use it from your Virtual Machine from the `/vagrant` directory.

Note that the opposite is true too: if you put some file in `/vagrant` from the Virtual Machine, it will also be put on you own computer in the directory that contains the `Vagrantfile`, making it an easy way to exchange files between the two systems.

This tutorial is not finished but there is a lot more to learn about Vagrant, so if you're interested in going further we encourage you to continue your learning using the [official documentation](http://docs.vagrantup.com/v2/).