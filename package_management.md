###Package Management Systems
The core parts of a Linux distribution and most of its add-on software are installed via the Package Management System. Each package contains the files and other instructions needed to make one software component work on the system. Packages can depend on each other. There are two broad families of package managers: those based on **dpkg** and those which use **rpm** as their low-level package manager. The two systems are incompatible, but provide the same features at a broad level.

**Package Management Systems**

1. ***dpkg***
  * ***apt-get*** (systems based on Debian)
2. ***rpm***
  * ***zypper*** (systems based on SUSE)
  * ***yum*** (systems based on RedHat)

Both package management systems provide two tool levels: a low-level tool (such as ``dpkg`` or ``rpm``), takes care of the details of unpacking individual packages, running scripts, getting the software installed correctly, while a high-level tool (such as ``apt-get``, ``yum``, or ``zypper``) works with groups of packages, downloads packages from the vendor, and figures out dependencies. Most of the time users need work only with the high-level tool, which will take care of calling the low-level tool as needed. Dependency tracking is a particularly important feature of the high-level tool, as it handles the details of finding and installing each dependency for you. Be careful, however, as installing a single package could result in many dozens or even hundreds of dependent packages being installed.
