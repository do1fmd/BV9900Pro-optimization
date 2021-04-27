# Optimization of Blackview BV9900 Pro
The Blackview BV9900 Pro is a very good outdoor smartphone with very good hardware specifications.
Unfortunately the software is not running very well. This is a guide, how I got my phone properly working and got rid of most software proplems.
Anyhow, I am not responsible, if you will damage your phone! You are continuing at your own risk!
By the way, all your data will be lost when unlocking the bootloader!

If you follow this guide, you hopefully will get a working rooted BV9900 Pro with MicroG (via Nanodroid) and mostly debloated.
In this guide I am working with official firmware version BV9900Pro_EEA_S900AA_V1.0_20201105V07.
Blackview uploaded this version to MEGA and it can be found [here](https://mega.nz/file/vLAnzQDb#AASpRKEQwCOhKTNXtxAPVX2nocLnCZgUdgeFxwDfzg4).
Since you should not trust any random links, check the [source here](https://bbs.blackview.hk/viewtopic.php?f=300&t=538469&start=170).
Before you flash this firmware, dial \*#8615# and check, if your fingerprint shows _Goodix_ or _sunwave_. If it shows _Goodix_ stop now and do not follow this guide any more, since fingerprint will stop working, if you upgrade to Android 10 and I am not sure, if this guid will work for Android 9!

# Prerequirements
Make sure you have installed and working adb and fastboot!
If you use Ubuntu, you can easily install it by running
```
sudo apt -y install adb fastboot
```

# Installing SP-Flash tool
First of all, you need to install [SP-Flash tool](https://spflashtool.com/download/) and have the right Scatter file available (which can be found in the firmware [or here](resources/MT6779_Android_scatter.txt)).
If you want to backup your device, open option.ini and in the \[ReadBack\] section change _ShowByScatter_ to true:

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
I recommend [this guide](https://www.getdroidtips.com/flash-stock-firmware-using-sp-flash-tool/), if you want to flash a different firmware.

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


# Get apps auto-starting after boot
The BV9900 Pro software has a bug with autostarting apps and widgets after boot. If you use an alarm clock and the phone is rebooted, the alarm clock will not ring any more.
Also most messanger apps won't work properly. This could be fixed by setting ro.freeme_freemanager and ro.hct_autostart_manager to 0 in build.prop. Instead of editing buid.prop directly, we will use the Magisk plug-in "MagiskHide Props Config".
Go to the module section of Magisk manager again and install "MagiskHide Props Config" from the Magisk repository.
Reboot the mobile phone.
Check if MagiskHide Props Config is enabled. If not, enable it and reboot again.

Open the shell on the phone as root again:
```
adb shell
su
```
Now let's set the first property correct by executing
``
props ro.freeme_freemanager 0
``
The plug-in will ask, at which state the property should be set. Choose _2 - post-fs-data_.
Now repeat again with
``
props ro.hct_autostart_manager 0
``
and choose _2 - post-fs-data_. again.

# Apps are not working properly or killed regularly
On the phone are two task killers installed, which kill background apps regularly. If an app got killed, it is not possible to receive any messages any more or make any notifications. Even SMS do not get delivered any more, if the SMS app gets killed.
## Duraspeed
The first task killer is called duraspeed. This app does not have an launcher icon and is killing other apps in the background.
If you do not get notifications Durapseed may has killed the application. Sometimes duraspeed kills the SMS app und no SMS will be delivered any more.
The easiest way is to deactivate it. Go to settings, search for _duraspeed_ and click _Open_. Here you can deactivate the app completely, or put other apps on the whitelist.
## Another Taskkiller
There is a second taskkiller installed on the BV9900 Pro. Open any App and swipe with your finger from the button of the screen to the top of the screen and then to the right.
At the bottom you will find two buttons. The left one looks like mechanical tools. Press it! A window called "white list" will appear. Here you can disable the second task killer completely or put some apps to the white list. I recommend to deactivate it completely.

# Debolating
Instead of deleting system apps directly, I would recommend to deactivate them in the settings screen or  uninstall them by ``pm uninstall -k --user 0`` or with Nanodroid-Overlay.
## Deactivate unwanted apps
First I will give a list, which apps can be simply deactivated in the Android settings.
If you cannot find the app (e.g. your phone is not in English), you can use the app [apps_Packages Info from F-Droid](https://f-droid.org/de/packages/com.oF2pks.applicationsinfo/).
Type in the apk name (you find them in \[brackets] in my list) and you can open the android settings of the searched app.
 
- Youtube (if you don't want to use Youtube vanced) \[com.google.android.youtube]
- (Google) Assistant \[com.google.android.apps.googleassistant]
- Home screen tips \[com.android.protips]
- Gmail \[com.google.android.gm]
- Duo \[com.google.android.apps.tachyon]
- Basic Daydreams \[com.android.dreams.basic]
- Digital Wellbeing \[com.google.android.apps.wellbeing]
- (Google) Fit \[com.google.android.apps.fitness]
- Call Log Backup/Restore \[com.android.calllogbackup]
- Google Play Movies & TV \[com.google.android.videos]
- (Google) Photos \[com.google.android.apps.photos]
- (Google) Calendar \[com.google.android.calendar]
- com.android.providers.partnerbookmarks \[com.android.providers.partnerbookmarks]
- Android Auto \[com.google.android.projection.gearhead]
- Google Keep \[com.google.android.keep]
- Google Location History \[com.google.android.gms.location.history]
- Youtube Music \[com.google.android.apps.youtube.music]

To debloat the other apps, you have two ways.
With nanodroid-overlay you can put an empty overlay over the system apps. In that way Android cannot "see" the app any more.
I took this way. The other way is described below by uninstalling the app for user 0.

Honestly, I kept some apps, which I want to use, like the calculator and Flir app.
So the lists below are far from complete, but a good start.

## Using nanodroid-overlay
This method is only working, if you installed Nanodroid as described above.

Login to a shell on your mobile and gain root access:
```
adb shell
su
```
If requested, grant root access again.
Now you can copy paste the following list to the terminal and the apps in the list will get "removed".
If you want to keep an app like FaceUnlock, you must skip this one from the list below.
```
nanodroid-overlay -a HctGameMode
nanodroid-overlay -a HctUserGuide
nanodroid-overlay -a HctBlackView
nanodroid-overlay -a HctChlidMode
nanodroid-overlay -a AdupsFota
nanodroid-overlay -a AdupsPrivacyPolicy
nanodroid-overlay -a VZWRemoteSimlockService
nanodroid-overlay -a MtkCapCtrl
nanodroid-overlay -a MDMConfig
nanodroid-overlay -a MDMLSample
nanodroid-overlay -a AutoDialer
nanodroid-overlay -a ChromeCustomizations
nanodroid-overlay -a WallpaperBackup
nanodroid-overlay -a WallpaperCropper
nanodroid-overlay -a HctNotepad
nanodroid-overlay -a DKFileTrans
nanodroid-overlay -a ATMWifiMeta
nanodroid-overlay -a ManagedProvisioning
nanodroid-overlay -a FaceUnlock
```
So far I do not have experienced any problems with these apps removed.
If you want to get back an app, you just have to use the ``-r`` instead of ``-a`` option.
Please pay attention to [these instructions](https://github.com/Nanolx/NanoDroid/blob/master/doc/NanoDroidOverlay.md)

## Using the quite common way of uninstalling apps for user 0.
If you have not installed Nanodroid or do not want to use the overlay feature, you can also uninstall the unwanted system apps for user 0.
I took the other way, so I cannot guarantee this way will work. But it is quite common for other devices, too.
The drawback is, that apps cannot be reinstalled that easy.

Let's do it! Login to a shell on your mobile and gain root access:
```
adb shell
su
```
If requested, grant root access again.
Now you can copy paste the following list to the terminal and the apps in the list will get "removed".
If you want to keep an app like FaceUnlock, you must skip this one from the list below.

```
pm uninstall -k --user 0 com.hct.gamemode
pm uninstall -k --user 0 com.hct.userguide
pm uninstall -k --user 0 com.hct.blackviewhome
pm uninstall -k --user 0 com.hct.chlidmode
pm uninstall -k --user 0 com.adups.fota
pm uninstall -k --user 0 com.adups.privacypolicy
pm uninstall -k --user 0 com.verizon.remoteSimlock
pm uninstall -k --user 0 com.mediatek.capctrl.service
pm uninstall -k --user 0 com.mediatek.mdmconfig
pm uninstall -k --user 0 com.mediatek.mdmlsample
pm uninstall -k --user 0 com.mediatek.autodialer
pm uninstall -k --user 0 com.android.partnerbrowsercustomizations.example
pm uninstall -k --user 0 com.android.wallpaperbackup
pm uninstall -k --user 0 com.android.wallpapercropper
pm uninstall -k --user 0 com.mediatek.todos
pm uninstall -k --user 0 com.blackview.filetrans
pm uninstall -k --user 0 com.mediatek.atmwifimeta
pm uninstall -k --user 0 com.android.managedprovisioning
pm uninstall -k --user 0 com.elephanttek.faceunlock
```
