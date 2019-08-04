# INSTALLATION INSTRUCTIONS

##Installing Raspbian Lite on your Raspberry Pi

1. download Raspbian LITE here: https://www.raspberrypi.org/downloads/raspbian/
2. Insert your SD card into your computer.
3. Locate the device, by running `sudo fdisk -l`. It will probably be the only disk about the right size. Note down the device name; let us suppose it is `/dev/sdx`. If you are in any doubt, remove the card, run `sudo fdisk -l` again and note down what disks are there. Insert the SD card again, run `sudo fdisk -l` and it is the new disk.
Unmount the partitions by running `sudo umount /dev/sdx*`. It may give an error saying the disk isn't mounted - that's fine.
4. Copy the contents of the image file onto the SD card by running
    

5. Copy the contents of the image file onto the SD card by running `sudo dd bs=1M if=your_image_file_name.img of=/dev/sdx`
Of course, you'll need to change the name of the image file above as appropriate.

!!Warning There is a significant risk of damage to your filesystem if you use the wrong `/dev/sdx`. Make sure you get it right!

6. Create an emty file called `ssh` and copy it to the root partition of the os (This enables SSH on first boot)

7. OPTIONAL: if you want your pi to automaticaly connect to Wifi, have a look at [this](https://www.techcoil.com/blog/how-to-setup-raspbian-stretch-lite-with-remote-configuration-over-wifi-on-first-boot/)

## Raspbian Setup

1. Insert SD card into Raspbery Pi and connect device to power and your network (wired)
2. Find out what IP has been assigned to the Pi (via router or other means)
3. SSH onto the Pi:
  `ssh pi@192.168.1.109`  (example IP)
  Username: pi
  Password: raspberry

4. Change password with `passwd`
5. Configure with `sudo raspi-config` (network, locale, timezone). See [this](https://www.techcoil.com/blog/set-of-configurations-to-perform-on-the-first-run-of-your-raspbian-stretch-lite/)
 for more information

6. Update
`sudo apt-get update`
`sudo apt update --allow-releaseinfo-change`
`sudo apt-get upgrade`

7. Set static IP
edit the file `/etc/dhcpcd.conf`
use `nano /etc/dhcpcd.conf` and copy the following text:

```
interface eth0
static ip_address=192.168.0.201/24
static routers=192.168.0.1
static domain_name_servers=192.168.0.1 8.8.8.8

interface wlan0
static ip_address=192.168.0.202/24
static routers=192.168.0.1
static domain_name_servers=192.168.0.1 8.8.8.8
```
Reboot after with
`sudo shutdown -r now`
and connect again via ssh (keep in mind the IP has now changed to the one you set)

## Jenkins Installation

1. install Java 11: `sudo apt-get install openjdk-11-jre openjdk-11-jdk`
2. verify with `java -version`
3. Install Jeninks by running these commands:
```bash

wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update

sudo apt-get install jenkins -y
```

4. Run `systemctl status jenkins.service` to check if Jenkins server is running

5. Open your browser and navigate to  http://RASP_IP:8080/ (for example: http://192.168.0.101:8080/) and follow the instructions there.
When asked to set the Jenkins IP, you should set the outwards facing ip, not the local one (you may need to forward a port or some other configuration in your router)
This will enable GitHub to comunicate with the server.

AAAAAAAAAAAAAAAAAAAa

sledi konfiguracijskim navodilom
( jaz sn zbral insall suggested plugins)

ker moraš nastavit jenkins url je pamento da maš static ip

# inštaliraj python in platform io core (da boš lahko compiliral in flashal kodo)
`sudo python -c "$(curl -fsSL https://raw.githubusercontent.com/platformio/platformio/develop/scripts/get-platformio.py)"`
ta skripta inštalira python, pip, in platformIO Core

preveri z
`platformio --version`

# install 99-platformio-udev.rules
`curl -fsSL https://raw.githubusercontent.com/platformio/platformio-core/master/scripts/99-platformio-udev.rules | sudo tee /etc/udev/rules.d/99-platformio-udev.rules`

# add jenkins to dialout group
`sudo usermod -a -G plugdev jenkins`
`sudo usermod -a -G dialout jenkins`

tako ma dovoljenje za uporabo usb portov
(po tem je potrebno restartat jenkins, da se pravice vspostavijo)
`sudo service jenkins restart`

# install git !!!!
`sudo apt-get install git`


# install stm tools (ne vem če potrebno)
sudo apt-get install gcc-arm-none-eabi binutils-arm-none-eabi libnewlib-arm-none-eabi

sudo apt-get install openocd

sudo apt-get install libftdi1

