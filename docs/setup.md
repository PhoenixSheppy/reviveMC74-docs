# Inital Setup:
Well, we have to start somewhere right? Let's start with the basics

#### What do you need?
- A copy of the repository in a local directory:
<br/><code>git clone https://github.com/reviveMC74/reviveMC74.git</code>
<br/><code>cd reviveMC74</code>
- A USB-A <-> USB-A cable [(Amazon)](https://www.amazon.com/s?k=USB+A+to+USB+A&ref=nb_sb_noss_2)
- A Meraki MC74 Cloud Phone (A-duh, that's why you're here)
- A way to power the phone, either via PoE (Power over Ethernet) or 12v adapter
- A computer with Python3 installed, preferibly Python27 for compatability reasons
> I Personally recommend installing Debian in a VM and passing the ADB interface through to it. I've found it easier to get the dependancies listed in the installer this way. The directions to get a VM with Debian running are outside the scope of this guide, but you can google it.
- The dependancies listed in the installer:
    - [UNPACKBOOTIMG/MKBOOTIMG/CPIO/CHMOD](https://github.com/huaixzk/android_win_tool)
    - [GZIP/GUNZIP](https://sourceforge.net/projects/unxutils/files/unxutils/current/UnxUtils.zip/download)
    > Extract `usr/local/wbin/gunzip.exe` and `usr/local/wbin/gzip.exe` from that download and place in a PATH directory.
- A reasonable amount of experience with [ADB](https://developer.android.com/studio/command-line/adb) (Android Debug Bridge)

#### Preparing the phone
After you've gathered the required components / software, you need to perform the following to get started:
><em>These steps put the phone into fastboot, allowing you to flash a custom recovery.</em>

1. Remove the USB cable from the side of the MC74 (If you've already connected it previously.)
2. Remove the ethernet/PoE cable from the LAN port on the back of the MC74
3. Reconnect the USB cable to the right-side USB port of the MC74 (Not the back) and the other end to your PC
> Before you move onto the next step, be prepared to do the following after continuing:
    1. Press and hold the mute button (before the backlight flashes)
    2. Keep pressing it until the Cisco / Meraki logo appears and the vibrator audibly sounds
4. Apply power with your PoE ethernet cable to the LAN port (It looks like a LAN icon, not the PC icon port.)
5. Release the mute button after step 3-B occurs.
> If the phone boots like normal, you've done something wrong and must try again. Please return to step 1.

#### Option 1: Running the script

Now that the phone is ready, we can finally run the script!
<br>BUT, hold your horses, we have a few ways of doing it depending on how you installed Python:

From inside the `reviveMC74-main` directory, type:
<br>- `C:\python27\python.exe revive-Mc74.py`
<br>- Or `python revive-Mc74.py`
<br> It should run as desired, and give a few more prompts. Just follow them as directed. 
<br>When finished, you should have a modded MC74!
<br>Open the app drawer and tap `wPhone` and configure as desired.
> Don't worry if something breaks when running the script, not everyone's phone is built the same, so the script may fail in some places and succeed in others. The following options are for you!

#### Option 2: replaceRecovery Failure

Let's say the script failed after flashing recovery and now your phone is just sitting there in CWM, there's a way to manually do the rest of the work.

My first phone failed here, and I had to do a bunch of tinkering to the `/system` partition to get it to allow access to ADB. Let's skip that and flash a pre-built image instead that I built from my phone:

1. Download the [following](https://mega.nz/file/sYgnXATK#JszO_wV1flV9aX145y60SIpWhyvpk6lezr3BFoyHgCg) `.zip` file which is a backup of my `/system` partition, preconfigured to allow ADB access upon boot, and already has the Meraki dialer pre-uninstalled.
1. From CWM, press `install zip from sideload`
2. In your ADB shell type `adb sideload {directory/to/downloaded/zip}`
3. After it pushes and restores, wipe `/data` and `/cache` to prevent potential issues.
4. Reboot to system.
5. When it boots, it may look like it's "bootlooping" but it should be operational, ensure the device has vibrated as that is an indicator of a successful boot.
6. Then, run the script with the following arguments: `python revive-Mc74.py installApps`
7. That should install (and uninstall if I missed anything) the custom dialer, led controls, and button redirections.
8. From there, just press and hold the mute button and it should prompt you to pick a home application, choose TeslaLauncher and be sure to hit `Always`.
9. And finally, from the app drawer run `wPhone` to get started on setting up your SIP softphone.

> Again, don't worry if that doesn't work either, there's always an option 3.

#### Option 3: installApps Failure

So, you've got CWM flashed to recovery, you've flashed my custom `/system` backup, or you've managed to get through the script up until this point, and now you're stuck:

This is how my second phone failed, and here's how I manually did the work:

1. Ensure you have ADB access to the phone (It's booted, and sitting on the Cisco Meraki screen, or you're on the homescreen.)
2. from the `/installFiles` subdirectory, run the following:
    1. `adb install com.teslacoilsw.launcher-4.1.0-41000-minAPI16.apk`
    2. `adb install -r -t revive.MC74-debug.apk`
    3. `adb install revive.SSMService-debug.apk`
    4. `adb root`
    5. `adb remount`
    6. `adb copy hex /system/bin/hex`
    7. `adb chmod 755 /system/bin/hex`
    8. `adb copy lights /system/bin/lights`
    9. `adb chmod 755 /system/bin/hex`

From there, you should be able to reboot the phone, and then run an application called `wPhone` from the launcher drawer. 