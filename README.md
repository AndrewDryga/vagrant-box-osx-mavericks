Mac OS X Vagrant box for VirtualBox
=========================
This is a issue tracker for OS X Vagrant boxes, which can be found in Download section

Box was tested only on VirtualBox with Mac OS as a host. Mainly, I made it to build our iOS applications via CI-server.

Also you might be interesed to take look at [radeksimko/vagrant-osx](https://github.com/radeksimko/vagrant-osx) that can build boxes for VMWare Vagrant provider.

Downloads
--
Since VagrantCloud can't host this images, you can use direct links to download them. Download speed may be slow.

* Mac OS X Maverics 10.9 (XCode 5.1): [v0.1.0, direct link](http://files.dryga.com/boxes/osx-mavericks-0.1.0.box)
* Mas OS X Yosemite 10.10 (XCode 6.1): [v0.2.0, direct link ](http://files.dryga.com/boxes/osx-yosemite-0.2.0.box)

OS X Licensing
--
Apple's EULA states that you can install your copy on your actual Apple-hardware, plus up to two VMs running on your Apple-hardware. So using this box on another hardware is may be illigal and you should do it on your own risk.

Setting up
--
1. Install [Vagrant](https://docs.vagrantup.com/v2/installation/) and [VirtualBox](https://www.virtualbox.org/wiki/Downloads);
2. ```cd``` into your project directory;
3. Run ```vagrant init http://files.dryga.com/boxes/osx-yosemite-0.2.0.box```;
4. Your Vagrantfile should be ready as soon as Vagrant downloads box;
5. Start VM by calling ```vagrant up```.

Warning
--
VirtualBox support for Mac OS X is experimental. More information can be found in [official docs](https://www.virtualbox.org/manual/ch03.html#intro-macosxguests).

What's included?
--
* [Homebrew](http://brew.sh/)
* [Homebrew Cask](https://github.com/phinze/homebrew-cask)
* Puppet
* XCode 5.1/6.1
* XCode Command Line Tools

How to accept the Xcode License from the CLI
--
In early versions of this box you need to accept XCode license by you own.

If you face error: ```Agreeing to the Xcode/iOS license requires admin privileges, please re-run as root via sudo.```, just run this command:
```shell
sudo xcodebuild -license accept
```

How to enable the Develop mode
--
When OSX is trying to prompt graphically for password (i.e when using swift REPL), it will raise the error

```error:process exited with status -1) (lost connection)```

because there is no graphical output when using vagrant via ssh login, enable the develop mode can solve this situation, run the following command:

```sudo /usr/sbin/DevToolsSecurity --enable```

Known issues
--
* Do not turn 3D acceleration on, or your Box will start retuning aborted condition and would not start in headless mode
* VirtualBox doen's have Guest additions for Mac OS X, so you can't have shared folders. Instead you can use normal network shared folders
* If you face VM freezed on message ```hfs mounted macintosh hd on device root_device``` then you need to set cpuidset inside your Vagrantfile: ```vb.customize ["modifyvm", :id, "--cpuidset", "1","000206a7","02100800","1fbae3bf","bfebfbff"]```

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
