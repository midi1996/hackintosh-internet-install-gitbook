# Requirements...

### Physical requirements

OK. So this thing requires a lot of stuff to prepare.

* \***KNOW YOUR HARDWARE**\*
  * Your CPU name, generation
  * Your RAM size \(and slots used if needed\)
  * Your GPUs \(All of them, Intel, AMD, Nvidia. Laptop users, you may have 2 GPUs, only the intel one will work, no questions asked\)
  * Your Storage Devices \(HDD/SSD, SATA/M.2, NVME/AHCI/RAID/IDE configuration. Note: Only NVME and AHCI/M.2 or AHCI/SATA will work. Other configurations may be harder to get by. RST users need to disable it, it can be named Intel Rapid Storage, RST or RAID\)
  * Your screen resolution \(for laptops essentially\)
  * Your Audio codec
  * Your Motherboard Model OR your Laptop Model
  * Your **LAN or Ethernet chipset**
  * Your WLAN/BT chipset
* **A BASIC KNOWLEDGE ON COMMAND LINES AND HOW TO USE A TERMINAL/COMMAND PROMPT**
  * This is not just \[CRUCIAL\], this is the basis of this whole guide. Dont come crying at me because you dont know how to cd to directory or delete a file.
* Your target computer **MUST** be an Intel Core/Xeon machine:
  * Machines with Pentium may get it working with a FakeCPUID in the clover configuration later brought up in this guide
  * Machines with Celerons may have issues
  * Machines with Atom can use that to get a better machine
  * Those 3 CPU types above will not get onboard intel graphics working \(only some rare cases of new Pentium CPUs, but that's not tested by me or recommended\), you will need a dedicated GPU for it.
  * Laptop users with any of the 3 above CPU can also look for a new device, _this will not work for you_.
  * AMD machines are harder to make, thought this method has been tested with AMD but the guide for it \*may\* be released by someone else \(I wont be updating my guide since I have 0 AMD hackintosh experience\). Hence this guide will be Intel-focused.
* A minimum of 4GB USB drive
  * Note: if you have a rooted android phone, look for DriveDroid, and make sure you have a shared internal storage \(no separate /data partition\) usually all phones made after 2012 should be like that, so if yours is fairly new it will handle it just fine.
  * Note2: **use a USB2.0 drive,** HDDs may not be a good choice, also if you don't have any USB2.0, plug the USB in a USB2.0 port if available, or use a USB extension cord that doesn't support USB3.0, this way the USB3.0 drive will run in USB2.0 mode.
* _**\[CRUCIAL\]**_ a **LAN connection** \(no wifi, no wifi dongles, Ethernet USB adapter may work depending on macOS support\) and you must know your LAN card's model \(and your internet speed\)
  * For lappies: you must either have a physical lan port, or a compatible macOS ethernet dongle/adapter, or in case your have a compatible wifi card, it's also good but not recommended \(unless it's the only way to go\)
  * For people who can't use ethernet but have an android phone, you can connect your android phone to WiFi and then tether it using USB. IOS users can do this as well but you need the tetherme tweak on a jailbroken iphone. Unfortnunately if you are not on the right version (any ios version up to 12.1.2) this is not possible. :/
* A **fast internet connection** \(20Mbps downlink may take about an hour for the install procedure, the faster the better\).
  * Users have complained of slow or locked up downloads, that's mainly due to slow or unstable internet.
* A **Proper OS Installation:**
  * Be it:
    * macOS \(a fairly recent one would be better\)
    * Windows \(Windows 10, 1703 or newer\)
    * Linux \(with python2.7 or later\), make sure it's clean and properly functioning.
  * And **15GB** of free space on the drive you're working on. On Windows, your OS disk must have 15GB free at least
* Some googling skills, which a lot of you lack sadly.
* **Brain** cells and patience and reading capabilities _**\[CRUCIAL\]**_

### Non-physical requirements

* Python 2.7 or greater:
  * For Windows, get it from [https://www.python.org/downloads/windows/](https://www.python.org/downloads/windows/) and make sure you enable "add to PATH" in the install
  * For linux users, install it if you dont have it following your distro's tools
  * For macOS users, you already have 2.7+ version installed, no need for extra tools
* ProperTree \[Recommended\]: a simple tool to edit plist files, from /u/corpnewt [https://github.com/corpnewt/ProperTree](https://github.com/corpnewt/ProperTree)
  * Or text editor: Notepad++, Sublime Text, VSCode...
    * Note: on October/fall 2018 Windows Update, the native notepad can work too. Older versions of windows must use a 3rd party text editor. If you dont know what this is, get a one of the text editors above.
* gibMacOS: a sweet tool from /u/corpnewt [https://github.com/corpnewt/gibMacOS](https://github.com/corpnewt/gibMacOS)
  * if you have git on windows use it to clone the repo
  * if you dont, press `Clone or Download` button and download as Zip, extract it somewhere
* Other software requirements will be downloaded thorough the guide \(OS specific\)

## Dualbooting

If you're thinking of dualbooting macOS and other OS, check this guide [here](https://hackintosh-multiboot.gitbook.io/), then come back and finish what you started.

Now you're probably ready for the next step...

