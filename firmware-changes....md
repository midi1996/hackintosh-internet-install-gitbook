# Firmware changes...

Now depending on your computer motherboard make sure of:

* XHCI Handoff: Enabled \(if applicable, some may not have this option\)
* OS Type: Other \(if applicable\)
* Secure Boot: Disabled \(refer to google or your motherboard's manual on how to disable it\)
* Legacy/CSM support: Disabled \[if you see a garbled screen, enable this\] If you have an Nvidia GPU:
* NO DISPLAY is connected to the motherboard's DP/HDMI/VGA/DVI ports
* under System Agent \(SA\) \[or some other menu\], Graphics Settings, Main Display: PEG or PCIE
* Disable anything like: Hybrid Graphics, Dual Graphics, DVMT size ... that are related to Intel GPU
* If you have Intel GPU:
  * make sure your DVMT pre-alloc \(not to be confused with DVMT size\), is set to 64MB \(or higher, 64 is enough\), DMVT size/apertures/whatever, to the max.

For laptops:

* make sure you have Secure Boot Disabled
* TMP/Security Chips/Security modules Disabled \(if possible, it may be enabled later on\)
* DVMT-prealloc to 64MB \(if possible, not to be confused with DVMT size/aperture/whatever, however it may be seen as Graphics Memory\)
* move from RAID/IDE to AHCI \(if applicable\)
* Disable the external GPU \(if possible\)
* Enable USB Legacy support \(on some cases\)
* Disable fast boot
* Disable LAN/WLAN/WWAN boot/wake
* Disable USB wake

