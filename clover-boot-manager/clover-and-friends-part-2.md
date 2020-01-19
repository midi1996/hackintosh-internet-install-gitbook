---
description: 'Step 3: Configuring Clover'
---

# Clover and friends \[part 2\]

Now your USB has clover and a macOS recovery. For the second part we will create a config.plist.

There are two section: **LAPTOPS** and **DESKTOPS**. Pick the one for you setup.

## For lappies:

Follow [**Fewtarius's Laptop Guide**](https://fewtarius.gitbook.io/laptopguide/prepare-install-macos/macos-memory-allocation), this part will start from the beginning with Clover drivers and stuff, do follow it properly.

## For deskies:

**NOTE:** _**This is a brief and limited guiding to get booting and running. This is not meant to be the final config. Check the link at the end if this part to understand what's going on.**_

1. Open [CCE](http://cloudclovereditor.altervista.org/) \(it's a link\): Cloud Clover Editor: an open-source web-based clover editor, and better than the app in some ways.
2. Create a new config
3. Under ACPI:
   * if you have a _**4rd Gen intel Core or newer**_: select _Generate Plugin Type_ under SSDT
   * if you have a _**3nd Gen intel Core or older**_: You **must** make a CPUPM SSDT _after_ the install \(check the Laptop PM link in Posty Posty\)
   * \[May cause issues, select them after the install\] Select `FixRTC` `FixTMR` `FixIPIC` under `DSDT` &gt; `Fixes`
   * In the bottom right of that section: `Fix ACPI Tables Headers` and `Auto Merge SSDTs`
   * \[For people with iGPU\] Select the Blue Globe under Patches and choose GFX0 to IGPU
4. Under Boot:
   * Boot Arguments \(the big zone\): `-v debug=0x100 nv_disable=1 kext-dev-mode=1 dart=0` \(these are generic boot args for: Verbose `-v debug=0x100` - Disabling Nvidia drivers from loading `nv_disable=1`and works only on 10.11 and prior - unsigned kexts allow in for 10.10.x `kext-dev-mode-1` - disable VT-d on macOS `dart=0`\)
   * XPM detection: NO
5. Under Devices:
   * USB: Inject - Add Clock ID - Fix Ownership
   * Audio: Inject : 1 \(type it inside Layout ID\)
   * Properties: follow the platform that suites you if you're using an intel GPU:
      * For [IvyBidge](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/config.plist-per-hardware/ivy-bridge#properties)
      * For [Haswell ](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/config.plist-per-hardware/haswell#properties)\*
      * For [Skylake ](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/config.plist-per-hardware/skylake#properties)\* 
      * For [KabyLake ](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/config.plist-per-hardware/kaby-lake#properties)\*
      * For [CoffeeLake ](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/config.plist-per-hardware/coffee-lake#properties)\*
6. Under GUI:
   * Scan Options: Custom - Scan Entries - Scan Tools - Scan Kernel: Disabled - Scan Legacy: Disabled.
   * \[optional\] Mouse: Enabled
   * \[optional\] Screen Resolution - Language - Theme \(I recommend Embedded Theme Type: Dark\)
7. Under Graphics:
   * Keep it CLEAN, nothing here.
8. Under Kernel And Kext Patches:
   * Apple RTC - Kernel PM
   * Select the Blue Globe in Kernel Patches: add all patches
   * \[optional\] IF YOU NEED IT, choose a FakeCPU ID \(only if you're using an older macOS version on a new hardware system, eg: running Sierra on a CoffeeLake system, choose either KabyLake if it's 10.12.6, or SkyLake id =&lt;10.12.6\)
   * \[ONLY FOR IVYBRIDGE AND SANDYBRIDGE\] AppleICPUPM \(this helps with AppleIntelCPUPM kernel panic, it patches it for locked MSR 0xE2\)
9. Under RT Variables:
   * Booter Config: 0x28
   * Csr Active Config: 0x3E7 \(some may use 0x67 for pre-High Sierra\)
10. Under SMBIOS:
    * Coffee Lake - _iMac18,2/18,3 or iMac19,x_
      * Use _iMac18,1_ if you are using the iGPU only
    * Kabylake - _iMac18,2/18,3_
    * Skylake - _iMac17,1_
    * Broadwell - _iMac16,1_ \(rarely used, if ever\)
    * Haswell Refresh \(Devil's Canyon\) - _iMac15,1_
    * Haswell With NVIDIA GPU - _iMac14,2_
    * Haswell With iGPU - _iMac14,1_
    * Ivy Bridge - _iMac13,2_
    * Sandy Bridge - _iMac12,2_ \(although recently I've had better success with _iMac13,2_\)
    * X79/X99 - _MacPro6,1_
    * X299 - _iMacPro1,1_
      * \(~source: Vanilla guide from /u/corpnewt, edited\)
11. Under System Parameters:
    * Inject Kexts: Yes
12. Under Boot Graphics:
    * \[optional\] for high resolution panel users \(3k~4k, 1440p...\) change UI Scale to 2
13. Select the two squares on the top right corner
14. Select Download
15. name: `config` \(no extension\)
16. Download
17. Save
18. Rename and copy the resulting plist file and paste it in CLOVER \(partition\)&gt; EFI &gt; CLOVER and replace the one already there.

**NOTE**: to understand any of these options you **must** read this guide: [https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/). This guide gathers information and explanations for most of the hackintoshing process. Use it as an information base and guide if needed.  
  
This can be also done with CCE.

