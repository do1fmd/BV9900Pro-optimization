# BV9900Pro-optimization
The Blackview BV9900 Pro is a very good outdoor smartphone with very good hardware specifications.
Unfortunately the software is not running very well. This is a guide, how I got my phone properly working and got rid of most software proplems.
Anyhow, I am not responsible, if you will damage your phone! You are continuing at your own risk!
By the way, all your data will be lost when unlocking the bootloader!

If you follow this guide, you hopefully will get a working rooted BV9900 Pro with MicroG (via Nanodroid) and mostly debloated.
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

# Unlocking Bootloader
Go to the settings, "About phone" and press the Build number at least 10 times in a row.
Now head back to the settings menu -> System. There you will find the new entry "Developer options".
Here you can unlock the bootloader by activating "OEM unlocking" and also enable "USB debugging".
Open terminal and run **adb reboot bootloader** to enter fastboot mode. Alternatively you can switch off the phone and press Volume Up + Power to enter a boot menu. Chose fastboot mode with Volume Up button here and confirm that with Volume Down.
Open terminal on your PC and run **fastboot flashing unlock**
Your device will wipe all your userdata now.
Go back to developers section and make sure "USB debugging" is still enabled.

# Update Firmware (not necessary)
You can now update (and also downgrade) the firmware of your BV9900 Pro with SP Flash tools.
I recommend [https://www.getdroidtips.com/flash-stock-firmware-using-sp-flash-tool/](this guide), if you want to flash a new firmware.

# Rooting BV9900 Pro with Magisk
Boot to the Android system again.
Download the [https://github.com/topjohnwu/Magisk/releases](Magisk-Manager APK) and install it with **adb install Magisk-v22.1.apk** (you have to change the version number accordingly to your downloaded file).
Put the boot.img (from backup or from downloaded firmware) to your device memory (r.g. internal SD card). Make sure that the boot.img fits to your installed firmware version!
Launch Magisk manager and patch the boot image. You can easily follow [this guide](https://www.droidwin.com/patch-stock-boot-image-flash-magisk/).
Copy the patched boot.img to your PC again and restart to Fastboot mode.

Also download this emtpy vbmeta file. Otherwise you will be caught in a bootloop, when you flash the patched Magisk boot image.
Now flash the magisk_patched_boot.img and also the empty vbmeta:
```
fastboot flash boot magisk_patched_boot.img
fastboot --disable-verity --disable-verification flash vbmeta vbmeta.img
fastboot reboot
```
Open Magisk Manager and check, of Magisk is running fine.
Well done! You have rooted your device!

# Installing Nanodroid
To get rid of Google Services, you can install Nanodroid and MicroG now.
## Activate Signature spoofing
Nanodroid needs signature spoofing, if you want to use the Google Play Store.
Download the last version of NanoDroid-patcher (e.g. _NanoDroid-patcher-23.1.2.20210117.zip_) and the full-package (e.g. _NanoDroid-23.1.2.20210117.zip_) from [Nanodroid download page](https://downloads.nanolx.org/NanoDroid/Stable/).
Put both files on your android device (e.g. internal SD).
Download [.nanodroid-setup](./resources/.nanodroid-setup) and [modify](https://gitlab.com/Nanolx/NanoDroid/-/blob/master/doc/AlterInstallation.md) it to your needs.
Explanation of the parameters can be found [here](https://gitlab.com/Nanolx/NanoDroid/-/blob/master/doc/AlterInstallation.md).
Put that file to the sd card. To make sure nanodroid will find the .nanodroid-setup, you can copy this file to /data, too. Use the following commands (grant root access, when requested):
```
adb shell
su
cp external_sd/sdcard0/.nanodroid-setup /data/
```
Open Magisk Manager, proceed to the modules section and click **Install from storage.**
Chose the Nanodroid-patcher zip file and flash it.
Reboot the phone.
Make sure the module "NanoDroid Patcher" is enabled in the Magisk module section. If not, enable it and reboot again.
If you want, you can use [Signature Spoofing Checker](https://f-droid.org/de/packages/lanchon.sigspoof.checker/) to check, if signature spoofing is enabled now.
## Install Nanodroid 
Again, open the Magisk module section and click **Install from storage** again. This time choose the Nanodroid-full-package and flash it.
Reboot again, check if it is enabled and if not enable it and reboot again.
## Remove Stock Google Services
To remove the stock google services and use MicroG in future, enter the following commands (and grant root access, when requested):
```
adb shell
su
nanodroid-overlay -a GmsCore
nanodroid-overlay -a GoogleServicesFramework
reboot
```
Now the phone will reboot.

Open Magisk manager again and proceed to MagiskHide. Search for the entries "microG DroidGuard Helper", "microG Services Core" and "microG Services Framework Proxy". Click on the name (not the checkbox!) of each and disable the appearing sliders from all these apps mentioned.
Now go to the module section of Magisk manager again and install "MagiskHide Props Config" from the Magisk repository.
Reboot the mobile phone.
Check if MagiskHide Props Config is enabled. If not, enable it and reboot again.
