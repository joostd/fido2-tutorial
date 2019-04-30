Using a [Security Key by Yubico](https://www.yubico.com/product/security-key-by-yubico/)
on an Ubuntu 18.04 LTS ([Bionic Beaver](http://releases.ubuntu.com/18.04/)) VM

Software:
- [Virtualbox](https://www.virtualbox.org) with Virtualbox extention pack

macos:

installing using [brew.sh](https://brew.sh):

    brew cask install virtualbox
    brew cask install virtualbox-extension-pack


The Vagrantfile will take care of:
- installing [libfido2](https://developers.yubico.com/libfido2/) from the Yubico PPA
- connecting the USB port from the VM
