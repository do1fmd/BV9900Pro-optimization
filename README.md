# Optimization of Blackview BV9900 Pro
The Blackview BV9900 Pro is a very good outdoor smartphone with very good hardware specifications.
Unfortunately the software is not running very well. This is a guide, how I got my phone properly working and got rid of most software proplems.
Anyhow, I am not responsible, if you will damage your phone! You are continuing at your own risk!
By the way, all your data will be lost when unlocking the bootloader!

If you follow this guide, you hopefully will get a working rooted BV9900 Pro with MicroG (via Nanodroid) and mostly debloated.
In this guide I am working with official **EEA** firmware version BV9900Pro_EEA_S900AA_V1.0_20201105V07.
Blackview uploaded this version to MEGA and it can be found [here](https://mega.nz/file/vLAnzQDb#AASpRKEQwCOhKTNXtxAPVX2nocLnCZgUdgeFxwDfzg4).
Remember, do not trust any random links! Check the [source here](https://bbs.blackview.hk/viewtopic.php?f=300&t=538469&start=170#p999471).
Before you flash this firmware, dial \*#8615# and check, if your fingerprint shows _Goodix_ or _sunwave_. If it shows _Goodix_ stop now and do not follow this guide any more, since fingerprint will stop working, if you upgrade to Android 10 and I am not sure, if this guid will work for Android 9!

# Prerequirements
Make sure you have a working installation of adb and fastboot!
If you use Ubuntu, you can easily install it by running
```
sudo apt -y install adb fastboot
```

# Installing SP-Flash tool
First of all, you need to install [SP-Flash tool](https://spflashtool.com/download/) and have the right Scatter file available (which can be found in the firmware [or here](resources/MT6779_Android_scatter.txt)).
If you want to backup your device, open option.ini and in the _\[ReadBack\]_ section change _ShowByScatter_ to true:

```
[ReadBack]
ShowByScatter=true
```

# Backup (Recommended but not mandatory)
Switch off your phone and open SP Flash tool. In Download section open the correct scatter file and move on the section "Readback".
Leave out partitions without partition name. For each partition with partition name you should give a recognisable filename to the backup file (e.g. _partitionname_.img) by double-click the partition. In the first window enter the filename. For the second window just press ESC. Do not change any values here!
Afterwards mark the checkbox at the beginning of each partition. Again leave out partitions with empty partition name and click the "Read Back" button.
Now connect the switched-off phone to PC while holding the Vol-Up button.
The readback will take some time.

# Unlocking Bootloader
Go to the settings, "About phone" and press the Build number at least 10 times in a row.
Now head back to the settings menu -> System. There you will find the new entry "Developer options".
Here you can unlock the bootloader by activating "OEM unlocking" and also enable "USB debugging".
Open terminal and run ``adb reboot bootloader`` to enter fastboot mode. Alternatively you can switch off the phone and press Volume Up + Power to enter a boot menu. Choose fastboot mode with Volume Up button here and confirm that with Volume Down.
Open terminal on your PC and run ``astboot flashing unlock``. Now reboot your phone by executing ``fastboot reboot``.
All your usedata will get wiped now.

# Update Firmware (Not mandatory, if Android 10 is already installed)
You can now update (and also downgrade) the firmware of your BV9900 Pro with SP Flash tools.
I recommend [this guide](https://www.getdroidtips.com/flash-stock-firmware-using-sp-flash-tool/), if you want to flash a different firmware.

# Rooting BV9900 Pro with Magisk
Boot to the Android system again.
Go back to developers section and make sure "USB debugging" is still enabled.
Download the [Magisk-Manager APK](https://github.com/topjohnwu/Magisk/releases) and install it with ``adb install Magisk-v22.1.apk`` (you have to change the version number accordingly to your downloaded file).
Put the boot.img (from backup or from downloaded firmware) to your device memory (e.g. internal SD card). Make sure that the boot.img fits to your installed firmware version!
Launch Magisk manager and patch the boot image. You can easily follow [this guide](https://www.droidwin.com/patch-stock-boot-image-flash-magisk/).
Copy the patched boot.img to your PC again and restart to Fastboot mode.

Also download [this emtpy vbmeta file](resources/vbmeta.img). Otherwise you will be caught in a bootloop, when you flash the patched Magisk boot image.
Now flash the magisk_patched_boot.img and also the empty vbmeta:
```
fastboot flash boot magisk_patched_boot.img
fastboot --disable-verity --disable-verification flash vbmeta vbmeta.img
fastboot reboot
```
Open Magisk Manager and check, of Magisk is running fine.
Well done! You have rooted your device!

# Replacing Google Services by Nanodroid/MicroG
To get rid of Google Services, you can install Nanodroid and MicroG now.
## Activate Signature spoofing
Nanodroid needs signature spoofing, if you want to use the Google Play Store.
Download the latest versions of NanoDroid-patcher (e.g. _NanoDroid-patcher-23.1.2.20210117.zip_) and full-package (e.g. _NanoDroid-23.1.2.20210117.zip_) from [Nanodroid download page](https://downloads.nanolx.org/NanoDroid/Stable/).
Put both files on your android device (e.g. internal SD).
Download [.nanodroid-setup](./resources/.nanodroid-setup) and [modify](https://gitlab.com/Nanolx/NanoDroid/-/blob/master/doc/AlterInstallation.md) it to your needs.
Put that file to the internal sd card. To make sure nanodroid will find the .nanodroid-setup, you can copy this file to /data, too. Use the following commands (grant root access, if requested):
```
adb shell
su
cp external_sd/sdcard0/.nanodroid-setup /data/
```
Open Magisk Manager, proceed to the modules section and click **Install from storage.**
Select the Nanodroid-patcher zip file and flash it.
Reboot the phone.
Make sure the module "NanoDroid Patcher" is enabled in the Magisk module section. If not, enable it and reboot again.
If you want, you can use [Signature Spoofing Checker](https://f-droid.org/de/packages/lanchon.sigspoof.checker/) to check, if signature spoofing is enabled now.
## Install Nanodroid 
Again, open the Magisk module section and click **Install from storage** again. This time select the Nanodroid-full-package and flash it.
Reboot again, check if it is enabled and if not enable it and reboot again.
## Remove Stock Google Services
To remove the stock google services and use MicroG in future, enter the following commands (and grant root access, if requested):
```
adb shell
su
nanodroid-overlay -a GmsCore
nanodroid-overlay -a GoogleServicesFramework
reboot
```
The phone will reboot now.

Open Magisk manager again and proceed to MagiskHide. Search for the entries "microG DroidGuard Helper", "microG Services Core" and "microG Services Framework Proxy". Click on the name (not the checkbox!) of each item mentioned and disable the appearing sliders from all those apps.
Reboot again.
Open the MicroG settings app, do the self check and grant all requested permissions and optimizations.
There is a [bug](https://github.com/microg/GmsCore/issues/1011) in the current MicroG version.
As a workaround you can put com.google.android.gsf on the deviceidle whitelist:
```
adb shell
su
dumpsys deviceidle whitelist +com.google.android.gsf
```
Delete all data from Google Play Store in the settings of the app.
Last step to finish MicroG setup: Open Google Play and login to your google account.


# Fixed: Get apps auto-starting after boot
The BV9900 Pro software has a bug with autostarting apps and widgets after boot. If you use an alarm clock and the phone is rebooted, the alarm clock will not ring any more.
Also most messanger apps won't work properly. This could be fixed by setting ro.freeme_freemanager and ro.hct_autostart_manager to 0 in build.prop. Instead of editing buid.prop directly, we will use the Magisk plug-in "MagiskHide Props Config".
Go to the module section of Magisk manager again and install "MagiskHide Props Config" from the Magisk repository.
Reboot the mobile phone.
Check if MagiskHide Props Config is enabled. If not, enable it and reboot again.

Open a root shell on the phone again:
```
adb shell
su
```
Now let's set the first property correct by executing
``
props ro.freeme_freemanager 0
``
The plug-in will ask, at which state the property should be set. Choose _2 - post-fs-data_.
Now continue with the second property by running:
``
props ro.hct_autostart_manager 0
``
and choose _2 - post-fs-data_. again.

# Fixed: Apps are not working properly or killed regularly
On the phone are two task killers installed, which kill background apps regularly. If an app got killed, it is not possible to receive any messages any more or make any notifications. Even SMS do not get delivered any more, if the SMS app gets killed.
## Duraspeed
The first task killer is called duraspeed. This app does not have an launcher icon and is killing other apps in the background.
If you do not get notifications Durapseed may has killed the application. Sometimes duraspeed kills the SMS app und no SMS will be delivered any more.
The easiest way is to deactivate it. Go to settings, search for _duraspeed_ and click _Open_. Here you can deactivate the app completely, or put other apps on the whitelist.
## Another Taskkiller
There is a second taskkiller installed on the BV9900 Pro. Open any App and swipe with your finger from the button of the screen to the top of the screen and then to the right.
At the bottom you will find two buttons. The left one looks like mechanical tools. Press it! A window called "white list" will appear. Here you can disable the second task killer completely or put some apps to the white list. I recommend to deactivate it completely.

# Debolating
If you want to debloat your phone, continue [here](/debloat.md).

# Other common bugs and fixes
There are some more bugs users are facing while working with the phone.
I collected them [here](other%20bugfixes.md).
