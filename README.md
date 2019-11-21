Intel Nuc8i5bek hackintosh

Mojave

General
https://www.tonymacx86.com/threads/guide-intel-nuc7-nuc8-using-clover-uefi-nuc7i7bxx-nuc8i7bxx-etc.261711/

add EmuVariableUefi-64.efi to drivers64UEFI
Where
→ in Clover installer custemize ?
use the nuc8 variant plists (config_install_nuc8_bc.plist, config_nuc8_bc.plist) 
(from https://github.com/RehabMan/Intel-NUC-DSDT-Patch )
use the nuc8 make install script ('make install_nuc8bc')
Where
in BIOS settings you *must* enable legacy boot (continue to boot UEFI, but enable legacy for CSM) (you will have KP/reboot without it)
Preparing USB and initial Installation
https://www.tonymacx86.com/threads/guide-booting-the-os-x-installer-on-laptops-with-clover.148093/ 

MBR with a FAT32 partition for Clover and a separate HFS+J partition for the OS X installer.

$ diskutil list # check for your disk
# repartition /dev/disk1 MBR, two partitions
# first partition, "CLOVER EFI" FAT32, 200MiB
# second partition, "install_osx", HFS+J, remainder
diskutil partitionDisk /dev/disk2 2 MBR FAT32 "CLOVER EFI" 200Mi HFS+J "install_osx" R
download current clover: 
https://sourceforge.net/projects/cloverefiboot/ 
https://github.com/RehabMan/Clover 


install to the USB "CLOVER EFI" partition.
select the target of the install to "CLOVER EFI" using "Change Install Location"

select "Customize" (the default is a legacy install -- we need to change it)
check "Install for UEFI booting only", "Install Clover in the ESP" will automatically select
check "BGM" from Themes (the config.plist files I provide use this theme)
the defaults for Drivers64UEFI are recommended

Finally, we need one EFI driver not included in the Clover installer, HFSPlus.efi:
it can be downloaded from here: https://github.com/JrCs/CloverGrowerPro/raw/master/Files/HFSPlus/X64/HFSPlus.efi.
copy it to /EFI/Clover/drivers64UEFI
Preparing essential kexts
Remove EFI/CLOVER/kexts/10.6, 10.7, 10.8, 10.9, 10.10, leaving just 'Other'. Note that if you use the RehabMan Clover build, you will only see 'Other' (less clutter in my build).

 Copy essential kexts to the 'Other' directory (
FakeSMC.kext
 https://github.com/RehabMan/OS-X-FakeSMC-kozlek
VoodooPS2Controller.kext
https://github.com/RehabMan/OS-X-Voodoo-PS2-Controller
Lilu.kext
https://github.com/acidanthera/Lilu
WhateverGreen.kext
https://github.com/acidanthera/WhateverGreen (replaces IntelGraphicsFixup.kext))
You only need the kexts that allow you to boot and operate the installer. Other kexts that you might use in the final installation can wait.

USBInjectAll.kext may be helpful for reaching the installer (and in post-install... see FAQ)
USBInjectAll.kext: https://github.com/RehabMan/OS-X-USB-Inject-All

Note: Please READ the README at each link so you know where pre-built binaries are located. Copy only the kext to Clover/kexts/Other (usually the kext is found in the Release folder inside the ZIP). At the acidanthera github, pre-built kexts are available from the "Releases" page.
simple copy/paste with Finder to EFI/Clover/kexts/Other
hack-tools github project:
https://github.com/RehabMan/hack-tools
If your main SSD is NVMe, make sure you read important NVMe information in the FAQ:
http://www.tonymacx86.com/el-capitan-laptop-support/164990-faq-read-first-laptop-frequent-questions.html 
config.plist
Always use a plist editor (such as Xcode or PlistEdit Pro) when making changes to config.plist.
use the nuc8 variant plists (config_install_nuc8_bc.plist, config_nuc8_bc.plist) 
(from https://github.com/RehabMan/Intel-NUC-DSDT-Patch )
paste it to /EFI/Clover, make sure it is re-named as config.plist. Clover will only load configurations from /EFI/Clover/config.plist.


Building the OS X installer
download mojave
https://support.apple.com/de-de/macos/mojave
It is a single line, executed in Terminal:
Code:
# copy installer image
sudo "/Applications/Install macOS Mojave.app/Contents/Resources/createinstallmedia" --volume  /Volumes/install_osx --nointeraction
Then change the name to be less unwieldy...
Code:
# rename
sudo diskutil rename "Install macOS Mojave" install_osx



BIOS settings
The boot menu and BIOS setup can be accessed by mashing the F2 key during BIOS startup. After the main screen comes up choose "Advanced". That gets you to the main BIOS setup screens.

(if F2 don’t work: hold Powerbutton for 3s)

To start, choose "Load Defaults" (choose from the menu or press F9 in the BIOS setup).

Then change:
Boot->Boot Configuration, disable "Network Boot"
Power->Secondary Power Settings, "Wake on LAN from S4/S5", set to "Stay Off"

These settings are important but are already set as needed by "Load Defaults"
Devices->Video, "IGD Minimum Memory" set to 64mb or 128mb
Devices->Video, "IGD Aperture Size" set to 256mb
Boot->Secure Boot, "Secure Boot" is disabled
Security->Security Features, "Execute Disable Bit" is enabled. (I don’t find this option)

Boot->Boot Priority->Legacy Boot Priority, enable "Legacy Boot"




Boot
Boot menue: F10 Key
select start mac os


