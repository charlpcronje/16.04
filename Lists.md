# What is the .LIST file type?

In Debian-style GNU/Linux operating systems such as Ubuntu, the .list filename suffix occurs with reference to package file/directory list files. A .list file (e.g., `amarok-utils.list`) is a plaintext hierarchical listing of all files and directories to be created or written for a given software package installed through the Debian package management system (APT via dpkg). For all installed packages, their .list files, along with other dpkg-specific service files, are located in the `/var/lib/dpkg/info/` directory. Certain packages have their .list files empty.

Apart from that, the "sources.list" file in the `/etc/apt/` directory controls the local software sources by providing a list of active binary and source-code repositories from which all software on a Debian-style system is installed or updated on a Debian system.

﻿
For convenience's sake, the .list suffix is also commonly used for various list files. A generic .list file is just a list of filenames or `pathnames`, usually automatically generated by some file management utility or an OS shell command. As a rule, all such .list files are text-based and can be opened in a text editor or passed as arguments to a file manipulation tool or command.

Another incidence of the .list extension is related to JarIndex files (`INDEX.list`) created in the "META-INF" directory inside Java application archives (JAR). The `INDEX.list` file provides a plaintext list of classes and other resources in one or more `.jar` archives. Indexed JAR's are mainly used in web Java applications, so that classes' locations are known in advance and only the necessary `.jar` files are downloaded, saving the bandwidth and improving responsiveness. One can add an index (`INDEX.list`) to any `JAR` by using the `jar -i` command.

The railroad vehicle and tracks simulation software GENSYS from AB DEsolver uses the `.list` extension for `ASCII` data files generated in the human-readable form by the GENSYS cata_gp tool from the complex GPdat calculation output format.