Install the Driver
With an official Ubuntu kernel (with a high enough version number), the installation should succeed. This section is provided mainly for other readers who may not have gotten as far as you did in installing the driver. You (and anyone else who knows how to do the rest of the driver installation) do not have to follow this part yourself.

Install dependencies:

```sh

sudo apt update
sudo apt install build-essential linux-headers-generic dkms

```

After cding to the directory where you have unpacked the DisplayLink USB Graphics Software for Ubuntu 1.3.54.zip file, run the installer:

```sh

sudo ./displaylink-driver-1.3.54.run
```
It has to build (at least part of) the driver behind the scenes, and on some computers this might be slow, so don't worry if it doesn't finish immediately.

You should see something like this as the output:
```sh
Verifying archive integrity... All good.
Uncompressing DisplayLink Linux Driver 1.3.54  100%
DisplayLink Linux Software 1.3.54 install script called: install
Distribution discovered: Ubuntu 16.04.2 LTS
Installing
Configuring EVDI DKMS module
Registering EVDI kernel module with DKMS
Building EVDI kernel module with DKMS
Installing EVDI kernel module to kernel tree
EVDI kernel module built successfully
Installing x64-ubuntu-1604/DisplayLinkManager
Installing libraries
Installing firmware packages
Installing license file
Adding udev rule for DisplayLink DL-3xxx/5xxx devices
If you see something like that, and there are no errors, then the installation worked. I suggest rebooting before attempting to use your DisplayLink device, though running sudo modprobe evdi seems to load the driver successfully even without an intervening reboot.
```
