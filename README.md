# First contact...

Some people can't just get the Vanilla guide from /r/Hackintosh working just right. Reasons can differ: can't set up the VM for whatever reason, can't access a real mac \(although it works on macOS\), download corruptions, small USB disks \(at this time and age? shame on you\), 4GB of RAM \(seriously?\)...

So it happens that real macs can re-install macOS directly from the internet, without the need for a disk or installer support, and they do that through "Internet Recovery". When a mac finds no OS or recovery partition, it will try to load a recovery system from Apple's servers and then boot it then install macOS through the internet to the mac. And following the same idea, we can, as hackintoshers, do the same \(sort of\)

#### How does it work?

So this procedure goes mainly like this

1. Download macOS recovery system \(as we can't do that directly from clover, yet\)
2. Restore the macOS recovery system to the USB
3. Install Clover to the USB
4. Configure Clover \(Kexts and UEFI Drivers\)
5. Boot macOS Recovery Environment
6. Format and whatever you like to do in the macOS installer
7. Install macOS
8. BOOM! We're done.

#### What will this guide cover?

* Requirements
* macOS recovery downloading
* USB device partitioning and macOS recovery image restoring
* Clover configuration
  * Kexts
  * UEFI drivers
  * config.plist \(configuration file\)
    * For laptops
    * For desktops
* Firmware/BIOS general settings advice
* Booting and Installing macOS

This guide will \*probably\* not contain any post-install guide, as it's documented on the [Vanilla Guide](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/). This guide is for Pre-install preparation and Install, you will need to go to the Vanilla Guide to finish the rest for Desktop users, and for laptop users you'll have to continue [here](https://www.tonymacx86.com/threads/guide-booting-the-os-x-installer-on-laptops-with-clover.148093/#post-917904). This will be brought up at the end of this guide.

Good luck :\)...



