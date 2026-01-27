# Yoga-Duet-7-13IML05-OpenCore
 macOS on the Lenovo Yoga Duet 7 13IML05 thanks to [Acidanthera's OpenCore bootloader](https://github.com/acidanthera/OpenCorePkg) 

![OpenCore logo](https://github.com/acidanthera/OpenCorePkg/raw/master/Docs/Logos/OpenCore_with_text_Small.png)

> [!WARNING]
> ### Installing or upgrading to macOS Tahoe
>
> As of January 2026, the Type Cover works in the installer and on the installed system, but it causes a kernel panic on restart, shutdown and when detaching and attaching the Type Cover to the Yoga Duet 7. While working on a fix, I recommend using an external USB keyboard and mouse to complete the upgrade or install of macOS Tahoe. It is abolutely possible to install or upgrade to Tahoe with the Type Cover attached, but keep in mind that you'll have to forcefully shutdown the Yoga Duet 7 everytime the installer attempts to reboot and hangs due to the kernel panic.
> 
> When  <ins>**upgrading**</ins> your existing installation to macOS Tahoe, you <ins>**MUST**</ins> log out of your `Apple Account` (iCloud) before proceeding with the upgrade. Furthermore make sure you <ins>**DESELECT**</ins> the option to enable `FileVault` disk encryption in the installer and do not sign into your `Apple Account` (iCloud) until the installer is done and you reach the desktop. Failing to do so will eventually encrypt your disk and prevent you from unlocking your disk with your password on restart.
>
> If `FileVault` disk encryption is enabled in your existing installation of macOS and you wish to keep it enabled in macOS Tahoe, then [carefully read the section below](https://github.com/jlempen/Yoga-Duet-7-13IML05-OpenCore/tree/main#fixing-filevault-when-upgrading-to-macos-tahoe) before proceeding with the upgrade.
>
> When doing a <ins>**clean install**</ins> of macOS Tahoe, make sure you <ins>**DESELECT**</ins> the option to enable `FileVault` disk encryption in the installer and do not sign into your `Apple Account` (iCloud) until the installer is done and you reach the desktop. Failing to do so will eventually encrypt your disk and prevent you from unlocking your disk with your password on restart.
>
> If you wish to enable `FileVault` disk encryption in macOS Tahoe, [carefully read the section below](https://github.com/jlempen/Yoga-Duet-7-13IML05-OpenCore/tree/main#fixing-filevault-when-upgrading-to-macos-tahoe).

## Latest News
* (20260127) Added instructions to enable Wifi in the macOS Sequoia and Tahoe installer ([see section below](https://github.com/jlempen/Yoga-Duet-7-13IML05-OpenCore/tree/main#enabling-the-intel-wireless-card-in-the-macos-sequoia-and-tahoe-installer)).
* (20260115) Added the `apfs_aligned.efi` driver to fix `FileVault` when upgrading to macOS Tahoe ([see section below](https://github.com/jlempen/Yoga-Duet-7-13IML05-OpenCore/tree/main#fixing-filevault-when-upgrading-to-macos-tahoe)).
* (20260115) Added resources and instructions to enable `AirportItlwm.kext` on macOS Sequoia and Tahoe ([see section below](https://github.com/jlempen/Yoga-Duet-7-13IML05-OpenCore/tree/main?tab=readme-ov-file#enabling-the-intel-wireless-card-in-macos-sequoia-and-tahoe)).
* (20260115) Fixing audio in macOS Tahoe ([see section below](https://github.com/jlempen/Yoga-Duet-7-13IML05-OpenCore/tree/main#fixing-audio-on-macos-tahoe)).
* (20260115) With the stuff merged today, `macOS Tahoe` installs and runs quite nicely on the Yoga Duet 7. At the moment, restarting or shutting down the system will cause a kernel panic. Likewise when detaching and attaching the Type Cover.
* (20260115) New Bluetooth fixes for `macOS Sequoia` and hopefully `macOS Tahoe` as well.
* (20260115) Recent Linux distros such as `Fedora 43` should now appear in the OpenCore picker thanks to updated `btrfs_x64.efi` and `ext4_x64.efi` filesystem drivers.

## Software Specifications
| Software         | Version                            |
| ---------------- | ---------------------------------- |
| Target OS        | Apple macOS 26 Tahoe, 15 Sequoia, 14 Sonoma and 13 Ventura |
| OpenCore         | [MOD-OC v1.0.7](https://github.com/wjz304/OpenCore_NO_ACPI_Build/releases/download/1.0.7_ffbd7f5/OpenCore-Mod-1.0.7-RELEASE.zip) |
| SMBIOS           | MacBookPro16,2 |
| UEFI Firmware    | ERCN32WW |
| SSD format       | APFS file system, GPT partition table |

## Abstract
The Yoga Duet 7 13IML05 is a nearly perfect Hackintosh tablet. Its 13-inch display with a 16:10 aspect ratio similar to the MacBooks looks amazing, the Yoga Duet 7 sleeps and wakes quickly and the battery life is on par with an Intel MacBook Pro/Air. Only the internal microphone and the native media keys of the keyboard cover are non-functional. The latter may however be replaced by remapping the function keys, [see the section below](https://github.com/jlempen/Yoga-Duet-7-13IML05-OpenCore#remapping-the-media-keys-on-the-keyboard-cover).

The Yoga Duet 7 13IML05 works great as a macOS tablet. It won't entirely replace an iPad or even an Android tablet, but once set up properly, macOS is actually quite a nice tablet OS and almost on par with the Windows tablet experience. All the fancy Trackpad gestures available on macOS work on the Touchscreen as well and are very smooth and reliable. Folding the removable keyboard cover behind the tablet disables the keyboard and trackpad and folding it to the laptop position again re-enables both of them.

> [!TIP]
> As the Type Cover causes a kernel panic on shutdown, reboot and when detaching or attaching it to the Yoga Duet 7, I highly recommend not to upgrade to macOS Tahoe for the time being. macOS Sequoia works great though.

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

`AirportItlwm-Ventura.kext`, `AirportItlwm-Sonoma140.kext`, `AirportItlwm-Sonoma144.kext` and `AirportItlwm-Sequoia-Tahoe.kext` from the [OpenIntelWireless repo](https://github.com/OpenIntelWireless/itlwm) are drivers required to enable the Wifi chip. This EFI will dynamically load the appropriate kext for macOS Ventura, Sonoma, Sequoia or Tahoe depending on the running kernel. No need to manually replace the kext file when updating your version of macOS. 

In macOS Sequoia and Tahoe, you'll need to apply root patches with [Laobamac's OCLP-Mod Patcher](https://github.com/laobamac/OCLP-Mod/releases) once the OS is up and running in order to enable the Intel Wifi chip. [Head over to the instructions.](https://github.com/jlempen/Yoga-Duet-7-13IML05-OpenCore/tree/main?tab=readme-ov-file#enabling-the-intel-wireless-card-in-macos-sequoia-and-tahoe).

the `Itlwm.kext` driver and its companion app [HeliPort](https://github.com/OpenIntelWireless/HeliPort/releases) are included but disabled in this EFI for those who prefer to connect to their Wifi network this way. You'll find the latest stable `HeliPort.dmg` in the [Tools folder](https://github.com/jlempen/Yoga-Duet-7-13IML05-OpenCore/blob/main/Tools/HeliPort.dmg) of this repo.

The macOS installer USB sticks created on Linux or Windows are __online__ installers which require a working Internet connection. To install macOS Sequoia or Tahoe with an online installer, you'll need to disable the `AirportItlwm.kext` Intel Wifi driver and use the `itlwm.kext` Intel Wifi driver instead. To do so, [follow the instructions here](https://github.com/jlempen/Yoga-Duet-7-13IML05-OpenCore/tree/main#enabling-the-intel-wireless-card-in-the-macos-sequoia-and-tahoe-installer). You could also use an USB to Ethernet adapter to connect to your router with a good old Ethernet cable.

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
- [x] iGPU with full acceleration (`AAPL,ig-platform-id 0900A53E`, `device-id 9B3E0000`)
- [x] SSD drive
- [x] Sleep and wake by closing/opening the keyboard/trackpad cover (`hibernatemode 0` and `hibernatemode 3`)
- [x] Sleep by selecting "Sleep" in the Apple Menu and wake by pressing the power button (`hibernatemode 0` and `hibernatemode 3`)
- [x] Hibernate by closing the keyboard/trackpad cover and wake by pressing the power button (`hibernatemode 25`)
- [x] Hibernate by selecting "Sleep" in the Apple Menu and wake by pressing the power button (`hibernatemode 25`)
- [x] Automatic transition from Sleep to Hibernate (`hibernatemode 3`)
- [x] USB-C ports with hotplug
- [x] WLAN
- [x] Bluetooth
- [x] Front and rear cameras
- [x] Internal stereo speakers (`VoodooHDA.kext` or `AppleALC.kext`, [see notes below](https://github.com/jlempen/Yoga-Duet-7-13IML05-OpenCore#fixing-the-audio-outputs))
- [x] Headphones output
- [x] Power and volume keys on the tablet
- [x] Trackpad with native multi-touch gestures
- [x] Touchscreen with native multi-touch gestures
- [x] Battery percentage and cycle count
- [x] USB Type-C Power Delivery
</details>

<details>
  <summary>What needs some more work</summary>
  
## What needs some more work
- [ ] MicroSD Card Reader
- [ ] Native media keys on the keyboard cover
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

All other settings may remain on their default values and won't prevent macOS from booting, but keep in mind that every disabled device saves power and increases the battery runtime.
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
3. Select `Set DVMT Pre-Allocated to 64M`
4. Press enter
5. Then select `Set DVMT Total GFX Mem to MAX`
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
- `Set DVMT Pre-Allocated to default (32M)`
- `Set DVMT Total GFX Mem to default (256M)`

Repeat for every UEFI variable you wish to revert to its default value.

***Please be aware that you need to revert any changes made to your `config.plist` file before reverting the UEFI variables to their default values, or macOS won't boot anymore!***
</details>

<details>
  <summary>Fixing Hibernate Mode 25</summary>
  
## Fixing Hibernate Mode 25
If for whatever reason `Hibernate Mode 25` is not working correctly on your system, you should reset the `Power Management` settings and rebuild the `sleepimage` file. To do so, open the `Terminal` and enter the following commands, then reboot for the changes to take effect:
```
sudo rm /Library/Preferences/com.apple.PowerManagement*
sudo rm /var/vm/sleepimage
sudo pmset hibernatefile /var/vm/sleepimage
```
Once you are back in macOS, restore the default values for your SMBIOS, then reboot once more:
```
sudo pmset restoredefaults
```

It's also a good idea to reset the NVRAM before rebooting into macOS. To do so, press the space bar in the OpenCore picker and use the arrow keys to select `Reset NVRAM`.
</details>

<details>
  <summary>Enabling the Intel Wireless Card in macOS Sequoia and Tahoe</summary>
  
## Enabling the Intel Wireless Card in macOS Sequoia and Tahoe

### Using the AirportItlwm.kext driver and root patching
It is now possible to have Intel WiFi with native Airport features on macOS Sequoia and Tahoe by enabling the `AirportItlwm.kext` driver for macOS Ventura provided by [the OpenIntelWireless project](https://openintelwireless.github.io/). 

`AirportItlwm.kext` uses Apple's IO80211Family. It provides certain Airport features but lacks stability compared with `itlwm.kext` due to the ambiguity of reverse engineering.

To enable the `AirportItlwm.kext` driver, download and install the latest release of [Laobamac's OCLP-Mod Patcher](https://github.com/laobamac/OCLP-Mod/releases).

Launch the OCLP-Mod Patcher, this may take a few seconds. As of January 2026, the tool is only available in Chinese.

Now click on the upper right button to select the Root Patching option:
<img width="712" height="443" alt="Screenshot 2026-01-10 at 00 43 16" src="https://github.com/user-attachments/assets/3af5464a-836b-4f71-a0fc-8ac8bd4af304" />

Then click on the highlighted button or press Enter to start the patching process:
<img width="712" height="443" alt="Screenshot 2026-01-10 at 00 46 51" src="https://github.com/user-attachments/assets/843687df-655f-47ea-bfc8-5b3f06681756" />

Once the patching is done, click on the highlighted button or press Enter to close the tool and restart your computer. Your Intel wireless card should be working now.

> [!IMPORTANT]
> You'll need to repeat the above steps after every macOS update!

### Using the Itlwm.kext driver
`itlwm.kext` uses Apple's IOEthernet rather than IO80211. It is purely based on open-source resources, provides a stabler and faster performance, and the ability to unload on Kernels that use prelined kernel.

To enable Intel WiFi with the `itlwm.kext` driver, open the `Kernel -> Add` tab in your `config.plist` file.

Enable the following kext:
```
itlwm.kext
```

Then disable the following kexts:

```
IOSkywalkFamily.kext
IO80211FamilyLegacy.kext
IO80211FamilyLegacy.kext/Contents/PlugIns/AirPortBrcmNIC.kext
AMFIPass.kext
AirportItlwm-Sequoia-Tahoe.kext
```

Now head over to the `Kernel -> Block` tab and disable the `com.apple.iokit.IOSkywalkFamily` item.

Then head over to the `NVRAM` tab, select the `7C436110-AB2A-4BBB-A880-FE41995C9F82` UUID and remove the `-amfipassbeta` argument from the `boot-args` key.

Save and close the `config.plist` file and reboot your computer.

Download and install the latest `HeliPort` Intel WiFi client for `itlwm` from [the OpenIntelWireless project](https://openintelwireless.github.io/HeliPort/#download).

Add the `HeliPort` client to your login items and hide the macOS WiFi icon from the Menu Bar.
</details>

<details>
  <summary>Enabling the Intel Wireless Card in the macOS Sequoia and Tahoe installer</summary>
  
## Enabling the Intel Wireless Card in the macOS Sequoia and Tahoe installer
To have working Wifi in the installer for macOS Sequoia or Tahoe, you'll need to disable the `AirportItlwm.kext` driver and use the `itlwm.kext` driver instead.

Open the `Kernel -> Add` tab in your `config.plist` file.

Disable the following kexts:

```
IOSkywalkFamily.kext
IO80211FamilyLegacy.kext
IO80211FamilyLegacy.kext/Contents/PlugIns/AirPortBrcmNIC.kext
AMFIPass.kext
AirportItlwm-Sequoia-Tahoe.kext
```

Then enable the following kext:

```
itlwm.kext
```

Save and close the `config.plist` file.

Then open the `info.plist` file inside the `itlwm.kext` and add the name and password for your Wifi network in the `ssid` and `password` fields. Save and close the file and reboot into the macOS installer. Your installer should automagically connect to your Wifi network now.

<img width="930" height="771" alt="Image" src="https://github.com/user-attachments/assets/094a62e7-bb90-4d8b-a92e-57f7c4a4f682" />

Once macOS Sequoia or Tahoe is up and running, you may switch back to the `AirportItlwm.kext` driver by reverting the changes you made above, then [follow my instructions](https://github.com/jlempen/Yoga-Duet-7-13IML05-OpenCore/tree/main#enabling-the-intel-wireless-card-in-macos-sequoia-and-tahoe) to apply the root patches which enable the Intel Wifi chip.
</details>

<details>
  <summary>Fixing audio on macOS Tahoe</summary>
  
## Fixing audio on macOS Tahoe

### By root patching
As Apple removed the `AppleHDA.kext` from macOS Tahoe, [Acidanthera's AppleALC.kext](https://github.com/acidanthera/AppleALC) won't work on macOS Tahoe out of the box and audio is broken. To fix this, simply run [Laobamac's OCLP-Mod Patcher](https://github.com/laobamac/OCLP-Mod/releases) a second time once you have [fixed the Intel wireless card](https://github.com/jlempen/Yoga-Duet-7-13IML05-OpenCore/tree/main?tab=readme-ov-file#enabling-the-intel-wireless-card-in-macos-sequoia-and-tahoe) and Internet is working again. The reason for this is that the OCLP-Mod Patcher needs to download the Kernel Development Kit from Apple's servers in order to reinstall the `AppleHDA.kext` on your macOS Tahoe partition and to do so, it needs a working Internet connection. 

### By installing the VoodooHDA audio driver
Another way to get back the internal speakers and microphone on macOS Tahoe is to install [SergeySlice's VoodooHDA audio driver](https://github.com/CloverHackyColor/VoodooHDA) with [chris1111's convenient VoodooHDA-Tahoe installer](https://github.com/chris1111/VoodooHDA-Tahoe).
Before installing the VoodooHDA driver, you need to disable the `AppleALC.kext` driver in your `config.plist` file under `Kernel -> Add` and reboot your computer.

Then [grab the latest installer](https://github.com/jlempen/Yoga-Duet-7-13IML05-OpenCore/blob/main/Sound%20Fix/VoodooHDA-Tahoe.pkg) from the Tools folder in my repository, launch the installer and follow the instructions.

Once you're back in macOS Tahoe after a reboot, head over to `System Settings -> Sound -> Output & Input` and select the `Output` tab, then select `Speaker (Analog)` as your sound output device.
</details>

<details>
  <summary>Fixing FileVault when upgrading to macOS Tahoe</summary>
  
## Fixing FileVault when upgrading to macOS Tahoe
If `FileVault` disk encryption is enabled during the upgrade or install of macOS Tahoe, the `FileVault` login window will reject your password and the encrypted disk will remain locked after the first reboot. This is due to Tahoe's APFS filesystem driver not supporting the software `FileVault` disk encryption created with previous versions of macOS.

There are two solutions to this rather annoying issue:

### Enabling the older APFS driver from macOS Sequoia
Open the `UEFI -> Drivers` tab in your `config.plist` file and enable the `apfs_aligned.efi` driver.

Then head over to the `UEFI -> APFS` tab and disable the `EnableJumpstart` option.

Save and close the `config.plist` file and restart your computer. The `FileVault` login window should now accept your password and unlock the disk.

### Turning off FileVault in the macOS Recovery Console
Follow the nice instructions [on Jac Timms' website](https://www.ichi.co.uk/blog/removing-file-vault-from-internet-recovery-console).
</details>

<details>
  <summary>Enabling native HiDPI display settings in macOS</summary>
  
## Enabling native HiDPI display settings in macOS
The native resolution of the Yoga Duet 7 is 2160x1350, which represents an aspect ratio of 16:10 like on all Intel MacBooks. However, there has never been a MacBook with this odd resolution, so we have to enable a few more native HiDPI settings in the Display Preferences of macOS. To do so, download and run [the one-key-hidpi script](https://github.com/xzhih/one-key-hidpi) and select the option `(3) 1920x1200 Display`.
I also recommend downloading and installing [BetterDisplay](https://github.com/waydabber/BetterDisplay) to change and manage the display resolutions on the Yoga Duet 7 13IML05.
</details>

<details>
  <summary>Fixing the audio outputs</summary>

## Fixing the audio outputs
### AppleALC
Using Acidanthera's [AppleALC.kext](https://github.com/acidanthera/AppleALC) to enable the internal speakers and headphones outputs is a nightmare on the Yoga Duet 7. I managed to get it mostly working by sending specific alc-verbs to the ALC287 (alcid=11) codec, but the power amp for the speakers enters some kind of low power state when running on battery after about 20 seconds. It is possible to wake up the amp by sending the same alc-verbs to the codec every 20 seconds, which is ridiculous. The amp doesn't enter low power mode when the laptop is running on the power adapter and the headphones output seems to be always active, even on battery power. Moreover, I haven't been able to make the outputs stereo. As I've spent way too much time trying to make this work, I'm giving up on `AppleALC` on the Yoga Duet 7.

For those who want to play around with `AppleALC` on the Yoga Duet 7, you'll have to download and copy the [AppleALC.kext](https://github.com/acidanthera/AppleALC/releases) into your EFI's `Kexts` folder and add it to the `Kernel -> Add` section of your `config.plist` file. Then add the boot arguments `alcid=11` and `alcverbs=1` to the `boot-args` in the `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82` section of your `config.plist` file. Now download and install my [YogaDuet7ALCVerbs launch daemon](https://github.com/jlempen/Yoga-Duet-7-13IML05-OpenCore/blob/main/Sound%20Fix/YogaDuet7ALCVerbs.zip) which will launch on boot to check every 20 seconds if audio is supposed to be playing and send the specific alc verbs to the audio codec to enable sound output from the speakers. Don't forget to uninstall the `VoodooHDA driver` and `AudioSwitcher utility` before installing and playing around with `AppleALC`...

### VoodooHDA
[VoodooHDA](https://github.com/CloverHackyColor/VoodooHDA) is way easier to set up and works just fine. 

Simply download, unzip and run my [VoodooHDA installer](https://github.com/jlempen/Yoga-Duet-7-13IML05-OpenCore/blob/main/Sound%20Fix/VoodooHDA.zip). The installer will first ask for your password, then macOS will popup a notification asking you to allow the installation of a new kernel extension in the `Privacy & Security` pane of the `System Settings`. Then macOS will tell you it needs to reboot to enable the kernel extension. Agree to everything, reboot and you're done! 

If for whatever reason you wish to uninstall the `VoodooHDA` driver, run the installer again, select "Uninstall", then reboot.

As `VoodooHDA` will not switch automatically between the headphones jack and the internal speakers, you'll have to switch the output manually in the `Sound` pane of the `System Settings` or in the `Audio MIDI Setup` utility.

I wrote a small launch daemon which will take care of that automatically on boot by running [Devon Weller's AudioSwitcher utility](https://github.com/deweller/switchaudio-osx). Simply download, unzip and run my [AudioSwitcher installer](https://github.com/jlempen/Yoga-Duet-7-13IML05-OpenCore/blob/main/Sound%20Fix/AudioSwitcher.zip). Reboot and you're done!

If for whatever reason you wish to uninstall the `AudioSwitcher` utility, run the installer again, select "Uninstall", then reboot.

Both my `VoodooHDA` and `AudioSwitcher` installers will trigger MacOS's quarantine system on launch. This is meant to protect the user from installing unsigned software downloaded from the net. To easily circumvent this protection, simply open a Terminal, enter 
```
xattr -d com.apple.quarantine 
```
and drag the installer to the Terminal window, then press the "Enter" key. Make sure there's a `space` after the above command before dragging the installer to the Terminal window. Now you can launch the installer without macOS bitching about the installer being potentially unsafe.
</details>

<details>
   <summary>Remapping the media keys on the keyboard cover</summary>

## Remapping the media keys on the keyboard cover
As the native media keys on the keyboard cover are not functional yet (I'm pretty sure they used to work in macOS Monterey), simply remap the media keys to the function keys or any other keys or key combinations using the excellent open source [Karabiner-Elements keyboard customizer](https://karabiner-elements.pqrs.org/).

The `Mic Mute` (F4) and `Airplane Mode` (F8) keys do not emit any keycode. However, all the other native media keys emit keycodes that are seen by the `Karabiner-EventViewer`. `Mute` (F1), `Volume Down` (F2), `Volume Up` (F3), `Brightness Down` (F5), `Brightness Up` (F6) and `Calculator` (F12) emit a `consumer_key_code` and the remaining keys emit keycodes which emulate various key combinations. 

The keys emitting key combinations are most certainly remappable, but as they are of no use to me, I haven't bothered to try yet. Moreover, the keys emitting a `consumer_key_code` should also be remappable, but somehow it doesn't seem to work. [The issue](https://github.com/pqrs-org/Karabiner-Elements/issues/3374#issuecomment-2491381424) is referenced in the `Karabiner-Elements` GitHub repo.

For some odd reason, all the native media keys on the keyboard cover start working just fine if you also enable the `Duet 7 USB Composite Device (SIPODEV)` trackpad in the `Devices` section of `Karabiner-Elements`. However, this makes the trackpad go totally crazy and I haven't been able to find a way to fix this problem. There must be some compatibility issue between `VoodooI2C.kext` and/or `VoodooI2CHID.kext` and the `Karabiner DriverKit VirtualHIDPointing 1.8.0 (pqrs.org)` driver installed by `Karabiner-Elements`. [This issue](https://github.com/pqrs-org/Karabiner-Elements/issues/4041) is also referenced in the `Karabiner-Elements` GitHub repo.
</details>

<details>
  <summary>Rotating the display</summary>
  
## Rotating the display
### Rotating the display with the Display Rotation Menu widget
[Display Rotation Menu](https://www.magesw.com/displayrotation/) by Mage Software is a free app designed to quickly rotate the display between Landscape, Portrait, Landscape Flipped or Portrait Flipped right from a menu bar widget. There's also a handy keyboard shortcut to rotate the display back to Landscape: `CTRL-OPTION-COMMAND-0` (as in "zero").

<img width="282" alt="Display Rotation Menu" src="https://github.com/user-attachments/assets/ac058eda-6a75-4979-9f3c-907a8837f07f">

However, you'll need to set the resolution to 2160x1350 before rotating the display to a portrait mode, else the display will go blank. Moreover, when you rotate the display to a portrait orientation, the resolution is set to 1350x2160, which is way too small for the Yoga Duet's display. I have yet to find a way to add more resolutions for the portrait orientation.

Once the display is in portrait orientation, it is very likely that the menu bar will not be able to show the Display Rotation Menu widget anymore because there's not enough space to display every widget in the narrow portrait-mode menu bar. To fix this, hold down the `Command` key while dragging the Display Rotation Menu widget next to the macOS `Control Center` widget. If there is still not enough space to show the widget, you could also change the size of the clock by disabling the `Show date` and/or `Show the day of the week` items or even change the style of the clock to `Analog` in the `Clock Options` of macOS.

[Download Mage Software's Display Rotation Menu v1.5](https://www.magesw.com/displayrotation/DisplayRotationMenu_1.5.zip)

### Rotating the display with BetterTouchTool
[BetterTouchTool](https://folivora.ai/) by folivora.ai is a great, feature packed app that allows you to customize various input devices on your Mac.

With BetterTouchTool, you could define customised Trackpad/Touchscreen gestures to rotate your Yoga Duet 7's display. 

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
The most convenient way to show/hide the On-Screen Keyboard when using the Yoga Duet 7 as a tablet is with a touchscreen gesture. Here's how to set this up: navigate to `System Settings -> Accessibility -> Shortcuts` and uncheck every option but `Accessibility Keyboard`. This enables the keyboard shortcut `OPTION-COMMAND-F5` to show/hide the On-Screen Keyboard. Now use [BetterTouchTool](https://folivora.ai/) to assign this keyboard shortcut to a Trackpad/Touchscreen gesture. 
</details>

<details>
  <summary>Fixing broken Bluetooth on Wake from Hibernation</summary>

## Fixing broken Bluetooth on Wake from Hibernation
After the device wakes up from Hibernation, Bluetooth may be broken / unable to connect.

A very simple fix for this issue is to [download and install Bluesnooze](https://github.com/odlp/bluesnooze). Launch the app, enable `Launch at login` and you're done!
</details>

<details>
  <summary>Fixing broken Apple Messages and FaceTime</summary>
  
## Fixing broken Apple Messages and FaceTime
To fix issues with Apple Messages and FaceTime related to the [Intel Wireless driver](https://github.com/OpenIntelWireless/itlwm) on macOS Sonoma and Sequoia, disable all `AirportItlwm-***.kext` entries under `Kernel -> Add` in your `config.plist` file and use the [itlwm_v2.3.0_stable.kext.zip](https://github.com/OpenIntelWireless/itlwm/releases/download/v2.3.0/itlwm_v2.3.0_stable.kext.zip) and its companion app [HeliPort](https://github.com/OpenIntelWireless/HeliPort/releases/download/v1.5.0/HeliPort.dmg) instead.
The latest version 2.3.0 of `itlwm.kext` is already included in the Kext folder and `config.plist` file.
</details>

<details>
  <summary>Related repositories</summary>
  
## Related repositories
* https://github.com/jlempen/Yoga-Duet-7-13IML05-OpenCore
</details>
