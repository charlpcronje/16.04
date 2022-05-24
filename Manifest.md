# .Manifest files, what are they?

The .manifest files contain all the packages that's been added to the accompanying ISO file. Ubuntu sever does does get packages with a minimal of extra packages, but there are a few that are critical from the start. 

## Some critical packages

|-------------------|-----------------------------|--------------------------------|
| Package           | Versions                    | Description                    |
|-------------------|-----------------------------|--------------------------------|
| adduser	        | 3.113+nmu3ubuntu4           | To add a user to linux         |
| apt	            | 1.2.29ubuntu0.1             | To install new packages        |
| cron	            | 3.0pl1-128ubuntu2           | Run commands on a schedule     |
| dpkg	            | 1.18.4ubuntu1.5             | Also to install packages       |
| findutils	        | 4.6.0+git+2016012           | Find Files                     |
| grep	            | 2.25-1~16.04.1              | search through command output  |
| gzip	            | 1.6-4ubuntu1                | Unzipping and zipping files    |
| makedev	        | 2.3.1-93ubuntu2             | Compile sources to binary      |
| mount	            | 2.27.1-6ubuntu3.6           | Mount drives or net locations  |
| perl-base	        | 5.22.1-9ubuntu0.6           | Perl Programming language      |
| python3	        | 3.5.1-3                     | Python Programming Language    |
| sudo	            | 1.8.16-0ubuntu1.5           | Super user do                  |

that was only a small part of the manifest, but you should be able see how things like installing packages and adding users are critical to the working of the Operating system.

Manifest files can also be used to build your own unique install of Linux with all the packages you need.
