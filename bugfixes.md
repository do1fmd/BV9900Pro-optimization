# Well know bugs and fixes
Here you will find some well known bugs and fixes, which do not affect all customers.
Disclaimer: I am not responsible for any damage to your device, if you follow these steps here!

## MyFLIR does not start any more
If MyFLIR App does not start any more, wipe all data of this app.

## Some strange Chinese letters appear on the top right area
This is usually connected to Google Tee Key. Simply uninstall the system app _Factorymode_.
Open a shell on your mobile (``adb shell``) and run the following command:
```
pm uninstall --user 0 com.mediatek.factorymode
```
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


