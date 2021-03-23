# Big Sur guest on VirtualBox inside Ubuntu Desktop 20.04 host

## Prerequisites
### VirtualBox
#### Download: 
- https://download.virtualbox.org/virtualbox/6.1.18/virtualbox-6.1_6.1.18-142142~Ubuntu~eoan_amd64.deb
- https://download.virtualbox.org/virtualbox/6.1.18/Oracle_VM_VirtualBox_Extension_Pack-6.1.18.vbox-extpack

#### Install: 
- virtualbox
- add extension pack

#### Source:
- https://www.wikigain.com/how-to-install-macos-big-sur-on-virtualbox-on-windows-pc/

### BigSur iso file
You need a functional Mac OS (preferably a real mac computer). 
All the steps should be executed on a Mac OS.
If you don't have a real Mac, this might be helpful: https://www.wikigain.com/install-macos-catalina-on-vmware-on-windows/ (p.s. I used a real mac)

#### Download: 
- Download BigSur from App store: https://apps.apple.com/us/app/macos-big-sur/id1526878132?mt=12
- Create iso by creating a volume, and then converting it to iso: 
```
sudo hdiutil create -o /tmp/BigSur -size 16384m -volname BigSur -layout SPUD -fs HFS+J
```

```
sudo hdiutil attach /tmp/BigSur.dmg -noverify -mountpoint /Volumes/BigSur
```

```
sudo /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia --volume /Volumes/BigSur --nointeraction
```

```
hdiutil eject -force /Volumes/Install\ macOS\ Big\ Sur
```

```
hdiutil convert /tmp/BigSur.dmg -format UDTO -o ~/Desktop/BigSur
```

```
mv -v ~/Desktop/BigSur.cdr ~/Desktop/BigSur.iso
```

```
sudo rm -fv /tmp/BigSur.dmg
```

#### Source
- https://www.wikigain.com/create-macos-big-sur-iso-image/

## Install Mac OS 
### Prepare VirtualBox
- Create a VM
  -- Name: Mac OS
  -- Type: Mac OS X
  -- Version: macOS 10.13 High Sierra (64-bit)
  -- RAM: minimum 4GB
  -- Chose "Create a virtual hard disk now"
  -- Hard disk file type: VHD (Vidtual Hard Disk)
  -- Dynamically allocated size
  -- Minimum 65 GB of disk size
- Edit settings of VM after creation
  -- System:
    --- Untick Floppy
    --- Add at least 2 processors
  -- Display: 
     --- Add 128 MB of video memory
  -- Storage: 
     --- Mount the BigSur.iso to optical drive
  -- Audio:
     --- Enable Audio output and audio input
  -- Network: 
     --- Set adapter 1 to be a bridged adapter
  -- USB
     --- Enable USB 3.0 xHCI Controller
- Save settings
- Modify Virtual Box machine by executing commands in terminal:
```
vboxmanage modifyvm "Mac OS" --cpuidset 00000001 000106e5 00100800 0098e3fd bfebfbff
```

```
vboxmanage setextradata "Mac OS" "VBoxInternal/Devices/efi/0/Config/DmiSystemProduct" "iMac11,3"
```

```
vboxmanage setextradata "Mac OS" "VBoxInternal/Devices/efi/0/Config/DmiSystemVersion" "1.0"
```

```
vboxmanage setextradata "Mac OS" "VBoxInternal/Devices/efi/0/Config/DmiBoardProduct" "Iloveapple"
```

```
vboxmanage setextradata "Mac OS" "VBoxInternal/Devices/smc/0/Config/DeviceKey" "ourhardworkbythesewordsguardedpleasedontsteal(c)AppleComputerInc"
```

```
vboxmanage setextradata "Mac OS" "VBoxInternal/Devices/smc/0/Config/GetKeyFromRealSMC" 1
```

```
vboxmanage setextradata "Mac OS" "VBoxInternal2/EfiGraphicsResolution" "1920x1080"
```

### Install BigSur
- Start the virtual machine and install MacOS
- Partition the drive first:
  -- After the language selection, select the Disk utility
  -- Erase the biggest empty drive (similar to size you set when setting up the VM in VirtualBox)
  -- Exit Disk Utility
- Click on Install MacOS and follow instructions
- DO NOT SIGN IN TO APPLE USING APPLE ID YET

### Source: 
- https://www.geekrar.com/how-to-install-macos-big-sur-in-virtualbox-windows-2021/
- https://www.youtube.com/watch?v=oO72gadgQGI
- https://hkdb.medium.com/running-macos-in-virtualbox-with-ubuntu-31ec745f21ae
- https://github.com/hkdb/VBoxMacSetup

## Fix Apple ID
### Generate serial number, board id and other crucial information
- Shut down the VM if it's running
- Clone this repo: https://github.com/sickcodes/osx-serial-generator
- Install the dependencies (check readme in repo)
- Run command to generate (in my case: `./generate-unique-machine-values.sh -c 1 --model="iMac11,3"`)
- Add generated information to VirtualBox
```
vboxmanage modifyvm "Mac OS" --cpuidset 00000001 000106e5 00100800 0098e3fd bfebfbff
```

```
vboxmanage setextradata "Mac OS" "VBoxInternal/Devices/efi/0/Config/DmiSystemProduct" "iMac11,3"
```

```
vboxmanage setextradata "Mac OS" "VBoxInternal/Devices/efi/0/Config/DmiSystemSerial" "YOUR_GENERATED_SERIAL_NUMBER"
```

```
vboxmanage setextradata "Mac OS" "VBoxInternal/Devices/efi/0/Config/DmiBoardProduct" "YOUR_GENERATED_BOARD_SERIAL_NUMBER"
```

```
vboxmanage setextradata "Mac OS" "VBoxInternal/Devices/efi/0/Config/DmiSystemUuid" "YOUR_GENERATED_SYSTEM_UUID"
```

```
vboxmanage setextradata "Mac OS" "VBoxInternal/Devices/efi/0/Config/DmiSystemVersion" "1.0"
```

```
vboxmanage setextradata "Mac OS" "VBoxInternal/Devices/smc/0/Config/DeviceKey" "ourhardworkbythesewordsguardedpleasedontsteal(c)AppleComputerInc"
```

```
vboxmanage setextradata "Mac OS" "VBoxInternal/Devices/smc/0/Config/GetKeyFromRealSMC" 0
```

```
vboxmanage setextradata "Mac OS" "VBoxInternal2/EfiGraphicsResolution" "1920x1080"
```

Apply all of the config specified above! 
THIS COULD HAVE EASILY BE DONE BEFORE THE INITIAL VM SETUP. MOVE ABOVE AFTER TESTING!!

### Source:
- https://github.com/sickcodes/osx-serial-generator
- https://www.reddit.com/r/virtualbox/comments/cbr6ig/virtualbox_apple_id_difficulties_running_macos/
- https://github.com/myspaghetti/macos-virtualbox
- https://stackoverflow.com/questions/47965875/cannot-connect-to-apple-id-using-virtual-box-mac-os-high-sierra
Sign in to apple ID only AFTER the fix is successfully applied.

## Fix graphics (only 7mb video memory in guest os)
### Download ISO cd and mount it

### Source:
- https://gist.github.com/pjobson/11d1630d426b1b3ea318e9902db0227c

## Guest Additions for VirtualBox?
- Not sure if needed, but: https://forums.virtualbox.org/viewtopic.php?f=22&t=100837
