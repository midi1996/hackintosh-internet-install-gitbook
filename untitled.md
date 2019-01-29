# Downloading the Recovery HD image...

After cloning/downloading `gibMacOS` \(that will be refereed as gOS later on\), you'll have to open the the folder where it is \(or `cd` to it if you clone it with git\). If you're using macOS or Linux, you should know how to `cd` to it through your preferred Terminal emulator. If you're a **Windows** user, follow these instructions to get to the command line inside the folder.

* Open Command Prompt/PowerShell and `cd` to where it is.
* OR open the folder where it is, press `Files` &gt; `Open Windows PowerShell`

![Windows Explorer File Menu](.gitbook/assets/image%20%281%29.png)

Now that you're inside the command prompt/powershell/terminal, we'll use gibMacOS to download the Recovery HD to be restored:

1. run `python gibMacOS.command -r -v <macos version>` and replace `<macos version>` with your target version:
   1. `10.14` or `mojave` for macOS Mojave release
   2. `10.13` or `"high sierra"` \(with quotes\) for macOS High Sierra release
   3. `10.11` or `"el capitan"` \(with quotes\) for OS X El Capitan release
   4. `10.10` or `yosemite` for OS X Yosemite release
   5. `10.9` or `mavericks` for OS X Mavericks release
   6. You may be asking where is Sierra, well ever since Sierra, apple started removing links from the AppStore to that release and thus from the release catalog, you can manually download from [here](http://swcdn.apple.com/content/downloads/01/53/031-86778/pnekzincp6rkf5iu91onj1bm5mw1gotnwg/RecoveryHDUpdate.pkg). \(thanks cvad for providing backup links in his BDU tool\) I think the same will happen to High Sierra and Mojave, ¯\\_\(ツ\)\_/¯.
2. The software will download the recovery image, meanwhile:
   1. Plug your USB device \(USB 2.0 drive are recommended\)
   2. Backup any data \*from\* it
   3. Format it \(it will be formatted anyways later on\)

Now that's downloaded, let's move on to the USB drive making...

