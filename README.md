# Barebone Debian
Just to remind myself how I set my barebone Debian.

## 1. Install Debian
Downloaded non-free firmware net-install. Placing this [Link](https://cdimage.debian.org/cdimage/unofficial/non-free/cd-including-firmware/current/amd64/iso-cd/) for the version I downloaded when doign this tutorial, but link may break when Debian is up versioned. Burn USB pen and go through the whole installation process, without selecting any meta packages. Don't think it needs to the expert install, as long as no desktop environment is selected whem prompted it should sufficiently lean already.

## 2. Set network
At least in my laptop, I have connection issues, even for ethernet. I had to do the below, but it will be different for each laptop.

### 2.1 Ethernet
First need to cble connect to the router, but doesn't work straight way. To resolve it, first need the list of devices:
```
ls /sys/class/net
```
Should return something like: `enp0s3 lo`. In my case it shows `enp0s31f6 lo`.

Then need to edit `/etc/network/interfaces`, add the below at the end:
```
# The primary network interface
allow-hotplug enp0s31f6
iface enp0s31f6 inet dhcp
```
Replace "enp0s31f6" for whatever shows when listing the devices.
Then problably there's a way to re-start device, but rebooting did the trick. `ping 1.1.1.1` to check if you have connection

### 2.2 Wireless up
In order to have wireless need to install / re-install islwifi. (note, non-free should be included in the repositories list already if the version I indicate in step 1 was used, but follow [link](https://linuxhint.com/enable-non-free-packages-debian-11/) to check it).
```
sudo apt update && sudo apt install firmware-iwlwifi
```

Then to set the wi-fi:
```
sudo ip link set wlp1s0 up
```
Add wifi to `/etc/network/interfaces`:
```
allow-hotplug wlp1s0
iface wlp1s0 inet dhcp
    wpa-ssid ESSID
    wpa-psk PASSWORD
```
Then there are ways of bring up the interface, but simpler to just reboot.


If an error "firmware: failed to load iwl-debug-yoyo.bin (-2)" is showing up at boot, create the follwing file:
```
sudo nano /etc/modprobe.d/iwlwifi.conf
```
And then enter the following content:
```
options iwlwifi enable_ini=N
```
After saving and exiting nano you will need to
```
sudo update-initramfs -u
```
This should disable those messages from appearing during startup.


