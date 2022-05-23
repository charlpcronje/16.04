# 16.04

## 1 Cannot run apt source by default

The apt source after installing Ubuntu 16.04 will be failed.

```sh
apt source linux
# Reading package lists... Done
# E: You must put some 'source' URIs in your sources.list
```

This is because deb-src is comment out in /etc/apt/sources.list.

```sh
cat /etc/apt/sources.list

# See http://help.ubuntu.com/community/UpgradeNotes for how to upgrade to
# newer versions of the distribution.
deb http://jp.archive.ubuntu.com/ubuntu/ xenial main restricted
# deb-src http://jp.archive.ubuntu.com/ubuntu/ xenial main restricted

# Major bug fix updates produced after the final release of the
# distribution.
deb http://jp.archive.ubuntu.com/ubuntu/ xenial-updates main restricted
# deb-src http://jp.archive.ubuntu.com/ubuntu/ xenial-updates main restricted
```

## 2 Enable deb-src

Add deb-src URL to repository list.

```sh
sudo su -c "grep '^deb ' /etc/apt/sources.list | \
sed 's/^deb/deb-src/g' > /etc/apt/sources.list.d/deb-src.list"
```

Update repository database.

```sh
sudo apt update -y
```

## 3 Execution result

Now the apt source is succeed

```sh
mkdir linux
cd linux
apt source linux

# Reading package lists...
# NOTICE: 'linux' packaging is maintained in the 'Git' version control system at:
# git://git.launchpad.net/~ubuntu-kernel/ubuntu/+source/linux/+git/xenial
# Please use:
# git clone git://git.launchpad.net/~ubuntu-kernel/ubuntu/+source/linux/+git/xenial
# to retrieve the latest (possibly unreleased) updates to the package.
# Need to get 144 MB of source archives.
# Get:1 http://jp.archive.ubuntu.com/ubuntu xenial-updates/main linux 4.4.0-28.47 (dsc) [9,611 B]
# Get:2 http://jp.archive.ubuntu.com/ubuntu xenial-updates/main linux 4.4.0-28.47 (tar) [133 MB]
# Get:3 http://jp.archive.ubuntu.com/ubuntu xenial-updates/main linux 4.4.0-28.47 (diff) [11.3 MB]
# gpgv: Signature made 2016年06月24日 19時02分30秒 JST using RSA key ID FDCE24FC
# gpgv: Can't check signature: public key not found
# dpkg-source: warning: failed to verify signature on ./linux_4.4.0-28.47.dsc
# dpkg-source: info: extracting linux in linux-4.4.0
# dpkg-source: info: unpacking linux_4.4.0.orig.tar.gz
# dpkg-source: info: applying linux_4.4.0-28.47.diff.gz
# dpkg-source: info: upstream files that have been modified:

ls
# linux-4.4.0  linux_4.4.0-28.47.diff.gz  linux_4.4.0-28.47.dsc  linux_4.4.0.orig.tar.gz
```
