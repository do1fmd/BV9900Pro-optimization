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
## Apps are not working properly or killed regularly
[Solution here](/README.md#fixed-apps-are-not-working-properly-or-killed-regularly)
