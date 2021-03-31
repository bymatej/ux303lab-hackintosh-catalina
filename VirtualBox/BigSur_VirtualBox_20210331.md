# Disclaimer
You can legally install Apple Software only on Apple hardware. I have access to a real MacBook Pro. 
Installing this on non Apple hardware is considered piracy. 
If you still attempt to install this on a non Apple hardware, then you do this on your own risk. It might not even work out for you. 
I am not responsible for any damage or harm on your PC, or if you face any legal issues.

That being said - we can move on.

# Introduction
This is not owned by me. I just gathered the software around the internet, and I grouped it here together, so that you know how to install.

I used this to install Mac OS Big Sur as a Guest OS inside VirtualBox on my Linux Ubuntu as a Host OS.

## Sources used during installation
- https://www.reddit.com/r/virtualbox/comments/hfge4j/macos_big_sur_on_virtualbox/
- https://support.apple.com/en-us/HT211983
- http://swcdn.apple.com/content/downloads/12/32/071-14766-A_Q2H6ELXGVG/zx8saim8tei7fezrmvu4vuab80m0e8a5ll/InstallAssistant.pkg
- https://github.com/myspaghetti/macos-virtualbox
- https://github.com/sickcodes/osx-serial-generator


Everything I describe below can be found in the ReadMe files of the above-mentioned GitHub repositories. Read the ReadMe if you get stuck, or want to know more!

Basically, I installed Catalina, then upgraded to Big Sur.

# Installation steps (Catalina installation)
## Clone the `macos-virtualbox` repo from git
- Open the terminal and type `git clone https://github.com/myspaghetti/macos-virtualbox.git`
- Read the ReadMe and install all the dependencies

## Prepare the `macos-guest-virtualbox.sh` file by configuring your VM
- Open the `macos-guest-virtualbox.sh` file in your favorite text editor
- You need to edit the configuration in the `set_variables` function - the hardware configuration and Mac serial numbers

### Hardware configuration
I am just going to put my configuration here, so you can see how I did it. You do it for yourself.
```
vm_name="macOS"                  # name of the VirtualBox virtual machine
macOS_release_name="Catalina"    # install "HighSierra" "Mojave" or "Catalina"
storage_size=220000              # VM disk image size in MB, minimum 22000
storage_format="vdi"             # VM disk image file format, "vdi" or "vmdk"
cpu_count=2                      # VM CPU cores, minimum 2
memory_size=8192                 # VM RAM in MB, minimum 2048
gpu_vram=128                     # VM video RAM in MB, minimum 34, maximum 128
resolution="1920x1080"           # VM display resolution
```

### Serial numbers - EFI and NVRAM parameters - needed to get the iCloud working
You need to generate the unique set of information for your VM.

