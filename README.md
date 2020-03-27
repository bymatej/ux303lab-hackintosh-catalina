# Mac OS Catalina (10.15) on Asus Zenbook UX303LAB laptop PC
Files required for installation of Mac OS 10.15 Catalina on Asus Zenbook UX303LAB laptop PC.

This is the laptop: https://www.asus.com/supportonly/UX303LA/HelpDesk_Download/

More details can be found at https://bymatej.com/ <- insert link from blog when the blog post is finished

## Disclaimer
Do this at your own risk. If you mess something up - it is your fault. I will probably not have any time to provide any support for anyone who tries to install this OS. I just gathered all the info in one place. Keep in mind that if you have Windows or other OS on your machine, this installation will wipe it. Back everything up so even if you mess up you can restore.

## Pre-requirements
Computer running Mac OS Catalina. Legit Mac/MacBook or Hackintosh.
You can find VMWare and VirtualBox images for Hackintosh Catalina on the web. I had a legit MacBook Pro for this. You need a working Mac machine, or hackintosh to be able to download the Catalina installation.

And, of course, you need Asus Zenbook UX303LAB. I am not sure this will work with other UX303 models. You can give it a try if you want.

## Sources
### Must-reads **before doing anything** (must read!!!)
- Instructions for my Zenbook, but for Mac OS El Capitan: https://www.tonymacx86.com/threads/guide-asus-zenbook-ux303la-broadwell-edition.172279/
- Guide for making a bootable USB with Catalina: https://www.tonymacx86.com/threads/unibeast-install-macos-catalina-on-any-supported-intel-based-pc.285366/
- Guide for patching: https://www.tonymacx86.com/threads/guide-patching-laptop-dsdt-ssdts.152573/

### Good reads that may help you (recommended!!!)
- Instructions for Mac OS Catalina (latest as of writing this), but for a different Zenbook: https://www.tonymacx86.com/threads/guide-asus-zenbook-ux310ua-macos-mojave-catalina-installation-guide.224591/
- Guide for fixing the sound and microphone issues: https://www.tonymacx86.com/threads/voodoohda-jack-sense-auto-switching-and-hdmi-audio.185868/#post-1204878
- Catalina SIP disabling guide: https://support.studionetworksolutions.com/hc/en-us/articles/115003839246-How-to-disable-Systems-Integrity-Protection-SIP-in-macOS
- Read-only issues on Catalina (workaround): https://www.insanelymac.com/forum/topic/341349-read-only-file-system-catalina/
- HornDIS installation (internet over USB tethering from Android phone): https://github.com/jwise/HoRNDIS/issues/102#issuecomment-540072360
- AppStore issue fix (after installation): https://www.youtube.com/watch?v=isg7mGyzx9o

### Additional useful resources (optional)
- Some code for my laptop on GitHub: https://github.com/hieplpvip/ASUS-ZENBOOK-HACKINTOSH/issues/3
- Info on missing trackpad settings: https://www.hackintoshzone.com/files/file/1032-script-to-load-basic-trackpad-preferences/
- More on trackpad settings: https://www.insanelymac.com/forum/topic/315768-blank-trackpad-setting-in-sierra/
- More on trackpad: https://www.tonymacx86.com/threads/solved-trackpad-preference-pane-is-blank.199946/page-4
- And more: https://www.tonymacx86.com/threads/solved-trackpad-preference-pane-is-blank.199946/
- Backing up your disk with DD: https://www.cyberciti.biz/faq/how-to-create-disk-image-on-mac-os-x-with-dd-command/
- If you can boot El Capitan, but cannot boot Catalina after upgrade: https://www.tonymacx86.com/threads/catalina-hates-me.286452/

## DSDTs and SSDTs
**Do not** use my patched DSDTs and SSDTs. There is 99,99% chance they will not work with your laptop - even if you have the same model. Slight differences in BIOS settings and hardware configuration can render my patched DSDTs and SSDTs useless for your PC. 

