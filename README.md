# Yoga-Duet-7-13IML05-OpenCore
 macOS on the Lenovo Yoga Duet 7 13IML05 thanks to [Acidanthera's OpenCore bootloader](https://github.com/acidanthera/OpenCorePkg) 

![OpenCore logo](https://github.com/acidanthera/OpenCorePkg/raw/master/Docs/Logos/OpenCore_with_text_Small.png)
  
## Software Specifications
| Software         | Version                            |
| ---------------- | ---------------------------------- |
| Target OS        | Apple macOS 15 Sequoia, 14 Sonoma and 13 Ventura |
| OpenCore         | [MOD-OC v1.0.4](https://github.com/wjz304/OpenCore_NO_ACPI_Build/releases/download/1.0.4_3889c5a/OpenCore-Mod-1.0.4-RELEASE.zip) |
| SMBIOS           | MacBookPro16,2 |
| UEFI Firmware    | ERCN32WW |
| SSD format       | APFS file system, GPT partition table |

## Abstract
The Yoga Duet 7 13IML05 is a nearly perfect Hackintosh tablet. Its 13-inch display with a 16:10 aspect ratio similar to the MacBooks looks amazing, the Yoga Duet 7 sleeps and wakes quickly and the battery life is on par with an Intel MacBook Pro/Air.

The Yoga Duet 7 13IML05 works great as a macOS tablet. It won't entirely replace an iPad or even an Android tablet, but once set up properly, macOS is actually quite a nice tablet OS and almost on par with the Windows tablet experience. All the fancy Trackpad gestures available on macOS work on the Touchscreen as well and are very smooth and reliable. Folding the removable keyboard cover behind the tablet disables the keyboard and trackpad and folding it to the laptop position again re-enables both of them.

> [!TIP]
> I recommend installing `macOS 13 Ventura` rather than the newer `macOS 14 Sonoma` or `macOS 15 Sequoia`. The builtin Intel Wireless chip works almost perfectly with Apple's iServices and Continuity features on Ventura while those features are partially broken at the moment on newer versions of macOS.

> [!TIP]
> To boot from the USB stick containing the macOS installer, power on your Yoga Duet 7 13IML05 and press and hold the `F12 Key` as soon as the Lenovo logo is displayed, then choose the USB stick in the list.

> [!IMPORTANT]
> For macOS to be able to boot on the Yoga Duet 7 13IML05, the `Secure Boot` option  _**must be disabled**_ in the UEFI Settings.

> [!IMPORTANT]
> Please be aware that all `PlatformInfo` and `SMBIOS` information was removed from the OpenCore `config.plist` file. Users will therefore need to generate their own `PlatformInfo` with [CorpNewt's GenSMBIOS tool](https://github.com/corpnewt/GenSMBIOS) before attempting to boot a Yoga Duet 7 13IML05 with this repository's EFI folder.

## Disclaimer
This repository is neither a howto nor an installation manual. Using these files requires at least basic knowledge of [Acidanthera's OpenCore bootloader](https://github.com/acidanthera/OpenCorePkg), ACPI, UEFI and the art of hackintoshing in general. I recommend reading the excellent [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide), as well as all its linked resources.

## Recommendations
I recommend completely erasing the device's SSD by creating a new GPT partition table before attempting to install macOS, as it makes the installation process much easier. You may use any Linux live ISO with a partitioning tool such as `GParted` or `KPartition` to erase the SSD.

`AirportItlwm_Ventura.kext`, `AirportItlwm_Sonoma14.0.kext` and `AirportItlwm_Sonoma14.4.kext` from the [OpenIntelWireless repo](https://github.com/OpenIntelWireless/itlwm) are required to enable the Wifi chip. This EFI will dynamically load the appropriate kext for macOS Ventura or Sonoma depending on the running kernel. No need to manually replace the kext file when updating your version of macOS. 

As the Intel Wifi chip does not yet work with `AirportItlwm.kext` in macOS Sequoia, you'll need to use `Itlwm.kext` and its companion app [HeliPort](https://github.com/OpenIntelWireless/HeliPort/releases) to connect to a Wifi network. You'll find the latest stable `HeliPort.dmg` in the [Tools folder](https://github.com/jlempen/Yoga-Duet-7-13IML05-OpenCore/blob/main/Tools/HeliPort_v1.5.dmg) of this repo. This EFI will dynamically load `Itlwm.kext` instead of `AirportItlwm.kext` when you boot into macOS Sequoia.

Windows and Linux should be detected automagically by the OpenCore boot loader even when installed after macOS.

This repository uses the unofficial OpenCore_NO_ACPI_Build fork of OpenCore by [btwise](https://gitee.com/btwise/OpenCore_NO_ACPI), wich is not endorsed by Acidanthera (the dev team behind OpenCore). The main (and only) difference between this fork and the official OpenCore version is that it allows to prevent ACPI injection (e.g. patches, tables, boot parameters) into other OSes besides macOS.

<details>
  <summary>Computer Specifications</summary>
  
## Computer Specifications
| Device           | Hardware                           |
| ---------------- | ---------------------------------- |
| CPU              | Intel Core i7-10510U (1.8 - 4.9 GHz, Comet Lake) |
| iGPU             | Intel UHD Graphics 620 |
| Audio            | Realtek ALC 287 |
| RAM              | 2x8 GB DDR4 2666 MHz |
| Wifi + Bluetooth | Wifi 6 AX201, Bluetooth 5.0 |
| Storage          | M.2 2242 Samsung PM981 NVMe PCIe 1 TB SSD (unsupported), replaced with a Kioxia BG4 NVMe PCIe 1 TB SSD |
| Left USB-C 3.2 Gen 1 ports | Support Data Transfer, Power Delivery 3.0 and DisplayPort 1.2 |
| Right USB-C 3.2 Gen 1 | Support Data Transfer & Always On |
| USB-A 3.0| |
| Cameras | 5 MPix front and rear cameras |
| Keyboard / Trackpad | |
| Display | 13 inch 16:10, 2160x1350 IPS 450 nits Multitouch |
| Battery | 41 Wh Li-Polymer |
| Accelerometers, gyroscopes, ambient light sensors | |
</details>

<details>
  <summary>What works</summary>
  
## What works
- [x] CPU power management
- [x] CPU SpeedStep
- [x] iGPU with full acceleration (`AAPL,ig-platform-id 0400A53E`, `device-id A53E0000`)
- [x] SSD drive
- [x] Sleep/hibernate and wake
- [x] USB-C ports with hotplug
- [x] WLAN
- [x] Bluetooth
- [x] Front and rear cameras
- [x] Internal stereo speakers (`VoodooHDA.kext` or `AppleALC.kext` with `alcid=11` and alc-verbs)
- [x] Headphones output
- [x] Power and volume keys on the tablet
- [x] Trackpad with native multi-touch gestures
- [x] Touchscreen
- [x] Battery percentage and cycle count
- [x] USB Type-C Power Delivery
</details>

<details>
  <summary>What needs some more work</summary>
  
## What needs some more work
- [ ] MicroSD Card Reader
- [ ] Accelerometers, gyroscope
</details>

<details>
  <summary>What will probably never work</summary>
  
## What will probably never work
- [ ] Internal Microphone
- [ ] IR Camera (Windows Hello)
</details>

<details>
  <summary>UEFI Settings</summary>
  
## UEFI Settings
To enter the UEFI Settings, power on your Yoga Duet 7 13IML05 and press and hold the `F2 Key` as soon as the Lenovo logo is displayed on the screen.

The `Secure Boot` setting ***must be disabled to boot macOS***.

All other settings may remain on their default values and won't prevent macOS from booting, but keep in mind that every disabled device saves power and increases the battery runtime. For example, as the fingerprint reader won't work in macOS, disabling the device in the UEFI Settings is recommended unless you plan on using another operating system on the device as well. 
</details>

<details>
  <summary>Undervolting to reduce heat and improve performance</summary>
  
## Undervolting to reduce heat and improve performance
The `VoltageShift.kext` undervolting tool is already included and enabled in this repository's `Kexts` folder. To be able to launch the `voltageshift` command line tool from anywhere, copy [VoltageShift from the Tools folder](https://github.com/jlempen/Yoga-Duet-7-13IML05-OpenCore/blob/main/Tools/VoltageShift-EFI.zip) to your `/usr/local/bin` folder by entering `sudo cp voltageshift /usr/local/bin/` in the macOS terminal after navigating to the folder where you downloaded and unzipped the `voltageshift` executable file.

Please refer to the instructions found in the [VoltageShift repository](https://github.com/sicreative/VoltageShift), as well as to the excellent howto found in [zearp's repository](https://github.com/zearp/Nucintosh#undervolting).

Once you have found an undervolting configuration that works well on your device, make it permanent by entering the following command in the terminal after navigating to the folder where you downloaded and unzipped the `VoltageShift-EFI.zip` file:
> sudo ./voltageshift buildlaunchd -120 -50 -80 0 0 0 1 28 18 0.002 60

The above undervolting values are only an example and shouldn't be used on your system.
</details>

<details>
  <summary>Disabling CFG Lock to improve Power Management</summary>
  
## Disabling CFG Lock to improve Power Management
1. Boot into OpenCore
2. Press Space to see the list of tools
3. Select `Disable CFG Lock`
4. Press enter
5. Restart

To verify this worked, press Space and select the `Check CFG Lock State` tool -- if it was successful, you'll see:

> This firmware has UNLOCKED MSR 0xE2 register!

Finally, edit `config.plist` and set `Kernel -> Quirks -> AppleCpuPmCfgLock` and `Kernel -> Quirks -> AppleXcpmCfgLock` to `false` and reboot.
</details>

<details>
  <summary>Disabling Overclocking Lock to enable Undervolting</summary>
  
## Disabling Overclocking Lock to enable Undervolting
1. Boot into OpenCore
2. Press Space to see the list of tools
3. Select `Disable Overclocking Lock`
4. Press enter
5. Restart
</details>

<details>
  <summary>Increasing DVMT Gfx Memory to improve iGPU performance</summary>
  
## Increasing DVMT Gfx Memory to improve iGPU performance
1. Boot into OpenCore
2. Press Space to see the list of tools
3. Select `Set DVMT to 64M`
4. Press enter
5. Then select `Set Total GFX Mem to MAX`
6. Press enter
7. Restart

Finally, edit `config.plist` and delete or comment out `framebuffer-fbmem` and `framebuffer-stolenmem` in `Device Properties -> Add -> PciRoot(0x0)/Pci(0x2,0x0)` and restart.
</details>

<details>
  <summary>More information on the modified UEFI Firmware variables</summary>
  
## More information on the modified UEFI Firmware variables 
| VarName | VarOffset | VarStore | From | To |
| ---------------- | -- | -- | --------- | --------- |
| CFG Lock | 0x3E | 0x3 (CpuSetup) | 0x1 (Enabled) | 0x0 (Disabled) |
| Overclocking Lock | 0xDA | 0x3 (CpuSetup) | 0x1 (Enabled) | 0x0 (Disabled) |
| VT-d | 0x10B | 0x2 (SaSetup) | 0x1 (Enabled) | 0x0 (Disabled) |
| DVMT Pre-Allocated | 0x107 | 0x2 (SaSetup) | 0x1 (32M) | 0x2 (64M) |
| DVMT Total Gfx Mem | 0x108 | 0x2 (SaSetup) | 0x2 (256M) | 0x3 (MAX) |

To revert all changes made to the UEFI Firmware variables to their default values, enable the corresponding entries in the `config.plist` file under `Misc` -> `Tools`, restart to the OpenCore menu, press space to see the list of tools and revert the changes by launching the option you wish to revert to its default value:
- `Enable CFG Lock`
- `Enable Overclocking Lock`
- `Set DVMT to default (32M)`
- `Set Total GFX Mem to default (256M)`

Repeat for every UEFI variable you wish to revert to its default value.

***Please be aware that you need to revert any changes made to your `config.plist` file before reverting the UEFI variables to their default values, or macOS won't boot anymore!***
</details>

<details>
  <summary>Enabling native HiDPI display settings in macOS</summary>
  
## Enabling native HiDPI display settings in macOS
On the installed macOS system, the default display resolution is already "Retina", as the native 3000x2000 resolution is scaled down to 1500x1000. To enable a few more native HiDPI settings in the Display Preferences of macOS, download and run the [one-key-hidpi](https://github.com/jlempen/one-key-hidpi) script and select the option `(6) 3000x2000 Display`.
I also recommend downloading and installing [BetterDisplay](https://github.com/waydabber/BetterDisplay) to change and manage the display resolutions on the Yoga Duet 7 13IML05.
</details>

<details>
  <summary>Fixing the audio outputs</summary>

## Fixing the audio outputs
### AppleALC
Using Acidanthera's [AppleALC.kext](https://github.com/acidanthera/AppleALC) to enable the internal speakers and headphones outputs is a nightmare on the Yoga Duet 7. I managed to get it mostly working by sending specific alc-verbs to the ALC287 (alcid=11) codec, but the power amp for the speakers enters some kind of low power state when running on battery after about 20 seconds. It is possible to wake up the amp by sending the same alc-verbs to the codec every 20 seconds, which is ridiculous. The amp doesn't enter low power mode when the laptop is running on the power adapter, though. Moreover, I haven't been able to make the outputs stereo. As I've spent way too much time trying to make this work, I'm giving up on AppleALC on the Yoga Duet 7.

### VoodooHDA
[VoodooHDA](https://github.com/CloverHackyColor/VoodooHDA) is way easier to set up and works just fine. 

Simply download, unzip and run my [VoodooHDA installer](https://github.com/jlempen/Yoga-Duet-7-13IML05-OpenCore/blob/main/Sound%20Fix/VoodooHDA.zip). The installer will first ask for your password, then macOS will popup a notification asking you to allow the installation of a new kernel extension in the `Privacy & Security` pane of the `System Settings`. Then macOS will tell you it needs to reboot to enable the kernel extension. Agree to everything, reboot and you're done! 

If for whatever reason you wish to uninstall the `VoodooHDA` driver, run the installer again, select "Uninstall", then reboot.

As VoodooHDA will not switch automatically between the internal speakers and the headphones jack, you'll have to switch the output manually in the `Sound` pane of the `System Settings` or in the `Audio MIDI Setup` utility.

I wrote a small launch daemon which will take care of that automatically on boot by running [Devon Weller's AudioSwitcher utility](https://github.com/deweller/switchaudio-osx). Simply download, unzip and run my [AudioSwitcher installer](https://github.com/jlempen/Yoga-Duet-7-13IML05-OpenCore/blob/main/Sound%20Fix/AudioSwitcher.zip). Reboot and you're done!

If for whatever reason you wish to uninstall the `AudioSwitcher` utility, run the installer again, select "Uninstall", then reboot.

Both my `VoodooHDA` and `AudioSwitcher` installers will trigger MacOS's quarantine system on launch. This is meant to protect the user from installing unsigned software downloaded from the net. To easily circumvent this protection, simply open a Terminal, enter `xattr -d com.apple.quarantine ` and drag the installer to the Terminal window, then press the "Enter" key. Now you can launch the installer without macOS bitching about the installer being potentially unsafe.
</details>

<details>
  <summary>Rotating the display</summary>
  
## Rotating the display
### Rotating the display with the Display Rotation Menu widget
[Display Rotation Menu](https://www.magesw.com/displayrotation/) by Mage Software is a free app designed to quickly rotate the display between Landscape, Portrait, Landscape Flipped or Portrait Flipped right from a menu bar widget. There's also a handy keyboard shortcut to rotate the display back to Landscape: `CTRL-OPTION-COMMAND-0` (as in "zero").

<img width="282" alt="Display Rotation Menu" src="https://github.com/user-attachments/assets/ac058eda-6a75-4979-9f3c-907a8837f07f">

The first time you rotate the display to a portrait orientation, the resolution is set to 1280x1920, which is way too small for the SGO2's display. You'll need to head over to the `Displays` tab in the `System Settings` and set the only other available resolution, 640x960, which is quite perfect on our Surface Go 2 in portrait orientation.

However, once the display is in portrait orientation, it is very likely that the menu bar will not be able to show the Display Rotation Menu widget anymore because there's not enough space to display every widget in the narrow portrait-mode menu bar. To fix this, hold down the `Command` key while dragging the Display Rotation Menu widget next to the macOS `Control Center` widget. If there is still not enough space to show the widget, you could also change the size of the clock by disabling the `Show date` and/or `Show the day of the week` items or even change the style of the clock to `Analog` in the `Clock Options` of macOS.

[Download Mage Software's Display Rotation Menu v1.5](https://www.magesw.com/displayrotation/DisplayRotationMenu_1.5.zip)

### Rotating the display with BetterTouchTool
[BetterTouchTool](https://folivora.ai/) by folivora.ai is a great, feature packed app that allows you to customize various input devices on your Mac.

With BetterTouchTool, you could define customised Trackpad/Touchscreen gestures to rotate your Surface Go 2's display. 

You could for instance assign a Four-Finger Double-Tap gesture to the Display Rotation Menu widget's keyboard shortcut `CTRL-OPTION-COMMAND-0` (as in "zero") to switch back to landscape orientation. 

Or you could use BetterTouchTool together with jakehilborn's [displayplacer command line tool](https://github.com/jakehilborn/displayplacer) to define Rotate Left and Rotate Right gestures which would execute Async, non blocking Terminal Commands such as `/usr/local/bin/displayplacer degree:0` for landscape orientation and `/usr/local/bin/displayplacer degree:90` for portrait orientation.

[Download BetterTouchTool](https://folivora.ai/releases/BetterTouchTool.zip) by folivora.ai

[Download displayplacer](https://github.com/jakehilborn/displayplacer/releases/tag/v1.4.0) by jakehilborn
</details>

<details>
  <summary>Enabling the On-Screen Keyboard</summary>
  
## Enabling the On-Screen Keyboard
macOS has a very nice On-Screen Keyboard readily available in the Accessibility Settings. To enable it, navigate to `System Settings -> Accessibility -> Accessibility Keyboard -> Enable`. When you minimize the keyboard, it shrinks down to a small button that can be dragged to a convenient location in a corner of the screen. There are plenty of options to configure the keyboard to your liking.

### Enabling the On-Screen Keyboard on the Lock Screen
Navigate to `System Settings -> Lock Screen -> Accessibility Options -> Accessibility Keyboard -> Enable` to enable the On-Screen Keyboard on the Lock Screen.

### Showing/hiding the On-Screen Keyboard with touchscreen gestures
The most convenient way to show/hide the On-Screen Keyboard when using the Surface Go 2 as a tablet is with a touchscreen gesture. Here's how to set this up: navigate to `System Settings -> Accessibility -> Shortcuts` and uncheck every option but `Accessibility Keyboard`. This enables the keyboard shortcut `OPTION-COMMAND-F5` to show/hide the On-Screen Keyboard. Now use [BetterTouchTool](https://folivora.ai/) to assign this keyboard shortcut to a Trackpad/Touchscreen gesture. 
</details>

<details>
  <summary>Fixing broken Apple Messages and FaceTime</summary>
  
## Fixing broken Apple Messages and FaceTime
To fix issues with Apple Messages and FaceTime related to the [Intel Wireless driver](https://github.com/OpenIntelWireless/itlwm) on macOS Sonoma and Sequoia, disable all `AirportItlwm-***.kext` entries under `Kernel -> Add` in your `config.plist` file and use the [itlwm_v2.3.0_stable.kext.zip](https://github.com/OpenIntelWireless/itlwm/releases/download/v2.3.0/itlwm_v2.3.0_stable.kext.zip) and its companion app [HeliPort](https://github.com/OpenIntelWireless/HeliPort/releases/download/v1.5.0/HeliPort.dmg) instead.
The latest version 2.3.0 of `itlwm.kext` is already included in the Kext folder and `config.plist` file.

In addition to the above, to enable `itlwm.kext` under macOS Ventura and macOS Sonoma, you need to delete any text (i.e. `24.0.0` and `24.99.99` respectively) in the `MinKernel` and `MaxKernel` fields under `Kernel -> Add -> itlwm.kext` in your `config.plist` file.
</details>

<details>
  <summary>Related repositories</summary>
  
## Related repositories
* https://github.com/jlempen/Yoga-Duet-7-13IML05-OpenCore
</details>
