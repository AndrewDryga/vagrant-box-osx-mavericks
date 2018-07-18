Mac OS X Vagrant box for VirtualBox
=========================
This is a issue tracker for OS X Vagrant boxes, which can be found in Download section

Box was tested only on VirtualBox with Mac OS as a host. Mainly, I made it to build our iOS applications via CI-server.

Downloads
--
Since VagrantCloud can't host this images, you can use direct links to download them. Download speed may be slow.

* Mac OS X Yosemite 10.10 (XCode 6.4): [v0.2.1, direct link](https://vagrant-osx.nyc3.digitaloceanspaces.com/osx-yosemite-0.2.1.box) (13Gb) (sha1: 6339bf812017b9f0046d9fdda79cc6f3d2ab169a)
* macOS Sierra (XCode 8.2): [v0.3.1, direct link](https://vagrant-osx.nyc3.digitaloceanspaces.com/osx-sierra-0.3.1.box) (14.1Gb)

Also you can download older boxes from Hashicorp Atlas page [AndrewDryga/vagrant-box-osx/](https://atlas.hashicorp.com/AndrewDryga/boxes/vagrant-box-osx/) (if it's accessible at the moment), or simply by starting with ```vagrant init AndrewDryga/vagrant-box-osx``` instead of url.

Setting up
--
1. Install [Vagrant](https://docs.vagrantup.com/v2/installation/) and [VirtualBox](https://www.virtualbox.org/wiki/Downloads);
2. ```cd``` into your project directory;
3. Run ```vagrant init https://vagrant-osx.nyc3.digitaloceanspaces.com/osx-sierra-0.3.1.box```;
4. Your Vagrantfile should be ready as soon as Vagrant downloads box;
5. Start VM by calling ```vagrant up```.

OS X Licensing
--
Apple's EULA states that you can install your copy on your actual Apple-hardware, plus up to two VMs running on your Apple-hardware. So using this box on another hardware is may be illegal and you should do it on your own risk.

By using it you agree with all macOS Sierra and XCode license agreements.

What's included?
--
Sierra box:
* [Default Vagrantfile](https://github.com/AndrewDryga/vagrant-box-osx/blob/master/src/include/_Vagrantfile) (inside box) that fixes most of common issues;
* [Homebrew](http://brew.sh/);
* [Homebrew Cask](https://github.com/phinze/homebrew-cask);
* NodeJS 7.2.1 (npm 3.10.10, n 2.1.3);
* Lunchy;
* Puppet 4.8.1;
* XCode 8.2 (and XCode Command Line Tools).

Yosemite box:
* [Default Vagrantfile](https://github.com/AndrewDryga/vagrant-box-osx/blob/master/src/include/_Vagrantfile) (inside box) that fixes most of common issues;
* [Homebrew](http://brew.sh/);
* [Homebrew Cask](https://github.com/phinze/homebrew-cask);
* Puppet 3.7.4;
* XCode 6.4;
* XCode Command Line Tools;
* NodeJS 0.12.7 (for npm);
* Appium 1.4.10;
* iOS Simulator (all devices for iOS 8.4).

Useful cli tools and information
--
* [Nomad CLI](http://nomad-cli.com/) - provides a set of tools that allow to manage certificates, profiles and many other things;
* [ObjC.io Issue 6](http://www.objc.io/issues/6-build-tools/travis-ci/) - how-to article about building apps in cli-only (this one about Travis-Ci);
* ```security``` - use it to manage your keychains;
* [xctool](https://github.com/facebook/xctool) - Facebook project for building iOS apps.

Common issues
--
* Box may [crash on AMD-based hosts](https://github.com/AndrewDryga/vagrant-box-osx/issues/52) due to VirtualBox issues.
* Do not turn 3D acceleration on in VirtualBox, or it will start retuning aborted condition and would not start in headless mode;
* VirtualBox doesn't have Guest additions for Mac OS X, so you can't have shared folders. Instead you can use normal network shared folders ([docs](http://docs.vagrantup.com/v2/synced-folders/nfs.html)):
```
    # Use NFS for the shared folder
    config.vm.synced_folder ".", "/vagrant",
        id: "core",
        :nfs => true,
        :mount_options => ['nolock,vers=3,udp,noatime']
```
* If your VM freezes with ```hfs mounted macintosh hd on device root_device``` then you need to set cpuidset inside your Vagrantfile: ```vb.customize ["modifyvm", :id, "--cpuidset", "1","000206a7","02100800","1fbae3bf","bfebfbff"]``` (it's included since version 0.2);
* If your mouse does not work on a MacBook Pro host machine, shut down the VM and open the VirtualBox Manager. Edit the VM's settings. Choose the _System_ tab. Under the _Motherboard_ sub-tab, set the _Chipset_ option to be _PIIX 3_, and set the _Pointing Device_ option to be _USB Tablet_. Restart the VM;
* When OSX is trying to prompt graphically for password (i.e when using swift REPL), it will raise the error ```error:process exited with status -1) (lost connection)``` because there is no graphical output when using vagrant via ssh login, enable the develop mode can solve this situation, run the following command:
```sudo /usr/sbin/DevToolsSecurity --enable```;
* If you need user password (for example for Homebrew Cask). Vagrant have default consideration to create user ```vagrant``` with password ```vagrant```, you can use it.

Warning
--
VirtualBox support for Mac OS X is experimental. More information can be found in [official docs](https://www.virtualbox.org/manual/ch03.html#intro-macosxguests).

Tips to build your own box
--
Main think you should remember is that you need latest VirtualBox version BEFORE you start installation. Process of installation is pretty straight forward (as on usual Mac), but you need to erase virtual drive during installation via Disk Utilities. After that just follow [Vagrant guide](https://docs.vagrantup.com/v2/boxes/base.html) to create base box and [another one](https://docs.vagrantup.com/v2/virtualbox/boxes.html) to package it.

Sometimes you need to [rebuild VirtualBox kernel extensions](https://gist.github.com/AndrewDryga/9880938) before installing OS on VM.

Helpful links (most of them are outdated):
--
- [Script to create Mac OS installer image](http://ntk.me/2013/12/01/iesd/)
- [Templates to create Host](https://github.com/timsutton/osx-vm-templates)
- [And guide to use it](http://grahamgilbert.com/blog/2013/08/23/creating-an-os-x-base-box-for-vagrant-with-packer/)
- [Another guide by Lifehacker](http://lifehacker.com/5583650/run-mac-os-x-in-virtualbox-on-windows/all)
- [Legal question](http://www.tomshardware.co.uk/answers/id-1802838/illegal-run-osx-virtual-box.html)
- [How to disable default mounting of shared folder](https://github.com/mitchellh/vagrant/issues/1004)
- Also you might be interested to take look at [radeksimko/vagrant-osx](https://github.com/radeksimko/vagrant-osx) that can build boxes for VMWare Vagrant provider.

Sponsors
--
I want to thank DigitalOcean for providing a free storage for box images.

<a href="https://www.digitalocean.com/products/object-storage/"><img src="https://www.digitalocean.com/assets/media/logos-badges/png/DO_Powered_by_Badge_blue-fe4c6688.png" width="150" /></a>