Always generate new ones. The easiest way is: 
- insert bootable USB with Clover installed
- when in clover menu press F4 on your keyboard
- boot the OS from USB
- all original files will appear in `/EFI/EFI/CLOVER/ACPI/origin`

## Kexts
You can extract kexts that I provided here, but I recommend using the latest versions of these kexts from their official repos. The ones I packed worked for me on March the 18th of 2020 on Mac OS Catalina 10.15.3.

Kexts used: 
- ACPIBatteryManager.kext: https://bitbucket.org/RehabMan/os-x-acpi-battery-driver/downloads/
- AsusNBFnKeys.kext: I got it from one of the links above. If you cannot find the official repo, use this kext.
- CodecCommander.kext: https://bitbucket.org/RehabMan/os-x-eapd-codec-commander/downloads/
- DummyHDA.kext: I got it from one of the links above. If you cannot find the official repo, use this kext.
- HibernationFixup.kext: https://github.com/acidanthera/HibernationFixup/releases
- Lilu.kext: https://github.com/acidanthera/lilu/releases
- NullEthernet.kext: https://bitbucket.org/RehabMan/os-x-null-ethernet/downloads/
- USBInjectAll.kext: https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/
- VirtualSMC.kext: https://github.com/acidanthera/VirtualSMC/releases
- VoodooHDA.kext: https://sourceforge.net/projects/voodoohda/
- VoodooPS2Controller.kext: https://bitbucket.org/RehabMan/os-x-voodoo-ps2-controller/downloads/
- WhateverGreen.kext: https://github.com/acidanthera/whatevergreen/releases

Kext locations:
All kexts need to be placed in /EFI/EFI/CLOVER/kexts (both in your bootable USB and later on on your HDD/SSD).

These kexts need to be placed in `/Library/Extensions/` directory on your HDD/SSD after installation:
- DummyHDA.kext
- VoodooHDA.kext

What is `AX88772C_772B_772A_760_772_Macintosh_Driver_Installer_v2.8.0` in kexts directory?

This is a driver for the Ethernet USB dongle.

## Important info
### Fn Brightness Keys Patch
This patch came with UX303LA.zip file from https://www.tonymacx86.com/threads/guide-asus-zenbook-ux303la-broadwell-edition.172279/

It doesn't work.
I used the `Fn Brightness Keys Patch.txt` patch from Asus DSDT patches.zip file found in https://www.tonymacx86.com/threads/guide-asus-zenbook-ux310ua-macos-mojave-catalina-installation-guide.224591/

### DummyHDA.kext for sound
It is in UX303LA.zip file from https://www.tonymacx86.com/threads/guide-asus-zenbook-ux303la-broadwell-edition.172279/

Do not use it. It does not work with Catalina. Use the kexts I provided here - DummyHDA.kext and VoodooHDA.kext.

### USB Dongle drivers
Just install `AX88772_v2_8_0.dmg` found in kexts directory.
You might need to turn the SIP off and disable write protection for this: 
- Catalina SIP disabling guide: https://support.studionetworksolutions.com/hc/en-us/articles/115003839246-How-to-disable-Systems-Integrity-Protection-SIP-in-macOS
- Read-only issues on Catalina (workaround): https://www.insanelymac.com/forum/topic/341349-read-only-file-system-catalina/

### HoRNDIS
HoRNDIS lets you use your Android phone as an USB internet hotspot.

