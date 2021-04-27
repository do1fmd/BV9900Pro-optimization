# BV9900Pro-optimization
The Blackview BV9900 Pro is a very good outdoor smartphone with very good hardware specifications.
Unfortunately the software is not running very well. This is a guide, how I got my phone properly working and got rid of most software proplems.
Anyhow, I am not responsible, if you will damage your phone! You are continuing at your own risk!
By the way, all your data will be lost when unlocking the bootloader!

If you follow this guide, you hopefully will get a working rooted BV9900 Pro with MicroG and mostly debloated.
In this guide I am working with official firmware version BV9900Pro_EEA_S900AA_V1.0_20201105V07.
Blackview uploaded this version to MEGA and it can be found here: [Download](https://mega.nz/file/vLAnzQDb#AASpRKEQwCOhKTNXtxAPVX2nocLnCZgUdgeFxwDfzg4)
Since you should not trust any random links, I will give you the [source here](https://bbs.blackview.hk/viewtopic.php?f=300&t=538469&start=170)
Before you flash this firmware, dial \*#8615# first and check, if your fingerprint shows _Goodix_ or _sunwave_. If it shows _Goodix_ stop now and do not follow this guide any more, since fingerprint will stop working, if you upgrade to Android 10.

# Installing SP-Flash tool
First of all, you need to install SP-Flash tool and the right Scatter file (which can be found in the firmware).
I would recommend to open option.ini and in the \[ReadBack\] section change _ShowByScatter_ to true:

```
[ReadBack]
ShowByScatter=true
```

# Backup
Switch off your phone and open SP Flash tool. In Download section open the correct scatter file and move on the section "Readback".
Leave out partitions without partition name. For each partition with partition name you should give a recognisable filename to the backup file (e.g. _partitionname_.img) by double-click the partition. In the first window enter the filename. For the second window just press ESC. Do not change any values here!
Afterwards mark the checkbox at the beginning of each partition. Again leave out partitions with empty partition name and click the "Read Back" button.
Now connect the switched-off phone to PC while holding the Vol-Up button.
The readback will take some time.

# Update Firmware (not necessary)
You can now update (and also downgrade) the firmware of your BV9900 Pro with SP Flash tools.
I recommend [https://www.getdroidtips.com/flash-stock-firmware-using-sp-flash-tool/](this guide), if you want to flash a new firmware.


# Unlocking Bootloader
Go to the settings, "About phone" and press the Build number at least 10 times in a row.
Now head back to the settings menu -> System. There you will find the new entry "Developer options".
Here you can unlock the bootloader by activating "OEM unlocking" and also enable "USB debugging".
Open terminal and run **adb reboot bootloader** to enter fastboot mode. Alternatively you can switch off the phone and press Volume Up + Power to enter a boot menu. Chose fastboot mode with Volume Up button here and confirm that with Volume Down.
Open terminal on your PC and run **fastboot flashing unlock**
Your device will wipe all your userdata now.

# Rooting BV9900 Pro with Magisk
Boot to the Android system again.
Download the [https://github.com/topjohnwu/Magisk/releases](Magisk-Manager APK) and install it with **adb install Magisk-v22.1.apk** (you have to change the version number accordingly to your downloaded file).
Put the boot.img (from backup or from downloaded firmware) to your device memory (r.g. internal SD card). Make sure that the boot.img fits to your installed firmware version!
, launch Magisk manager and patch the boot image. You can easily follow [this guide](https://www.droidwin.com/patch-stock-boot-image-flash-magisk/).
