---
layout: post
title: Fedora Linux installation on Beagle Bone Black
date: 2017-02-01
tags:
  - linux
  - electronics
---

Recently I've spent about 8-10 hours trying to install Fedora Linux 25 on a Beagle Bone Black SD card. I had no serial cable, only a HDMI-mini HDMI adapter, keyboard and screen. I'd like to share my experience so others may learn from it.

The first step is to download a suitable Fedora spin. It has to be one of the `armhf` ones available on the [official sites](https://arm.fedoraproject.org/). I've chosen the minimal distribution but any will do really. Remember to check the checksum and the signature of the download:

{% gist 821ad09b7e3fbebcfe31a6bf34e91af5 %}

After downloading the `raw.xz` file it has to be written to th SD card. There is a number of ways to do that. The simplest one is to just `xzcat` it to the `/dev/mmblk0` device, but it is not the best choice.

A far better one (if you're preforming the operation on a Fedora desktop) is to use the `fedora-arm-installer` package as follows:

{% gist be2264f65e0fb663bba36a01b4c9d169 %}

There are a number of benefits with this method. The `fedora-arm-installer` script performs various additional actions on the written image eg. resizing the SD card filesystem, enabling SELinux, disabling root password etc. The same script can be used to write images for various other boards. For a list of supported targets check the `/usr/share/doc/fedora-arm-installer/SUPPORTED-BOARDS` file.

If you do not use Fedora but want to write a image to the SD card you can simply view the [source code](https://github.com/sorki/fedora-arm-installer) of the `fedora-arm-installer` -- it is a normal shell script.

After the script finishes, the SD card should be bootable, but to sucessfully boot it you should hold the "User Boot" button (small one next to the SD card reader). Otherwise the leds will stay lit and the Beagle Bone Black will be unresponsive, the connected screen shall remain black. Debugging this situation usually requires a serial cable.

Pressing the "User Boot" button on every boot is of course unacceptable if you wish to use this board as a server. There is a number of ways to boot the SD card automatically but the easiest one, which worked for me, was to mount the eMMC boot partition on the booted Fedora system and rename the MLO file.

{% gist 4ad7a156a7c83d0a121a162b74035a8d %}

After this operation the Beagle Bone Black should boot the SD card automatically.

If you have any questions, comments or know a better way of setting up Fedora for the Beagle Bone Black board please leave a comment.