#### Generating unique serial numbers
- Clone the `osx-serial-generator` repo: `git clone https://github.com/sickcodes/osx-serial-generator.git`
- Read the ReadMe and install all the dependencies
- Go to the cloned repo, and execute the `generate-unique-machine-values.sh`. Ensure to pass in the device model if you want to. I did it like this: `./generate-unique-machine-values.sh -c 1 --model="iMacPro1,1"
- This will generate the required serial numbers, uuids, etc. Copy them to the `macos-guest-virtualbox.sh` inside `set_variables` functions on the right places

This is how the generated output of `./generate-unique-machine-values.sh -c 1 --model="iMacPro1,1"` command looks like:
(DO NOT USE THESE VALUES AND DO NOT COPY/PASTE THEM ANYWHERE, THEY ARE JUST HERE FOR THE REFERENCE)
```
DEVICE_MODEL:       iMacPro1,1
SERIAL_SET_COUNT:   1
OUTPUT_DIRECTORY:   .
Warning: arc4random is not available!
"iMacPro1,1","C02ZV9YNHX87","C02951301GUJG361M","58F55DB6-770B-4979-B16B-4EBDCB9A42B7","D8:30:62:1F:75:2C","1920","1080"
Wrote CSV to: ./serial_sets-2021-03-31-23:36:53.csv
iMacPro1,1	C02ZV9YNHX87	C02951301GUJG361M	58F55DB6-770B-4979-B16B-4EBDCB9A42B7	D8:30:62:1F:75:2C	1920	1080
Wrote TSV to: ./serial_sets-2021-03-31-23:36:53.tsv
DEVICE_MODEL,SERIAL,BOARD_SERIAL,UUID,MAC_ADDRESS,WIDTH,HEIGHT
"iMacPro1,1","C02ZV9YNHX87","C02951301GUJG361M","58F55DB6-770B-4979-B16B-4EBDCB9A42B7","D8:30:62:1F:75:2C","1920","1080"
DEVICE_MODEL	SERIAL	BOARD_SERIAL	UUID	MAC_ADDRESS	WIDTH	HEIGHT
iMacPro1,1	C02ZV9YNHX87	C02951301GUJG361M	58F55DB6-770B-4979-B16B-4EBDCB9A42B7	D8:30:62:1F:75:2C	1920	1080
```

The last two lines are the most important ones.
In your `macos-guest-virtualbox.sh` you would paste this like so:
```
DmiSystemFamily="iMacPro1,1"         # Model Name
DmiSystemProduct="iMacPro1,1"        # Model Identifier
DmiSystemSerial="C02ZV9YNHX87"       # Serial Number (system)
DmiSystemUuid="58F55DB6-770B-4979-B16B-4EBDCB9A42B7" # Hardware UUID
DmiBIOSVersion="string:MBP7.89"      # Boot ROM Version
DmiOEMVBoxVer="string:1"             # Apple ROM Info - left of the first dot
DmiOEMVBoxRev="string:.23.45.6"      # Apple ROM Info - first dot and onward
#   ioreg -l | grep -m 1 board-id
DmiBoardProduct="Mac-3CBD00234E554E41"
#   nvram 4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14:MLB
DmiBoardSerial="C02951301GUJG361M"    # stored in EFI
MLB="${DmiBoardSerial}"               # stored in NVRAM
#   nvram 4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14:ROM
ROM='%aa*%bbg%cc%dd'
#   ioreg -l -p IODeviceTree | grep \"system-id
SYSTEM_UUID="0BBAF458-D7F0-421B-99F2-7059E396CEF4"
#   csrutil status
SYSTEM_INTEGRITY_PROTECTION='10'  # '10' - enabled, '77' - disabled
```

BTW, I set `SYSTEM_INTEGRITY_PROTECTION` (the last entry) to value `'77'` on my machine.


This completes the preparation of the `macos-guest-virtualbox.sh`!

## Running the script
The script consists of the several "stages". Run each stage and follow the on screen instructions.
Here are all the commands that I ran (one by one - read the on screen instructions carefully!):
```
./macos-guest-virtualbox.sh check_shell
./macos-guest-virtualbox.sh check_gnu_coreutils_prefix
./macos-guest-virtualbox.sh set_variables
./macos-guest-virtualbox.sh welcome
./macos-guest-virtualbox.sh check_dependencies
./macos-guest-virtualbox.sh prompt_delete_existing_vm
./macos-guest-virtualbox.sh create_vm
./macos-guest-virtualbox.sh check_default_virtual_machine
./macos-guest-virtualbox.sh prepare_macos_installation_files
./macos-guest-virtualbox.sh create_nvram_files
./macos-guest-virtualbox.sh create_macos_installation_files_viso
./macos-guest-virtualbox.sh configure_vm
./macos-guest-virtualbox.sh populate_basesystem_virtual_disk
./macos-guest-virtualbox.sh create_bootable_installer_virtual_disk
./macos-guest-virtualbox.sh populate_bootable_installer_virtual_disk
./macos-guest-virtualbox.sh create_target_virtual_disk
./macos-guest-virtualbox.sh populate_macos_target_disk
./macos-guest-virtualbox.sh prompt_delete_temporary_files
```

Before the last three (3) commands (or before last line) your VM will shut down (or reboot, I don't remember).
When resetting or powering up the VM, immediately press Esc when the VirtualBox logo appears. This boots into the EFI Internal Shell or the boot menu. 
If the boot menu appears, select \"Boot Manager\" and then \"EFI Internal Shell\" and then allow the ${low_contrast_color}startup.nsh${default_color} script to execute automatically, applying the NVRAM variables before booting macOS.

This is needed to load and persist all the serial numbers and uuids so your iCloud will work right from the get go.

After that - you're done! You have a fully working Mac OS Catalina. Now le's upgrade to Big Sur.

# Upgrading to Big Sur
- Open the App Store and search for Big Sur
- Download it
- Do NOT run the Big Sur upgrade yet!
- Power down your VM.
- Go to VM settings in VirtualBox and: 
  -- disable network (go to network, and untick all adapters)
  -- change USB settings to USB 1.1 (go to USB in settings and set USB 1.1)
- Power on your VM and run the upgrade
- Wait for the upgrade to finish, and when it's finished, power down the VM and change back the above-mentioned settings (enable network back, and use USB 3.0)
- Boot back your VM again and you have Big Sur

# Fixing the Graphics
Notice you have only 7 MB of memory when you click on About My Mac (or even less) despite the 128 MB in the VirtualBox settings.
The goal is to bump it higher!

## Fix graphics (only 7mb video memory in guest os)
### Apply fix
- Shut down the VM
- Download ISO cd and mount it in optical drive: https://github.com/bymatej/ux303lab-hackintosh-catalina/raw/master/VirtualBox/vm_tools.iso
- Execute this in terminal (on your Linux Host OS): 
```
vboxmanage modifyvm "macOS" --vram 128 --graphicscontroller vmsvga
```
- Start the VM and launch the Terminal in the VM (in Mac OS)
- Execute the following commands:
```
sudo spctl --master-disable
```

```
sudo spctl --status
```

- The last command should return `assessments disabled`
- Close Terminal
- Open System Preferences, click on the little lock icon in the bottom left corner, and allow apps downloaded from Anywhere
- Open the mounted optical disk, run it and install (allow everything when prompted)
- Reboot
- Respond to all prompts and allow everything

### Source:
- https://gist.github.com/pjobson/11d1630d426b1b3ea318e9902db0227c

## Fix resolution after fixing the video memory
### Apply fix: 
- Boot into the VM
- Download VMWare preference pane: https://github.com/bymatej/ux303lab-hackintosh-catalina/raw/master/VirtualBox/VMware.prefPane.pkg
- Install it
- Set the resolution in settings (you will see a new entry in System Preferences) or by executing this in Mac terminal: `cd /Library/Application Support/VMware Tools && ./vmware-resolutionSet <width> <height>` where width and height are numbers like 1920 and 1080
```
/Library/Application\ Support/VMware\ Tools/vmware-resolutionSet 1920 1080
```

# Fixing the Audio
Notice that the Audio does not work inside your VM.

We will load two kexts. These two worked for me. If they don't work for you, then you need to do more research on your hardware.

- Download kexts in zip: https://github.com/bymatej/ux303lab-hackintosh-catalina/raw/master/VirtualBox/kexts_audio.zip
- Extract the zip to get kexts
- Copy them to `/Library/Extensions/`
- Reboot

Note: You can download the latest version of the Vodoo kext from the official repo: https://sourceforge.net/projects/voodoohda/

Your audio will hopefully work now!

# Improve performance
- Download beamoff on your host machine: 
- Mount the iso in the virtual optical drive on VM
- Boot your VM
- Open the mounted disk on the guest machine and run beamoff file

Your performance might be increased now.

# Tips
- Disable sleep in power options. It crashes the VM sometimes.
- 