Connect your Android to a WiFi network or to mobile network. Install `HoRNDIS-9.2.pkg` from tools.zip. Plug your Android phone and enable USB Tethering. Your Hackintosh should be connected to the internet and it should be visible in the Network settings.

 You might need to turn the SIP off and disable write protection for this: 
 - Catalina SIP disabling guide: https://support.studionetworksolutions.com/hc/en-us/articles/115003839246-How-to-disable-Systems-Integrity-Protection-SIP-in-macOS
 - Read-only issues on Catalina (workaround): https://www.insanelymac.com/forum/topic/341349-read-only-file-system-catalina/
 
 In case of trouble, refer here: https://github.com/jwise/HoRNDIS/issues/102#issuecomment-540072360
 
 ### Internal WiFi
 It does not work. If you replace the chip, it might work. If you want to do that refer to this: https://www.tonymacx86.com/threads/guide-asus-zenbook-ux303la-broadwell-edition.172279/
 
 I did not do that, as I use my internet from my USB dongle and/or HoRNDIS.
 
 ### Repos for MaciASL
 Before patching read this first if you haven't: https://www.tonymacx86.com/threads/guide-patching-laptop-dsdt-ssdts.152573/ 
 You need three sources: 
 - http://raw.github.com/RehabMan/Laptop-DSDT-Patch/master
 - http://maciasl.sourceforge.net/pjalm/intel9
 - `file:///Users/[USERNAME NEEDS TO GO HERE]/Location/Of/Where/You/Extracted/Patches/Directory/From/UX303LA.zip/File`
 
 _Example for the third repo: `file:///Users/user/Desktop/Patches`_
 
 Instructions on how to add sources can be found somewhere here: https://www.tonymacx86.com/threads/guide-asus-zenbook-ux310ua-macos-mojave-catalina-installation-guide.224591/
 
 Download the latest MaciASL or build the one yourself. Either download from the links found here: https://www.tonymacx86.com/threads/guide-patching-laptop-dsdt-ssdts.152573/ ,  or read the section _Building the latest iasl from github_ form this link for building it yourself. The MaciASL.app that I provided should work fine.
 
 Download: https://bitbucket.org/RehabMan/os-x-maciasl-patchmatic/downloads/

## What works and what doesn't?
Everything works except: 
- WiFi (you can make it work if you swap out the WiFi card)
- TouchPad (probably patchable, but so far I had no luck with it)

I did not test the HDMI and mini display port.

Everything else works fine so far. External and internal microphone works. GarageBand works flawlessly with my iRig 2. Keyboard works nice (and it is backlit).

## Multiple OS boot
I configured a triple boot: Mac OS Catalina, Windows 10 and Linux Ubuntu 18.04

Just google how to do it.

Basically, I did the following: 
- during the installation of Hackintosh I divided the disk in two pieces 150 GB for Mac OS and rest for other OS)
- first partition (150 GB) was APFS, and second one was FAT (I formatted it properly later)
- Then I installed the Mac OS first
- Then I installed Windows (and during the instalation of Windows I split the FAT partition in two and one part was NTFS and other FAT32 (later used for Linux and reformatted for Linux))
- Then I fixed my boot order in BIOS so Clover is first (disable fast startup in Windows to be able to enter BIOS again: https://help.uaudio.com/hc/en-us/articles/213195423-How-To-Disable-Fast-Startup-in-Windows-10)
- Then I installed Ubuntu and this messed up my Clover (it was gone in BIOS for Boot order).
- Then I fixed Clover inside my Linux by following these instructions: https://www.tonymacx86.com/threads/guide-how-to-keep-clover-working-when-installing-windows-and-linux.272807/
- When everything was installed the clock was wrong in Windows, so I fixed it by following this guide: https://www.tonymacx86.com/threads/fix-incorrect-time-in-windows-osx-dual-boot.133719/

And that was it in a nutshell. Please, google this if you need help. It is easier than it seems.

## Credit
I am not an author of any of this. I just gathered all the information on one place for my own reference and I think it might help somebody else out too.
All credit goes to: 
- Apple
- Owners of the forums from where I gathered all the info
- Authors of the posts on respective forums
- People replying to posts with useful information
- Owners of above-mentioned GitHub repositories
- Developers of all tools used

# Enjoy
https://bymatej.com/

