---
description: 'Step 1: Prepare Clover'
---

# Clover and friends \[part 1\]

For this part of the guide you'll install and prepare clover for the ramble.

{% hint style="warning" %}
There is an issue with the latest Clover releases where the network kexts dont get injected into macOS Recovery/Installer environments. The latest known good Clover that did inject them properly is **r5092**. Please use this release then update it afterwards.

Some users have reported **r5103** had this issue fixed.
{% endhint %}

#### Install Clover

1. For Windows, you already did with the tool before, skip to _Prepare Clover_
2. For Linux or macOS:
   1. Download ~~the latest~~ Clover LZMA [r5092](https://github.com/Dids/clover-builder/releases/tag/v2.5k_r5092) [~~(Clover LZMA latest)~~](https://github.com/Dids/clover-builder/releases/latest) from Dids' repo \(thanks bb ❤️\)
   2. For linux, use p7zip or file roller or whatever you use, to extract the LZMA and TAR inside it until you get the iso
   3. For macOS, use `keka` \(google it\) or `The Unarchiver` \(AppStore, Free\) to extract the LZMA and TAR inside it until you get to the iso
3. Mount the ISO
   1. On linux, use `mount` command with `-o loop,ro` or with `gnome-disks` by right-clicking the iso &gt; mount image 
   2. On macOS, double click
4. Copy the EFI folder from the ISO to the CLOVER USB
   1. you should have CLOVER \(USB\) &gt; EFI \(folder\) &gt; CLOVER & BOOT

5.  _**Note for macOS users** who want to use **legacy** on the destination machine:_
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

#### Prepare Clover folder

1. Open CLOVER &gt; EFI &gt; CLOVER
   1. delete `doc` and `OEM` \(remove if any found\)
   2. Make a folder named `ACPI` with subfolders: `origin` `patched` `WINDOWS` \(case sensitive\)
   
2. Clover Drivers \(EFI drivers, not to be confused with kexts, which are macOS drivers\)
   1. for UEFI users:
      * Open `drivers > UEFI`, _delete_ everything, **BUT** _ApfsDriverLoader_  _- OcQuirks_ _- FwRuntimeServices_ _- HFSPlus_
       * (If _AptioMemoryFix_ or _HFSPlus_ are missing, copy them from `drivers > off`)
   2. for Legacy users:
      * go to `drivers > BIOS`, keep it as it is.
       * (If _AptioMemoryFix_ or _HFSPlus_ are missing, copy them from `drivers > off`)
   
3. Delete now:
   1. UEFI users, delete:
      1. `drivers > BIOS`
   2. Legacy users, delete:
      1. `drivers > UEFI`

#### Prepare macOS Kexts

Go to kexts &gt; Other

  * Go to [Goldfish64's Kext Repo](http://kexts.goldfish64.com/)

  * Download these: [Note: Explore each folder and you'll find a Zip file, get that Zip file, not the whole folder]

**NECESSARY KEXTS**

| Kext | Mandatory | Description |  
| :--- | :--- | :--- |  
| Lilu | ✅ | An open source kernel extension bringing a platform for arbitrary kext, library, and program patching throughout the system for macOS. |  
| VirtualSMC | ✅ Better with Lilu | Advanced Apple SMC emulator in the kernel. |  
| WhateverGreen | ✅ Requires Lilu | Various patches necessary for certain ATI/AMD/Intel/Nvidia GPUs |  
| USBInjectAll | ✅ (may get deprecated soon™️) | Kext to inject all USB ports for the installed Intel EHCI/XHCI chipset automatically. |  
| VoodooPS2Controller | ✅ for laptops and desktops with PS2 peripherals | Enables PS/2 Support on macOS. |  

**NETWORK KEXT**

| Wired Network Kext | Devices to use with |  
| :--- | :--- |  
| AppleIntele1000 | Old Intel Cards (IntelMausi should be better) |  
| HoRNDIS | Android RNDIS Tethering. [Pre-compiled kext only](https://github.com/midi1996/JBOG/blob/master/Extra/HoRNDIS.kext.zip?raw=true) and [official website](https://joshuawise.com/horndis) to get the latest release whenever possible. |  
| IntelMausi | For most Intel NICs |  
| AtherosE2200Ethernet | For some Atheros/QualcommAtheros/Killer\(some\) NICs |  
| BCM5722D | For Broadcom BCM5722 NetXtreme and NetLink family |  
| BCM57xx | _\(other than 5722\)_ get FakePCIID zip folder, and use FakePCIID + FakePCIID\_BCM57XX\_as\_BCM57765 |  
| RealtekRTL8100 | For 10/100 Realtek Cards \(Realtek FE\) |  
| RealtekRTL8111 | For Gigabit Realtek Cards \(Realtek GbE\) |  
| SmallTreeIntel211 | for some Intel ethernet chipsets, [link for the binary](https://cdn.discordapp.com/attachments/390417931659378688/556912824228773888/SmallTree-Intel-211-AT-PCIe-GBE.kext.zip). |  
   
| Wireless Kext | Devices to use with |  
| :--- | :--- |  
| *none* | USB Wifi Users: **NO [won't work while the install, look for the driver after the install]** |  
| AirPortBrcmFixUp | For Broadcom chipsets: BCM4360, BCM4352, BCM4350 (found in DW1830, DW1560, DW1820A -- not recommended card --, Apple BRCM94360CS2/2CS/CD) |  
| ProbookAtheros.kext | for Atheros based chips _\(AR5B95/195/97/197, based on AR9280/AR9285 SoC\)_:<br/>           * go to [HP Probook 4x30 DSDT Patch](https://github.com/RehabMan/HP-ProBook-4x30s-DSDT-Patch/archive/master.zip)<br/>           * extract the zip<br/>           * explore to `kexts`<br/>           * get `ProbookAtheros.kext`<br />**The kext will not work on Catalina/Mojave anymore, AppleAtheros40 kext has been deprecated in 10.14+ and will need special workaround that will not work in a recovery environment.** |  
| *none* | Intel: _NOPE_ - Change it |  
| *none* | QComAtheros: _NOPE_ - Change it |  
| ATH9KFixUp | for Atheros based on _AR95XX-AR94XX_: ATH9KFixUp with proper boot argument options seen in the original github [repo](https://github.com/chunnann/ATH9KFixup) or rehabman's [fork](https://github.com/RehabMan/ATH9KFixup) \(DO NOT install it in /S/L/E, just use Clover's folder).<br />**The kext will not work on Catalina/Mojave anymore, AppleAtheros40 kext has been deprecated in 10.14+ and will need special workaround that will not work in a recovery environment.** |  

1. Extract every zip

   * Note: a kexts is a macOS driver, and it's in a form of a `a_folder_name.kext`, on windows it will show as a folder, on macOS it will show as a file.
   
2. Now copy: VirtualSMC.kext - Lilu.kext - WhateverGreen.kext - USBInjectAll.kext - AppleALC.kext - \[your\_ethernet\_driver\].kext \(and any extra kext you needed from above\) and put it in _**CLOVER \(USB\) &gt; EFI &gt; CLOVER &gt; kexts &gt; Other**_. _\[skip the sensor kexts, they may cause kernel panics, aka KP. For VirtualSMC, copy over SMCProcessor if you want to, leave the rest out\]_
   
4. **Basically what you should end up with**:

   - Lilu.kext
   - VirtualSMC.kext
   - WhateverGreen.kext
   - USBInjectAll.kext
   - VoodooPS2Controller.kext (for laptops and PS2 users)
   - **a network driver**.kext
   - **other kext if needed**.kext

5. Now you're mostly done with Clover and macOS installer preparation.

**Extra note for X299 people:**

You may need to add `GenericUSBXHCI` kext from the kext repository above, along VoodooTSCSync if you can't enable Core Syncing in your firmware, more on that [here](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/gathering-kexts#usb) \[USB\] and [here](https://www.tonymacx86.com/threads/how-to-build-your-own-imac-pro-successful-build-extended-guide.229353/) \[Go to the BIOS section, as it's the most important part of that whole talk\].

## Important

You must check _**\[MUST READ\] Config.plist Basics**_ section **before** getting to \[part 2\].

