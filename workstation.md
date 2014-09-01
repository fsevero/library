Configuring my workstation
====================================
My workstation consists on running a virtual machine inside [windows 8.1](http://windows.microsoft.com/pt-br/windows-8/meet) with an [Ubuntu Server 14.04 Trusty Thar](http://www.ubuntu.com/).

My editor of choice (at the moment) is the [Atom](https://atom.io/) editor, basically because it is Free :)

For the virtual machine, I use a combo of [VirtualBox](https://www.virtualbox.org/) and [Vagrant](https://www.vagrantup.com/). The [Git](http://git-scm.com/) is added basically for the SSH tools.

## Installing **Chocolatey**
[Chocolatey](http://chocolatey.org/) is a tool that try to be like `apt-get` is for Ubuntu. It allows you to manage software by automating their installation/update process.

From administrative cmd, install chocollatey unsing the following command:

```shell
@powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
```

If The code doesn't work, you can try it at PowerShell with the following commands:

```shell
# Allowing execution of scripts
Set-ExecutionPolicy Unrestricted
# Installing
iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))
# Back to the default
Set-ExecutionPolicy Restricted
```
All the following steps will be made at `PowerShell`

## Installing Atom text editor (Install editor of your choice)
```shell
choco install atom
```

## Installing Git
```shell
choco install git
```
Be sure to add Git to your `PATH` system variable, if chocolatey didn't. Add the following (based on the location of yout git installation):
```shell
;C:\Program Files (x86)\Git\bin
```


## Installing VirtualBox
I'm installing version 4.2 of VirtualBox due to [incompatibility with the last version of Vagrant](http://stackoverflow.com/questions/19689632/vagrant-errors-after-windows-8-1-update)
```shell
choco install Devbox-VirtualBox
```

## Installing Vagrant
```shell
choco install vagrant
```

## Working with the virtual machines
Creating the virtual machine configuration files: 

(See [my fork of Torchbox](https://github.com/fsevero/vagrant-django-base) for details of the construction of the django-base-v2.1)
```shell
mkdir myproject
cd myproject
mkdir shared_folder
vagrant init django-base-v2.1
```

Edit the Vagrantfile and add the two configuration lines for the port forwarding and definition of the shared folder, respectively:

```shell
config.vm.network "forwarded_port", guest: 8000, host: 8080
config.vm.synced_folder "shared_folder", "/home/vagrant/shared_folder"
```

Finally, to start and enter the virtual machine:
```shell
vagrant up
vagrant ssh
```

To exit the virtual machine:
```shell
exit
```

## Teardown the machine
To turn off the machine, you have 3 options:

For "hibernation" (both disk and memory will be kept in the host machine):
```shell
vagrant suspend
```
For "turn off" (only disk will be kept in the host machine):
```shell
vagrant halt
```
For "cleaning" (only the configuration files will be kept, all your guest machine data will be lost):
```shell
vagrant destroy
```
