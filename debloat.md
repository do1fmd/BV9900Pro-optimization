# Debloating
The BV9900 Pro comes with over 200 Apps preinstalled.
Of course, not all of them are necessary. So, I decided to uninstall some of them.
I want to point out again, that this instructions are made for Firmware version BV9900Pro_EEA_S900AA_V1.0_20201105V07.

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
This method is only working, if you installed Nanodroid as described in my guide.

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
If you do not root access or do not have Nanodroid installed or simply do not want to use the overlay feature, you can also uninstall the unwanted system apps for user 0.
I took the other way, so I cannot guarantee this way will work. But it is quite common for other devices, too.
The drawback is, that apps cannot be reinstalled that easy.

Let's do it! Login to a shell on your mobile. Root access is not needed:
```
adb shell
```
Now you can copy paste the following command list to the terminal and the apps in the list will get "removed".
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
