# Jigdo and how to use

- Definition
- Installation
- Use & Example downloading Ubuntu 12.04 (Precise Pangolin)


## Definition

Jigdo (JIGsaw-DOwnload) can be used to efficiently download daily alternate CD builds or update existing alternate .iso images without the need to re-download the entire CD image. It simply checks for differences or updates between an existing CD image (on your system) and the latest released image.

## Installation

Install the required packages for Jigdo.

```sh
sudo apt-get install jigdo-file
```

Use & Example downloading Ubuntu 12.04 (Precise Pangolin)

Start the program from the terminal:

```sh
jigdo-lite
```

Supply the .jigdo file

The program then asks you for the URL to the `.jigdo` template you want to download.

```sh
Jigsaw Download "lite"
Copyright (C) 2001-2005  |  jigdo@
Richard Atterer          |  atterer.net
Getting mirror information from /etc/apt/sources.list

-----------------------------------------------------------------
To resume a half-finished download, enter name of .jigdo file.
To start a new download, enter URL of .jigdo file.
You can also enter several URLs/filenames, separated with spaces,
or enumerate in {}, e.g. `http://server/cd-{1_NONUS,2,3}.jigdo'
jigdo: 
```

In this situation we'll use Alternate install CD for PC (Intel x86) computers (copy link-location & paste into prompt above)

## Supply previous CD image

Jigdo will then download a copy of the `.jigdo` file and prompt you for any previous images you have to base the download from. Here you can point Jigdo to an existing burned copy of the CD image, or loop-mounted ISO image (see MountIso). (`eg. /media/cdrom or /media/cdimage`)

```sh
If you already have a previous version of the CD you are
downloading, jigdo can re-use files on the old CD that are also
present in the new image, and you do not need to download them
again. Mount the old CD ROM and enter the path it is mounted under
(e.g. `/mnt/cdrom').
Alternatively, just press enter if you want to start downloading
the remaining files.
```

If you didn't burn the old image to a CD, you can loop-mount it (see MountIso)

```sh
sudo mkdir /media/cdimage
sudo mount -o loop precise-alternate-i386.iso /media/cdimage
```

## Comparing files

Jigdo then scans the existing files & compares them with the latest copies from the `.jigdo` template.

```sh
Found xxxx of the xxxx files required by the template
Copied input files to temporary file `precise-alternate-i386.iso.tmp' - repeat command and supply more files to continue
```

At this point you can include any additional sources you might have (optional) or press ENTER to continue to the next step.

If you have no additional sources you can press ENTER twice and it will download the remaining packages that it needs. This, again, significantly cuts down on bandwidth and download time & easily keeps you up to date on the latest daily builds.

## Update Precise Pangolin daily alternate iso image, terminal commands

To upgrade from an already dl Precise Pangolin iso image with less jigdo-lite confirmation use the terminal commands below:

```sh
sudo mkdir /media/cdimage
mv precise-alternate-i386.iso precise-alternate-i386.iso.old
sudo mount -o loop precise-alternate-i386.iso.old /media/cdimage
jigdo-lite --scan /media/cdimage --noask http://cdimage.ubuntu.com/daily/current/precise-alternate-i386.jigdo
sudo umount /media/cdimage; sudo rm precise-alternate-i386.iso.old
```
