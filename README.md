# Replacement img for the Seestar S50

Disclaimer: This guide is intended for educational purposes only. Flashing a new firmware image onto your Seestar device is risky and can potentially brick your device, making it inoperable. Please proceed with caution and understand that there is no guarantee of success, and reverting back to the original firmware may not be possible once you proceed. Always ensure you have a complete backup of your device's current state. We are not responsible for any damage to your device or loss of data that may occur from following this guide. These instructions are for a Linux enviroment tested on an fresh Ubuntu 22.04.4 install.

You can place the extracted seestarOS.img in a direcotry with the canOPENER.sh script and it will preform this for you and walk you through the process of placing the seestar in maskrom mode.

This process places a new img on the eMMC of the seestar. It overwrites everything. This image is ssh enabled, compatibilty for asiair app is enabled, and save all images is enabled. These settings are found in the file ~/.ZWO/ASIAIR_imager.xml

login: pi 

password: raspberry

---

## seestarOS.img

- Download via torrent: [magnet link](magnet:?xt=urn:btih:d064361ce9aaaef2328ab704835badb343e1f557&dn=seestarOS.zip&tr=udp%3A%2F%2Ftracker.openbittorrent.com%3A80&tr=udp%3A%2F%2Ftracker.coppersurfer.tk%3A6969&tr=udp%3A%2F%2Ftracker.leechers-paradise.org%3A6969&tr=udp%3A%2F%2Fexplodie.org%3A6969&tr=udp%3A%2F%2Ftracker.opentrackr.org%3A1337&tr=udp%3A%2F%2Ftracker.internetwarriors.net%3A1337)
- Direct download: [Google Drive](https://drive.google.com/file/d/1oWCFLvSCC0e7uNRTzw12Eg6EKf1ajdbA/view?usp=drive_link)


The zip file is 1.7 GB and extracts to 62.5 GB so make sure you have enough room to hold the img. Verify that the parittion table is good by running:

```
fdisk -l seestarOS.img
```

It should display very similar results to this:

```
Disk seestarOS.img: 58.25 GiB, 62545461248 bytes, 122159104 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 73987B6B-4974-4C94-A3E8-58AB2EB7A946

Device            Start       End   Sectors  Size Type
seestarOS.img1    16384     24575      8192    4M unknown
seestarOS.img2    24576     32767      8192    4M unknown
seestarOS.img3    32768    163839    131072   64M unknown
seestarOS.img4   163840    229375     65536   32M unknown
seestarOS.img5   229376   1277951   1048576  512M unknown
seestarOS.img6  1277952  11763711  10485760    5G unknown
seestarOS.img7 11763712  15958015   4194304    2G unknown
seestarOS.img8 15958016 122159040 106201025 50.6G unknown
```

---

## Prerequisites: Install Necessary Dependencies

Before you start, ensure your system is up to date and has all the necessary dependencies installed. Open a terminal and execute the following commands:

```
sudo apt-get update
sudo apt-get install -y python2 git libusb-1.0-0-dev libudev-dev pkg-config autoconf libtool build-essential
git clone https://github.com/rockchip-linux/rkdeveloptool.git
cd rkdeveloptool
autoreconf -i && ./configure CXXFLAGS="-Wno-error=format-truncation=" && make
sudo make install
```

---

## Preparing the Seestar Device

To flash a new image, your Seestar must be in Maskrom mode. Follow these steps to prepare your device:

1) Remove the battery from the Seestar device. Located on the bottom of the seestar under a cover with two philip screws.
2) Hold the reset button located at the bottom of the device While holding the reset button, connect your device to your computer using a USB cable.
3) Continue holding the reset button for 5 seconds after the device is connected.
4) Ensure no lights are on on the device to confirm it's in Maskrom mode.
5) If the device does not connect, repeat the steps to ensure it enters Maskrom mode.

### Verifying Connection in Maskrom Mode

To verify that your Seestar is connected and in the correct mode, use the following rkdeveloptool commands:

```
sudo rkdeveloptool ld  # Check if the device is in Maskrom mode
sudo rkdeveloptool rid # Get the device ID
```

---

## Flashing the New Image

To flash the new image onto your Seestar device, use the following command. Be patient, as this process can take some time, over an hour and will show a progress dialog.

```
sudo rkdeveloptool wl 0 seestarOS.img
```

After the process completes:

1) Disconnect the USB cable from the device.
2) Reinstall the battery and power on your Seestar.

This method has been tested and confirmed to work, including app and firmware updates, but your results may vary depending on the specific firmware and device model.

Remember, this process is risky, and you should only proceed if you're comfortable with the potential outcomes, including the possibility of bricking your device.

