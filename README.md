# Optimization of Blackview BV9900 Pro
The Blackview BV9900 Pro is a very good outdoor smartphone with very good hardware specifications.
Unfortunately the software is not running very well. This is a guide, how I got my phone properly working and got rid of most software proplems.
**Anyhow, I am not responsible, if you will damage your phone! You are continuing at your own risk! You will lose the ability of OTA updates and your warranty may be void!**
By the way, all your data will be lost when unlocking the bootloader! You should have a backup in place!

If you follow this guide, you should get a well working rooted BV9900 Pro with MicroG (via Nanodroid) and mostly debloated.
In this guide I am working with official **EEA** firmware version BV9900Pro_EEA_S900AA_V1.0_20201105V07.
Blackview uploaded this version to MEGA and it can be found [here](https://mega.nz/file/vLAnzQDb#AASpRKEQwCOhKTNXtxAPVX2nocLnCZgUdgeFxwDfzg4).
Remember, do not trust any random links! Check the [source and link to non-EEA-version here](https://bbs.blackview.hk/viewtopic.php?f=300&t=538469&start=170#p999471).
Before you flash this firmware, dial \*#8615# and check, if your fingerprint shows _Goodix_ or _sunwave_. **If it shows _Goodix_ stop now** and do not follow this guide any more, since fingerprint will stop working, if you upgrade to Android 10 and I am not sure, if this guid will work for Android 9!

If you have some improvement suggestions or questions, you can open an issue / pull request or let me know in [this thread on xda-developers](https://forum.xda-developers.com/t/root-microg-bugfixes-for-bv9900-pro.4270995/).

# Prerequirements
Make sure you have a working installation of adb and fastboot!
If you use Ubuntu, you can easily install it by running
```
sudo apt -y install adb fastboot
```
If you are using Windows, maybe you also have to install the MTK drivers.

# Install SP-Flash tool
First of all, you should install [SP-Flash tool](https://spflashtool.com/download/) for upgrades and backups and have the right Scatter file by hand (which can be found in the firmware [or here](resources/MT6779_Android_scatter.txt)).
If you want to backup your device, open option.ini and in the _\[ReadBack\]_ section change _ShowByScatter_ to true:

```
[ReadBack]
ShowByScatter=true
```

# Preparations for backing up the data partition
_If you do not want to include your data partition in your backups, you can safely skip this section._

The default scatter file will not cover the whole data partition. If you do a dump with default Scatter file, only a few bytes will be saved and the recovered partition won't be readable.
If you want to backup the data partition open the Scatter file, search for line 
```
partition_name: userdata
```
and proceed further to the next line starting with ```partition_size:```. This should be at line number 745.
Usually it is given the value ```0xc0000000``` there. Change this line to 
```
partition_size: 0x1BD7400000
```
Save the scatter file and you are ready to include the data partition in your backup.

<sub>_Credits for this section goes to [f1cscari](https://github.com/do1fmd/BV9900Pro-optimization/issues/3)_</sub>


# Backup (Recommended but not mandatory)
Switch off your phone and open SP Flash tool. In Download section open the correct scatter file and move to the section "Readback".
Leave out partitions without partition name. For each partition with partition name you should give a recognisable filename to the backup file (e.g. _partitionname_.img) by double-click the partition. In the first window enter the filename. For the second window just press ESC. Do not change any values here!
Afterwards mark the checkbox at the beginning of each partition. Again leave out partitions with empty partition name and click the "Read Back" button.
Now connect the switched-off phone to PC <s>while holding the Vol-Up button</s>.
The readback will take some time.


# Update Firmware (Not mandatory, if Android 10 is already installed)
You can now update (and also downgrade) the firmware of your BV9900 Pro with SP Flash tools.
I recommend [this guide](https://www.getdroidtips.com/flash-stock-firmware-using-sp-flash-tool/), if you want to flash a new firmware.
If you want to install an inofficial or modified firmware, you have to [unlock the bootloader first](/magisk+microg.md#unlock-bootloader).

# Root BV9900 Pro with Magisk and get rid of Google Services by installing Nanodroid with MicroG
If you want to Root your phone, you can follow [this guide](/magisk+microg.md). There you also have the instructions how to get rid of Google Services and replace them by MicroG.


# Debloat
If you want to debloat your phone, continue [here](/debloat.md).

# Common bugs and fixes
There are some bugs users are facing while working with the phone.
I collected the bugs and fixes [here](bugfixes.md).

Unfortunately I could not fix all bugs, yet. The still known problems are collected [here](to-fix.md).
