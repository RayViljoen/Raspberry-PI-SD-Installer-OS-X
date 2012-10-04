# Raspberry PI SD Installer OS X

### Shell Script for creating Raspberry PI SD card on OS X.

---

#####Usage:

1. Simply execute the install script from Terminal and pass the image to write. (Image, not ZIP)

	eg. `sudo ./pidisk ~/Downloads/wheezyDebian.img`

2. Select the disk to write the image to by selecting the disk number provided in the output.

3. Wait for disk to finish writing. You can check the write progress with `Ctrl+T`.

---

#####CAUTION:

**Make absolutely sure to select the correct disk from he list of mounted disks output by the script.
Selecting your system drive WILL overwrite it!**

If you are unsure which which disk you need to select you can remove the SD card, check the mounted disks by running `df -hl` and 
then re-check after re-inserting the SD card.

The disk name will likely be something similar to: `/dev/disk2s1`

---

####[Download SD Card Images here.](http://www.raspberrypi.org/downloads)