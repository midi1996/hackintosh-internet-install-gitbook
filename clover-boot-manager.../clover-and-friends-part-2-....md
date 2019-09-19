---
description: 'Step 3: Configuring Clover'
---

# Clover and friends \[part 2\]

Now your USB has clover and a macOS recovery. For the second part we will create a config.plist.

There are two section: **LAPTOPS** and **DESKTOPS**. Pick the one for you setup.

## For lappies:

Got to [Rehabman config.plist repository](https://github.com/RehabMan/OS-X-Clover-Laptop-Config) and get a fitting config from the list to your hardware configuration. \[Hint: Check your intel GPU, if you dont have one, check the intel GPU that _should_ be with your CPU\]

1. Download **ProperTree**: [https://github.com/corpnewt/ProperTree](https://github.com/corpnewt/ProperTree)
   * Use this to edit any plist file 👌, and it's crossplatform
2. Open ProperTree.cmd
3. File &gt; Open &gt; path to the chosen config.plist
4. Boot\Arguments:
   * add `-v debug=0x100 keepsyms=1`

Then, File &gt; Save the file, rename and copy the resulting plist file to `config.plist` and paste it in CLOVER \(partition\)&gt; EFI &gt; CLOVER and replace the one already there.

{% hint style="info" %}
### Tips:

* _**In case**_ you have a USB3.0 drive, add `-uia_exclude_hs` to the `Boot > Arguments`, this will disable your HS ports from your USB3.0 ports \(basically, you will not be able to use USB2.0 devices on those ports\)
  * However, **if you're using a USB mouse/keyboard** you **must not** use that argument, instead, fetch for a USB2.0 cable extender or a USB2.0 _drive_ and boot from it.
* _**In case**_ you need USB2.0 ports for mouse/keyboard \(for people with I2C touchpads or broken keyboards\) and only have USB3.0 drive, either: 
  * Use a USB2.0 cable extender \(this will force the USB3.0 flash drive to be on USB2.0 mode\)
  * Look for a USB2.0 drive
* For people with **Broadwell \(5th Gen Intel CPUs\) and older,** you have two sets of USB Controllers: _EHCI_ and _XHCI_. You will only use **XHCI** controller since the **EHCI** controller will be disabled. How should you know which port is connected to which? Look for USB3.0 ports \(named SS USB, has 4+5 sets of pins or is blue, and probably have a SS logo/name\) and plug your USB there.
{% endhint %}

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
18. \[If you're using intel GPU\] Download **ProperTree**: [https://github.com/corpnewt/ProperTree](https://github.com/corpnewt/ProperTree)
    * Use this to edit any plist file 👌, and it's crossplatform 
    * \[If you're using intel GPU\] Open ProperTree.bat \(or run `python ProperTree.command`\)
      1. File &gt; Open &gt; config.plist \(the one you downloaded\)
      2. Expand `Devices`
         1. Add Child \(press + key on expanded `Devices`, or right click &gt; add child\)
            * Key: Properties
            * Type: Dictionary
         2. Add Child under Properties \(press + key\)
            * Key: `PciRoot(0x0)/Pci(0x2,0x0)`
            * Type: Dictionary
         3. Add Child under PciRoot\(0x0\)/Pci\(0x2,0x0\) \(press + key\)
            * Key: `AAPL,ig-platform-id`
            * Type: Data
            * Value: _\[Check these links\]_
              * For [IvyBidge](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/config.plist-per-hardware/ivy-bridge#properties)
              * For [Haswell ](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/config.plist-per-hardware/haswell#properties)\*
              * For [Skylake ](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/config.plist-per-hardware/skylake#properties)\* 
              * For [KabyLake ](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/config.plist-per-hardware/kaby-lake#properties)\*
              * For [CoffeeLake ](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/config.plist-per-hardware/coffee-lake#properties)\*
                * \(\*\) For some of these setups, you'll be also invited to add other values next to `AAPL,ig-platform-id`, just highlight the entry in the config and press `+` key to add a sibling, and add all the data you need \(examples of the extra information: `device-id`, `framebuffer-patch*` for framebuffer entries.
                * What you need to do is read that explanation and check the sections above it to fill in the blank in your config \(device-id, AAPL, ig-platform-id, and so on\). You'll have to use some brain power \(not really\) to get that.
            * Check the screenshot bellow to see how it should look like
      3. File &gt; Save As &gt; path/to/CLOVER/partition/EFI/CLOVER/ &gt; Name: `config.plist`
19. Rename and copy the resulting plist file and paste it in CLOVER \(partition\)&gt; EFI &gt; CLOVER and replace the one already there \(you have already done this if you followed the intel GPU section\).

![Screenshot of ProperTree in action.](../.gitbook/assets/image%20%285%29.png)

**NOTE**: to understand any of these options you **must** read this guide: [https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/). This guide gathers information and explanations for most of the hackintoshing process. Use it as an information base and guide if needed.  
  
This can be also done with CCE.

