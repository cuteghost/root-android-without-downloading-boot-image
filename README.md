# Description

Purpose of this guide is to show you how to root android phone without downloading the specific OS .zip file.

# Disclaimer Alert
All the actions mentioned in this repository is your solely responsibility. Please be aware that this could brick your phone, and maybe put it in irrevertible state(fcking dead).

I am not responsible if you brick your phone.

## Requirements
Hardware:
* PC with Windows(Following tools should work on linux too)
* Original USB cable

Software
* SDK tool
* Magisk-Manager
* twrp.img

All the tools from requirements are in this repository under directory called `tools`.

## Steps

1. Enter developer mode on android. On most phones the way to enter developer mode is to open:

`Settings -> About phone -> Build Number`

Tap on Build Number until pop shows 'You are in developer mode now'

2. Enable OEM unlocking and USB debuging by going to the:
`Settings -> System -> Developer Options`. 
Find OEM unlocking and enable it. Scroll down to find USB debuging and enable that too.

3. Connect the phone to the PC via original USB cable

4. On PC open `cmd` as Administrator in folder where SDK tools are located.

5. Type in `cmd` following command
```bash
$ adb reboot bootloader
```

6. Wait for the phone to boot up in fastboot mode

7. In `cmd` type next command[NOTE: This command will wipe your phone data]:
```bash
$ fastboot flashing unlock
```

8. Reboot to the system from `cmd` by typing:
```bash
$ fastboot reboot system
```

9. Configure device once more with google account and wait for app updates to finish if there are any.

10. After updates are done in `cmd` type following command to boot in fastboot mode:
```bash
$ adb reboot bootloader
```

11. After phone boots up, flash the recovery with the `twrp.img` by typing following command in the `cmd`:
```bash
$ fastboot flash recovery twrp.img
```

12. After flashing is done boot into the recovery using following command in the `cmd`:
```bash
$ fastboot reboot recovery
```

13. Wait for the TWRP to boot. After menu showed up, go to the: `Advanced -> Capture Log -> Include Kernel Log -> Swipe to get the log`

14. If MTP is enabled you can copy `recovery.txt` that can be found in location `/sdcard/recovery.txt` to the PC. Otherwise look for `Enable MTP` option inside TWRP and copy the file.

15. Open `recovery.txt` and search for `boot` and look for something like this

```bash
/boot | /dev/block/bootdevice/by-name/boot_a | Size: 96MB
   Flags: Can_Be_Backed_Up IsPresent Can_Flash_Img SlotSelect 
   Primary_Block_Device: /dev/block/bootdevice/by-name/boot
   Display_Name: Boot
   Storage_Name: Boot
   Backup_Path: /boot
   Backup_Name: boot
   Backup_Display_Name: Boot
   Storage_Path: /boot
   Current_File_System: emmc
   Fstab_File_System: emmc
   Backup_Method: dd
```
The `/dev/block/bootdevice/by-name/boot_a`, in this example, is the location where `boot.img` is loaded on the phone. On other phones that location could be something else.

16. On PC use `adb` to create `boot.img`. In the same `cmd` type following commands:

```bash
$ adb shell
   ...
   android:/$ dd if=/dev/block/bootdevice/by-name/boot_a of=/sdcard/boot.img
      ...
      Command successfully executed! 
   android$ exit
```

17. Reboot to the system by typing next command:
```bash
$ adb reboot system
```

18. Once system boots up install Magisk-Manager from `magisk.apk`

19. Open Magisk-Manager and select next to the `Magisk` `Install` button, tap on `Select and Patch a File`. Choose boot.img from internal storage that you created. It should be located in `/sdcard/boot.img` location(Step 16.)

20. After that is done copy the patched image to the PC into the SDK folder and rename it to boot-rooted.img

21. In `cmd` type `$ adb reboot bootloader` to reboot to fastboot

22. Once in fastboot in `cmd` type:
```bash
$ fastboot flash boot boot-rooted.img
```

23. Wait for it to finish. Once it is done reboot to the system with:
```bash
$ fastboot reboot system
``` 

24. After system boots up, open Magisk-Manager to verify that root has been granted.

25. Enjoy ;)