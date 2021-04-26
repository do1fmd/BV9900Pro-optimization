# BV9900Pro-optimization
The Blackview BV9900 Pro is a very good outdoor smartphone with very good hardware specifications.
Unfortunately the software is not running very well. This is a guide, how I got my phone properly working and got rid of most software proplems.
Anyhow, I am not responsible, if you will damage your phone! You are continuing on your own risk!

If you follow this guide, you hopefully will get a working rooted BV9900 Pro with MicroG and mostly debloated.
In this guide I am working with official firmware version BV9900Pro_EEA_S900AA_V1.0_20201105V07.
Blackview uploaded this version to MEGA and it can be found here: [Download](https://mega.nz/file/vLAnzQDb#AASpRKEQwCOhKTNXtxAPVX2nocLnCZgUdgeFxwDfzg4)
Since you should not trust any random links, I will give you the [source here](https://bbs.blackview.hk/viewtopic.php?f=300&t=538469&start=170)
Before you flash this firmware, dial \*#8615# first and check, if your fingerpritn shows _Goodix_ or _sunwave_. If it shows _Goodix_ stop now and do not follow this guide any more, since fingerprint will stop working, if you upgrade to Android 10.

# Installing SP-Flash tool
First of all, you need to install SP-Flash tool and the right Scatter file (which can be found in the firmware).
I would recommend to open option.ini and in the \[ReadBack\] section change _ShowByScatter_ to true:

```
[ReadBack]
ShowByScatter=true
```

# Unlocking Bootloader
Go to the settings, "About phone" and press the Build number at least 10 times in a row.
Now head back to the settings menu -> System. There you will find the new entry "Developer options".
Here you can unlock the bootloader by activating "OEM unlocking" and also enable "USB debugging" for later use.

# Backup
Switch off your phone and open SP Flash tool. In Download section open the correct scatter file and move on the section "Readback".
Leave out partitions without partition name. For each partition with partition name you should give a recognisable filename to the backup file (e.g. _partitionname_.img) by double-click the partition. In the first window enter the filename. For the second window just press ESC. Do not change any values here!
Afterwards mark the checkbox at the beginning of each partition. Again leave out partitions with empty partition name and click the "Read Back" button.
Now connect the phone to PC while holding the Vol-Up button.
The readback will take some time.

# Rooting BV9900 Pro
