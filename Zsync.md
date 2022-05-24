# Using zsync to download Ubuntu

`zsync` is a file transfer utility that operates at remarkable levels of efficiency without requiring a specialized server or network implementation, instead operating over the same `HTTP` protocol that serves billions of web pages every hour over the public internet. Making use of the same algorithm that powers the venerable rsync file copy utility, it quickly identifies only the parts of a file that have changed between two copies of a file and retrieves only those, ending the need to download a full copy of a file after every new release in order to remain up-to-date.

Ubuntu has distributed its installation images using `zsync` since August 2009, since the `ISO` format used is perfectly suited to make use of the utility's advantages. The result is that it is now trivially easy to keep a perpetually up-to-date local copy of one or many Ubuntu installation ISOs with the most minimal investment of time and bandwidth, since the daily installer images generally change very little.

## Contents

- Using zsync to download Ubuntu
- Installing zsync
- Acquiring the ISO
- Updating the ISO
- Updating with rsync instead
- Another element of `zsync's` allure lies in its ability to performs a checksum comparison that makes it possible to change the flavour of the ISO after you've downloaded it to your system. Let's suppose that you've already downloaded the standard ISO image for Ubuntu Desktop, then later decide that you want to install Kubuntu instead. Simply supply the checksum for the `Kubuntu` ISO along with the path to the standard `ISO` you already have to zsync, and it will download just the parts that differ between the two versions.

## Installing zsync

Open a terminal session and enter the following command:

```sh
sudo apt -y install zsync
```

You will asked to authenticate with your password and then zsync will be downloaded and installed on your system.

## Acquiring the ISO

