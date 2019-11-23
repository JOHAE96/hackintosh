# Intel Nuc8i5bek hackintosh


## prepare the USB Stick
### create 2 Partions

check which disk is your stick.
```bash
$ diskutil list 
```
change in the command your disk!
this command create 2 partions on your srick.

```bash
$ diskutil partitionDisk /dev/disk2 2 MBR FAT32 "CLOVER EFI" 200Mi HFS+J "install_osx" R 
```

### copy installer image
download the installer from apple (Catalina installer for guys with older macs: https://drive.google.com/file/d/1BhxWr6UG2QNZFRptriS1BAGyaaVslp5x/view?usp=sharing)
```bash
sudo "/Applications/Install macOS Catalina.app/Contents/Resources/createinstallmedia" --volume  /Volumes/install_osx --nointeraction
```

### Install Clover
Copy the 'EFI' Folder from the repository to the EFI partion from your Stick. (if this dont work here is the (source)[https://www.tonymacx86.com/threads/guide-intel-nuc7-nuc8-using-clover-uefi-nuc7i7bxx-nuc8i7bxx-etc.261711/post-1968080])

CLOVER CONFIGURATOR: This is essential for mounting EFI partition
Unzip EFINUC8Cat.zip or EFI.zip. Use Clover Configurator https://mackie100projects.altervista.org/download-clover-configurator/ 

Mount your USB EFI partition using Clover Configurator and copy Leesureone's or Servift's EFI folder in finder to the USB EFI Partition.

Make unique SERIAL NUMBER. The ONLY thing needed to be done if not done already was to use "Clover Configurator" To make a unique serial number open Clover Configurator and on left menu choose SMBIOS. 
Hit generate new for both "serial number" and "smUUID" and on the right check coverage. Look up the many guides on this. Here is one guide but skip down to "Choose your intended System Definition from the list" do a search and it has all the steps to take. This unique serial makes sure iCloud, iMessage, Facetime etc works properly. https://www.tonymacx86.com/threads/an-idiots-guide-to-imessage.196827/


## BIOS settings
The boot menu and BIOS setup can be accessed by mashing the F2 key during BIOS startup. After the main screen comes up choose "Advanced". That gets you to the main BIOS setup screens.

(if F2 don’t work: hold Powerbutton for 3s)

- To start, choose "Load Defaults" (choose from the menu or press F9 in the BIOS setup).

Then change:
- Boot->Boot Configuration, disable "Network Boot"
- Power->Secondary Power Settings, "Wake on LAN from S4/S5", set to "Stay Off"

- These settings are important but are already set as needed by "Load Defaults"
- Devices->Video, "IGD Minimum Memory" set to 64mb or 128mb
- Devices->Video, "IGD Aperture Size" set to 256mb
- Boot->Secure Boot, "Secure Boot" is disabled
- Security->Security Features, "Execute Disable Bit" is enabled. (I don’t find this option)

- Boot->Boot Priority->Legacy Boot Priority, enable "Legacy Boot"

## Boot/install
- Boot menue: F10 Key
- (change in Optiotion > Graphics Injector > FakeID to 0x12345678)
- select start mac os
- erase your ssd with disk utility: apf and guid partion map
- close disk utility
- install macos

## post install

To make your Hackintosh hard drive bootable WITHOUT YOUR USB you need to again use Config Configurator to copy the EFI from the USB drive to your MacOS hard drive. Mount both USB and internal hard drive in Clover Configurator and copy and past the EFI from USB to internal hard drive. 

# credits
- https://www.tonymacx86.com/threads/guide-intel-nuc7-nuc8-using-clover-uefi-nuc7i7bxx-nuc8i7bxx-etc.261711/page-159#post-2019668
- https://www.tonymacx86.com/threads/guide-intel-nuc7-nuc8-using-clover-uefi-nuc7i7bxx-nuc8i7bxx-etc.261711/post-1968080
