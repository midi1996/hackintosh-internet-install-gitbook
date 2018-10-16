# Clover and friends \[part 2\]...

Now your USB has clover and a macOS recovery. For the second part we will create a config.plist.

There are two section: **LAPTOPS** and **DESKTOPS**. Pick the one for you setup.

## For lappies:

Got to [Rehabman config.plist repository](https://github.com/RehabMan/OS-X-Clover-Laptop-Config) and get a fitting config from the list to your hardware configuration.

Then open the file with a text editor \(check above\), and add these with the boot arguments: `-v debug=0x100`. Boot arguments entry:

```markup
	...
<key>Boot</key>
<dict>
	...
	<key>Arguments</key>
	<string>[You'll find some arguments here already, add the above to them]</string>
	...
```

Save the file, rename and copy the resulting plist file and paste it in CLOVER \(partition\)&gt; EFI &gt; CLOVER and replace the one already there.

_**In case**_ you have a USB3.0 drive look for:

```markup
<key>KextsToPatch</key>
<array>
    [paste things here]
</array>
```

and replace `[paste things here]` with this: _\[Note: `[paste things here]` doesnt exist in the configuration file\]_

_**For 10.14/Mojave**_

```markup
			<dict>
				<key>Comment</key>
				<string>disable port limit in XHCI kext (credit PMHeart)</string>
				<key>MatchOS</key>
				<string>10.14.x</string>
				<key>Name</key>
				<string>com.apple.driver.usb.AppleUSBXHCI</string>
				<key>Find</key>
				<data>g/sPD4MDBQAA</data>
				<key>Replace</key>
				<data>g/sPkJCQkJCQ</data>
			</dict>
```

_**For 10.13/High Sierra \(10.13.6\)**_

```markup
			<dict>
				<key>Comment</key>
				<string>disable port limit in XHCI kext (credit RehabMan, based prior PMHeart patch)</string>
				<key>MatchOS</key>
				<string>10.13.6</string>
				<key>Name</key>
				<string>com.apple.driver.usb.AppleUSBXHCI</string>
				<key>Find</key>
				<data>g32IDw+DpwQAAA==</data>
				<key>Replace</key>
				<data>g32ID5CQkJCQkA==</data>
			</dict>
```

_**For 10.12/Sierra**_

```markup
			<dict>
				<key>Comment</key>
				<string>change 15 port limit to 26 in XHCI kext</string>
				<key>MatchOS</key>
				<string>10.12.x</string>
				<key>Name</key>
				<string>com.apple.driver.usb.AppleUSBXHCIPCI</string>
				<key>Find</key>
				<data>g710////EA==</data>
				<key>Replace</key>
				<data>g710////Gw==</data>
			</dict>
```

_**For 10.11/El Capitan**_

```markup
			<dict>
				<key>Comment</key>
				<string>change 15 port limit to 26 in XHCI kext</string>
				<key>MatchOS</key>
				<string>10.11.x</string>
				<key>Name</key>
				<string>com.apple.driver.usb.AppleUSBXHCIPCI</string>
				<key>Find</key>
				<data>g72M/v//EA==</data>
				<key>Replace</key>
				<data>g72M/v//Gw==</data>
			</dict>
```

## For deskies:

1. Open [CCC](http://cloudclovereditor.altervista.org/)\[link\] : Cloud Clover Configurator: an open-source web-based clover configurator, and better than the app in some ways.
2. Create a new config
3. Under ACPI:
   * if you have a _**3rd Gen intel Core or newer**_: select _Generate Plugin Type_ under SSDT
   * if you have a _**2nd Gen intel Core or older**_: select _Generate P-States and C-States_ \[Note: you'll need to make an SSDT after the installing macOS for better PM\]
   * Select `FixRTC` `FixTMR` `FixIPIC` under `DSDT` &gt; `Fixes`
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
   * If you have Intel HD device, you can try Inject &gt; Intel, if it doesn't work, use a bogus fakeID back under Device sections, under Intel GFX = 0x12345678. Usually intel works OOB on many systems.
   * If you have AMD/Nvidia, **DO NOT TICK INJECT** \(unless you're on Kepler or older on Nvidia, no idea about AMD\).
8. Under Kernel And Kext Patches:
   * Apple RTC - Kernel PM
   * Select the Blue Globe in Kernel Patches: add both patches
   * \[optional\] IF YOU NEED IT, choose a FakeCPU ID \(only if you're using an older macOS version on a new hardware system, eg: running Sierra on a CoffeeLake system, choose either KabyLake if it's 10.12.6, or SkyLake id =&lt;10.12.6\)
9. Under RT Variables:
   * Booter Config: 0x28
   * Csr Active Config: 0x3E7 \(some may use 0x67 for pre-High Sierra\)
10. Under SMBIOS:
    * Coffee Lake - _iMac18,2/18,3_
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