All Ubuntu CD images are always available at [https://cdimage.ubuntu.com/](https://cdimage.ubuntu.com/), as well as at any of our 400+ mirror sites across the globe.

## Updating the ISO

The Ubuntu archives provide the necessary zsync control files alongside each of the ISO images in the very same directory, such that if you already know the URI for the installation image you want, you also know the URI for its `zsync` file, which is identical except for the addition of `.zsync` to the end of the filename.

For instance these are the URIs for the latest daily build of the Ubuntu 20.10 ("Impish Indri") x86_64 image and its zsync control file:

- https://cdimage.ubuntu.com/daily-live/current/impish-desktop-amd64.iso
- https://cdimage.ubuntu.com/daily-live/current/impish-desktop-amd64.iso.zsync

Knowing that, we can return to our terminal session, navigate to a directory of our choice and use zsync to sync an existing image found there (or download one in full, if none are present) with a single command [ Footnotes 1 2 ]:

zsync `https://cdimage.ubuntu.com/daily-live/current/impish-desktop-amd64.iso.zsync`

## Updating with rsync instead

Another way to update your ISO is by using rsync. rsync is another implementation of the same algorithm that zsync uses, but it requires special software at the server end and is generally less efficient (i.e. requires more data to be downloaded) for ISO images. `https://launchpad.net/products/rsync` is a good resource for more information. A number of the Ubuntu servers also work as rsync servers with similar URIs to the websites. For example:

```sh
rsync -LzhhP rsync://cdimage.ubuntu.com/cdimage/daily-live/current/utopic-desktop-amd64.iso .
```

will sync the server's daily Utopic desktop image (for amd64) to your local system with an older desktop image already stored on your hard drive. -L is to "resolve symbolic links" (sometimes used internally, you want a hard copy), -z is compression, -hh is human readable file size (in KB or MB), and -P is a progress indicator.

You can ascertain the filename to download by getting a recent URL from `http://cdimage.ubuntu.com/cdimage/daily-live/current/` and substituting http for rsync.

If you have a local Ubuntu archive mirror or cache, you can also use Jigdo, rsync is handy for finishing up Jigdo downloads that still have a few missing pieces.

Note: If you are using other flavours of Ubuntu you can add the flavour's name in the rsync path after cdimage/, e.g. for kubuntu :

```sh
rsync -LzhhP rsync://cdimage.ubuntu.com/cdimage/kubuntu/daily-live/current/utopic-desktop-amd64.iso .
```

A slightly more advanced script will automate much of the syncing process. To run this script successfully, you will need to set the DIR variable according to your needs. You can set ISO inside the script or simply pass it in on the command-line (to make it easier to sync multiple ISO images). In some cases you may need to manually set ISOPATH if you are downloading an image not automatically detected.

```sh
# SPDX-FileCopyrightText: © Henrik Nilsen Omma <henrik@ubuntu.com>
# SPDX-FileCopyrightText: © 2010 Aaditya Bhatia <aadityabhatia@gmail.com>
# SPDX-FileCopyrightText: 🄯 2021 Peter J. Mello <RogueScholar@users.noreply.github.com>
#
# SPDX-License-Identifier: GPL-3.0-or-later
#
# This script updates individual ISO image from cdimage.ubuntu.com via rsync

# Uncomment to adjust the ISOPATH, if you need to
# ISOPATH="cdimage.ubuntu.com/cdimage/LOCATION/current"

# If invoked with an argument, use that as the image name
if [ -n "${1}" ] && [ $(expr "${1}" : '.*\.iso$') -gt 4 ]; then
  ISO="$(basename ${1})"
  cd "$(dirname ${1})" || exit 1
else
  echo -e "\\nUsage:\\n\\t${BASH_SOURCE[0]} UBUNTU-IMAGE-FILENAME.iso"
  exit 1
fi

if [ -z "${ISOPATH}" ]; then
  case ${ISO} in
    *-desktop-*)ISOPATH="cdimage.ubuntu.com/cdimage/daily-live/current";;
    *-canary-*)ISOPATH="cdimage.ubuntu.com/cdimage/daily-canary/current";;
    *-preinstalled-*)ISOPATH="cdimage.ubuntu.com/cdimage/daily-preinstalled/current";;
    *)echo 'Unrecognized distribution, set $ISOPATH manually'; exit 2;;
  esac
fi

if curl -sI "https://${ISOPATH}/${ISO}" | head -1 | grep 200 >/dev/null; then
  echo "Location: rsync://${ISOPATH}/${ISO}"
else
  echo "Not Found: http://${ISOPATH}/${ISO}"; exit 2
fi

echo -n "Fetching file validation checksum from the server... "
curl -s "https://${ISOPATH}/MD5SUMS" | grep ${ISO} >${ISO}.md5 && echo "done." ||
  echo "failed."; exit 1

echo -n "Comparing downloaded file with checksum... "
md5sum --status -c ${ISO}.md5 && echo 'match!\nLocal copy is up to date.';rm ${ISO}.md5 ||
  echo 'mismatch!\nPerforming rsync...'; rsync -avzhhP "rsync://${ISOPATH}/${ISO}" .

echo -n "Retrying checksum validation... "
md5sum --status -c "${ISO}.md5" && echo 'match!\nDownloaded successfully!'; rm "${ISO}.md5" ||
  echo 'mismatch!\nDownload failed.'; exit 2
```

This script downloads `MD5SUMs` from the server, checks it against the local copy's MD5SUM. If they are not identical, it syncs the local ISO image to the most recent ISO image on the Ubuntu servers. Then it compares the new local MD5SUM against the server's copy again to check whether the sync process was successful displaying the result. Even though the differences between the images may be higher the total download size is very less and can be used easily.


If you are using other flavours of Ubuntu don't forget to add the flavour's name in the zsync path after cdimage/, for example
    `zsync https://cdimage.ubuntu.com/kubuntu/daily-live/current/impish-desktop-amd64.iso.zsync`
or
    `zsync https://cdimage.ubuntu.com/xubuntu/daily-live/current/impish-desktop-amd64.iso.zsync (1)`

If your system isn't x86_64 architecture, don't forget to change amd64 in the URI with the correct architecture name, like     
    `zsync https://cdimage.ubuntu.com/daily-live/current/impish-desktop-i386.iso.zsync (2)`
