---
description: 'Step 1: Prepare Clover'
---

# Clover and friends \[part 1\]

For this part of the guide you'll install and prepare clover for the ramble.

#### Install Clover

1. For Windows, you already did with the tool before, skip to _Prepare Clover_
2. For Linux or macOS:
   1. Download the latest [Clover LZMA](https://github.com/Dids/clover-builder/releases/latest) from Dids' repo \(thanks bb ❤️\)
   2. For linux, use p7zip or file roller or whatever you use, to extract the LZMA and TAR inside it until you get the iso
   3. For macOS, use `keka` \(google it\) or `The Unarchiver` \(AppStore, Free\) to extract the LZMA and TAR inside it until you get to the iso
3. Mount the ISO
   1. On linux, use `mount` command with `-o loop,ro` or with `gnome-disks` by right-clicking the iso &gt; mount image 
   2. On macOS, double click
4. Copy the EFI folder from the ISO to the CLOVER USB
   1. you should have CLOVER \(USB\) &gt; EFI \(folder\) &gt; CLOVER & BOOT

* _**Note for macOS users** who want to use **legacy** on the destination machine:_
  1. If you're booting from UEFI on the destination go to **step 2: Mount the ISO** \(do not follow this note\)
  2. Download the pkg from the source above
  3. Open the installer
  4. Select the destination as "CLOVER" \(USB\)
  5. Select Customize
  6. Choose:
     1. Boot Sectors : Install boot0ss in MBR
     2. Clover for BIOS \(legacy\) booting: Clover EFI 64-bits SATA
     3. Themes: \(a theme, choose clovy\)
     4. BIOS Drivers, 64 bit: ApfsDriverLoader - AppleImageLoader
     5. Uncheck the rest of there is anything checked
  7. Install
  8. Go to **Step 7** below

#### Prepare Clover

1. Open CLOVER &gt; EFI &gt; CLOVER
   1. delete `doc` and `OEM` \(remove if any found\)
   2. Make a folder named `ACPI` with subfolders: `origin` `patched` `WINDOWS` \(case sensitive\)
2. Clover Drivers \(EFI drivers, not to be confused with kexts, which are macOS drivers\)
   1. for UEFI users:
      * Open `drivers > UEFI`, _delete_ everything, **BUT** _ApfsDriverLoader_  _- AptioMemoryFix - HFSPlus_
      * (If _AptioMemoryFix_ or _HFSPlus_ are missing, copy them from `drivers > off`
   2. for Legacy users:
      * go to `drivers > BIOS`, keep it as it is.
3. Delete now:
   1. UEFI users, delete:
      1. `drivers > BIOS`
   2. Legacy users, delete:
      1. `drivers > UEFI`
   3. Optionally delete:
      1. `drivers > off`      
4. Go to kexts &gt; Other
   * Go to [Goldfish64's Kext Repo](https://1drv.ms/f/s!AiP7m5LaOED-m-J8-MLJGnOgAqnjGw)
   * Download these: _\[Note: Explore each folder and you'll find a Zip file, get that Zip file, not the whole folder\]_ 
     * _**Essential Kexts** \(essential for booting\)_
       * _**VirtualSMC**_ \(emulates an SMC system, VSMC has higher quality code than FakeSMC, another older and long developed kext. They both do the same, so _do not_  mix them\)
         * Use only VirtualSMC.kext \(or FakeSMC.kext\) in CLOVER
       * _**Lilu**_ \(an arbitrary kext patcher, required by some other kexts like WhateverGreen, AppleALC...\)
       * _**USBInjectAll**_ \(Injects USB information, configuration is needed\)
         * You will probably not need `XHCI-Unsupported`, dont copy it
       * _**AppleALC**_ \(AppleHDA patcher for audio injection, requires Lilu, it can be omitted until post install\)
       * _**WhateverGreen**_ \(Fixes and patches a lot of graphics related issues and injections, requires Lilu\)
       * _\[optional, for desktop, **CRUCIAL, for laptops**\]_ _**VoodooPS2Controller**_ \(PS2 drivers, needed for laptops\)
         * Note that there is an `acidanthera` fork, **do not use it**. Its support is quite limited to some trackpads and it might outright not work, use the other release. 
     * _**Network Kexts** \(**mandatory**\)_
       * **AppleIntele1000** for some old cards
       * [**HoRNDIS**](https://github.com/midi1996/JBOG/blob/master/Extra/HoRNDIS.kext.zip?raw=true) _ONLY IF YOU NEED NETWORK CONNECTION USING ANDROID_, use the link in the name
       * **IntelMausiEthernet** for most Intel NICs
       * **AtherosE2200Ethernet** for some Atheros/QualcommAtheros/Killer\(some\) NICs
       * **BCM5722D** for Broadcom BCM5722 NetXtreme and NetLink family
       * **BCM57xx** _\(other than 5722\)_ get FakePCIID zip folder, and use FakePCIID + FakePCIID\_BCM57XX\_as\_BCM57765
       * **RealtekRTL8100** for 10/100 Realtek Cards \(Realtek FE\)
       * **RealtekRTL8111** for Gigabit Realtek Cards \(Realtek GbE\)
       * \*\*\*\*[**SmallTreeIntel211**](https://cdn.discordapp.com/attachments/390417931659378688/556912824228773888/SmallTree-Intel-211-AT-PCIe-GBE.kext.zip) for some Intel ethernet chipsets \(use the link in the name\)
         * _**Note:** if you're not sure, LOOK FOR YOUR LAN CHIPSET AND CHECK THIS AGAIN._ 
       * _\[Exceptionally for WiFi-only users\]_ For your compatible WiFi Card:
         * USB Wifi Users: **NO \[won't work while the install, look for the driver after the install**
         * for Broadcom based chips _\(DW1550-DW1560-DW1830...\)_: AirPortBrcmFixUp
         * for Atheros based chips _\(AR5B95/195/97/197, based on AR9280/AR9285 SoC\)_:
           * go to [https://github.com/RehabMan/HP-ProBook-4x30s-DSDT-Patch/archive/master.zip](https://github.com/RehabMan/HP-ProBook-4x30s-DSDT-Patch/archive/master.zip)
           * extract the zip
           * explore to `kexts`
           * get `ProbookAtheros.kext`
         * for QComAtheros: _NOPE_ - Change it \(check AR95/4XX below\)
         * for Intel: _NOPE_ - Change it
         * for Atheros based on _AR95XX-AR94XX_: ATH9KFixUp with proper boot argument options seen in the original github [repo](https://github.com/chunnann/ATH9KFixup) or rehabman's [fork](https://github.com/RehabMan/ATH9KFixup) \(ignore the fact that you need to install it under /S/L/E\).
5. Extract every zip
   * Note: a kexts is a macOS driver, and it's in a form of a `a_folder_name.kext`, on windows it will show as a folder, on macOS it will show as a file.
6. now copy VirtualSMC.**kext** \(or FakeSMC.kext\) - Lilu.kext - WhateverGreen.kext - USBInjectAll.kext - AppleALC.kext - \[your\_ethernet\_driver\].kext \(and any extra kext you needed from above\) and put it in _**CLOVER \(USB\) &gt; EFI &gt; CLOVER &gt; kexts &gt; Other**_. _\[skip the sensor kexts, they may cause kernel panics, aka KP. For VirtualSMC, copy over SMCProcessor if you want to, leave the rest out\]_
   1. _Basically what you should end up with:_  `- AppleALC.kext  - Lilu.kext  - USBInjectAll.kext  - VirtualSMC.kext  - VoodooPS2Controller.kext (laptop users)  - WhateverGreen.kext  - <SomeEthernetOrWifiDriver>.kext`
7. Now you're mostly done with Clover and macOS installer preparation.

**Extra note for X299 people:**

You may need to add `GenericUSBXHCI` kext from the kext repository above, along VoodooTSCSync if you can't enable Core Syncing in your firmware, more on that [here](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/gathering-kexts#usb) \[USB\] and [here](https://www.tonymacx86.com/threads/how-to-build-your-own-imac-pro-successful-build-extended-guide.229353/) \[Go to the BIOS section, as it's the most important part of that whole talk\].

## Important

You must check _**\[MUST READ\] Config.plist Basics**_ section **before** getting to \[part 2\].

