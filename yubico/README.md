Using a Security Key from Yubico
on an Ubuntu 18.04 LTS (Bionic Beaver) VM

Software:
- Virtualbox with Virtualbox extention pack

macos:

installing using brew.sh:

    brew cask install virtualbox
    brew cask install virtualbox-extension-pack


The Vagrantfile will take care of:
- installing libfido2 from the Yubico PPA
- connecting the USB port from the VM
