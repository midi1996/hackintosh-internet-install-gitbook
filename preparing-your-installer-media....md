# Preparing your installer media...

After the download, you'll find a new folder named `macOS downloads` where the program we used downloaded macOS recovery image for your macOS version. For this part of the setup. I'll be separating it to the 3 main OSes and how to make it.

## Winders \(windows\)

Thanks to /u/corpnewt time and head butting, he made a pretty good script to make the installer on the USB.

Inside the same folder of gibMacOS, you'll find a cute `MakeInstall.bat` , double click it, accept UAC admin user, you'll be greeted by a CMD screen. The software will download software needed \(`dd` port for windows and 7-zip\). Then you'll be greeted with a list of drives:

![](.gitbook/assets/image%20%282%29.png)

* **Carefully** choose your drive \(look for the size, name or other features that points to your USB device\).
* type in its number
* press enter and follow the rest of the instructions
* You'll be asked to drop the Recovery image path, go to `macOS downloads` and explore its folders, until you get to a file with `.pkg` in the end, named either `RecoveryHDMetaDmg` or `RecoveryHDUpdate` \(depending on your macOS version\).
* Grag-n-Drop **will not work**, so the workaround is `Shift` + _Right click_ on the file, then select `Copy as Path` as shown here

![](.gitbook/assets/image%20%283%29.png)

* then paste it in the CMD window \(by Right Clicking in the window, this will paste what's in the clip board\)
* the program will then extract and restore to the USB the recovery downloaded earlier.
* You may continue go to the next section of this guide...

## Loonix \(Linux\)

This section will target making the necessary partitions in the USB device. You can use your favorite program be it `gdisk` `fdisk` `parted` `gparted` or `gnome-disks`. I will focus on `gdisk` as it's nice ¯\\_\(ツ\)\_/¯ and can change the partition type later on, as we need it so that macOS Recovery HD can boot. \[the distro used here is Ubuntu 18.04, other versions or distros may work\]

In terminal:

1. run `lsblk` and determine your USB device block
2. run `sudo gdisk /dev/<your USB block>`
   1. send `p` to print your block's partitions \(and verify it's the one needed\)
   2. send `o` to clear the partition table and make a new GPT one
      1. confirm with `y`
   3. send `n`
      1. partition number: keep blank for default
      2. first sector: keep blank for default
      3. last sector: `+200M` to create a 200MB partition that will be named later on CLOVER
      4. Hex code or GUID: `0700` for Microsoft basic data
   4. send `n`
      1. partition number: keep blank for default
      2. first sector: keep blank for default
      3. last sector: keep black for default \(or you can make it `+3G` if you want to partition further the rest of the USB\)
      4. Hex code or GUID: `ab00` for Recovery HD partition scheme
   5. send `w`
      1. Confirm with `y`
   6. Close `gdisk` by sending `q`
3. Use `lsblk` again to determine the 200MB drive and the other partition
4. run `mkfs.vfat -F 32 -n "CLOVER" /dev/<your 200MB partition block>` to format the 200MB partition to FAT32, named CLOVER
5. then `cd` to `macOS downloads` and keep going until you get to a `pkg` file
   1. download `p7zip-full` \(depending on your distro tools\)
      1. for ubuntu/ubuntu-based run `sudo apt install p7zip-full`
      2. for arch/arch-based run `sudo pacman -S p7zip`
      3. for the rest of you, you should know ¯\\_\(ツ\)\_/¯
   2. run this `7z e -txar *.pkg *.dmg; 7z e *.dmg */Base*; 7z e -tdmg Base*.dmg *.hfs` this will extract the recovery from the pkg through extracting the recovery update package then extracting the recovery dmg then the hfs image from it.
   3. then when you get `4.hfs` or `3.hfs` \(depending on the macOS version used\) run `dd if=*.hfs of=/dev/<your USB's second partition block> bs=8M --progress` \(you may change the input file `if` and the block size to match your needs
6. after that, you're ready to go for the next step of the guide...

## crackOS \(macOS\)

After the download, open `macOS downloads/.../...` until you find RecoveryHDUpdate.pkg or RecoveryHDMetaDmg.pkg \(enable file name extensions in Finder under Finder &gt; Preferences &gt; Advanced\).

1. locate the pkgs
2. do not open them
3. rename the extensions from `.pkg` to `.dmg`
4. open the image resulted \(it will be mounted\)
5. open `Disk Utility` application \(Launchpad &gt; Other &gt;\)
   1. Select View &gt; Show All devices \(above the drive list\)
   2. Select your USB device \(not the partition\)
   3. Select Partition
      1. if you do not see partition:
         1. Select Erase
         2. Name: whatever Format: MacOS Extended Journaled Scheme: GUID Partition Table
   4. Make:
      1. a minimum of 200MB partition Name: CLOVER Format: MSDOS
      2. the rest Name: whatever you want Format: MacOS Extended Journaled
   5. Apply
   6. Select the MacOS Extended Journaled partition
   7. Select Restore
   8. Choose BaseSystem from the list \(it should be the image you mounted earlier\)
   9. Let it restore
6. When done you can continue this guide...



