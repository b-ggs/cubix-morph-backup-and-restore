# Cherry Mobile Cubix Morph Backup and Restore

After an agonizing weekend of trying to get Windows 10 working with Wi-Fi, I decided to create an image of the eMMC so I wouldn't have to go through that again.

On other computers, I'd just boot into a live Xubuntu environment, connect to the Internet via Wi-Fi, install exFAT utilities from `apt`, and then use `dd` to image the drive and write to an exFAT drive.

But because of the limited port selection and lack of power delivered to the ports, it's a different story for this special snowflake.

## Disclaimer

The contents of this guide will talk specifically about how *I* managed to backup and restore the contents of the eMMC.

Your mileage may vary.

Also, this was written up on 4 AM on a Monday, so it's not gonna be the best guide. Not yet at least. A more thorough guide and better explained guide will follow... hopefully.

## Backup

### Things needed

* A flash drive with Ubuntu 17.10
* A flash drive formatted to exFAT that has at least 32-34 GB of free space to save the image on
  * We'll be connecting this to a USB hub, so n external hard drive probably wouldn't work unless it is externally powered.
* An Android phone and a USB cable for USB tethering
  * Ubuntu doesn't recognize the onboard network card.
* A 4-port USB 2.0 hub
* A USB OTG cable
* A wireless keyboard and mouse

### Backing up

* Connect the USB OTG cable to the microUSB port. Since this probably provides less power, let's just connect our wirleess keyboard and mouse to this port.

* Connect the 4-port USB hub.

* Connect the Ubuntu flash drive, exFAT flash drive, and Android phone to the 4-port USB hub.

* Boot into the BIOS by (repeatedly) hitting `Esc` while turning on the tablet.

* Boot into Ubuntu by selecting the flash drive it is installed on in the Boot Manager.

* Enable USB tethering on your Android phone so our Ubuntu environment can connect to the Internet. Ubuntu will not recognize the on-board network card.

* Run `sudo add-apt-repository universe` and `sudo apt-get install exfat-fuse exfat-utils` so Ubuntu can recognize our exFAT flash drive.

* In case the drive doesn't show up on the Desktop, we can manually mount it by running `lsblk` to check its path, and then `sudo mount -t exfat /dev/sdXY /media/ubuntu/exfat` to mount the drive. Make sure `/media/ubuntu/exfat` exists!

* Take note of the name of the eMMC with `lsblk`. It will most likely be `mmcblk0`.

* Run `sudo dd if=/dev/mmblk0 of=/media/ubuntu/exfat/cubix.img bs=64k conv=sync,noerror status=progress` and it will (slowly) start imaging the drive.

## Restore

### Things needed

* A flash drive with Ubuntu 17.10
* A flash drive formatted to exFAT that contains our image
  * We'll be connecting this to a USB hub, so n external hard drive probably wouldn't work unless it is externally powered.
* An Android phone and a USB cable for USB tethering
  * Ubuntu doesn't recognize the onboard network card.
* A 4-port USB 2.0 hub
* A USB OTG cable
* A wireless keyboard and mouse

### Restoring

* Connect the USB OTG cable to the microUSB port. Since this probably provides less power, let's just connect our wirleess keyboard and mouse to this port.

* Connect the 4-port USB hub.

* Connect the Ubuntu flash drive, exFAT flash drive, and Android phone to the 4-port USB hub.

* Boot into the BIOS by (repeatedly) hitting `Esc` while turning on the tablet.

* Boot into Ubuntu by selecting the flash drive it is installed on in the Boot Manager.

* Enable USB tethering on your Android phone so our Ubuntu environment can connect to the Internet. Ubuntu will not recognize the on-board network card.

* Run `sudo add-apt-repository universe` and `sudo apt-get install exfat-fuse exfat-utils` so Ubuntu can recognize our exFAT flash drive.

* In case the drive doesn't show up on the Desktop, we can manually mount it by running `lsblk` to check its path, and then `sudo mount -t exfat /dev/sdXY /media/ubuntu/exfat` to mount the drive. Make sure `/media/ubuntu/exfat` exists!

* Take note of the name of the eMMC with `lsblk`. It will most likely be `mmcblk0`.

* Run `sudo dd if=/media/ubuntu/exfat/cubix.img of=/dev/mmblk0 bs=64k conv=sync,noerror status=progress` and it will (slowly) start writing the contents of the image to the drive.
