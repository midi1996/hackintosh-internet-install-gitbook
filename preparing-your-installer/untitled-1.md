---
description: Step 1
---

# Downloading the Recovery HD image

After cloning/downloading `gibMacOS`, you'll have to open the the folder where it is \(or `cd` to it if you clone it with git\). If you're using macOS or Linux, you should know how to `cd` to it through your preferred Terminal emulator. If you're a **Windows** user, follow these instructions to get to the command line inside the folder.

* Open Command Prompt/PowerShell and `cd` to where it is.
* OR open the folder where it is, press `Files` &gt; `Open Windows PowerShell`

![Windows Explorer File Menu](../gitbook/assets/image%20%281%29.png)

Now that you're inside the command prompt/powershell/terminal, we'll use gibMacOS to download the Recovery HD to be restored:

1. run `python gibMacOS.command -r -v <macos version>` and replace `<macos version>` with your target version:
   * `10.14` or `mojave` for macOS Mojave release
   * `10.13` or `"high sierra"` \(with quotes\) for macOS High Sierra release
2. The software will download the recovery image, meanwhile:
   1. Plug your USB device \(USB 2.0 drive are recommended\)
   2. Backup any data \*from\* it
   3. Format it \(it will be formatted anyways later on\)

Now that's downloaded, let's move on to the USB drive making...

