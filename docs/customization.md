# Time to make your phone look pretty!

After doing all of that work, I'm sure you're dying to know if you can set custom ringtones, backgrounds, etc.
**Well, the answer is yes, absolutely!**

After completing intial configuration, (i.e. Connecting to a SIP/PBX server, ensuring phonecalls can be recieved and made, etc.) you can further customize and brand your phone in the following ways:

- You can change the ringtone to be any `.ogg` file, just use an online converter to convert it!
- You can change the background on the dialer, as well as the clock to be whatever you want, just ensure your photo is in `16:9` aspect ratio.
- You can change the sidebar shortcuts to be whatever you want; check MSN, check your favorite stock, a Grafana board, you name it.
- You can also change the incoming call behavior manually, enforcing vibration when an incoming call is recieved, or etc.

#### Changing the ringtone

To change the ringtone, perform the following steps;

1. **Ensure** your ringtone of choice is in the `.ogg` file format, as other formats are untested.
> If your ringtone is not in the `.ogg` file format, you can convert it using a tool, like [this one](https://audio.online-convert.com/convert-to-ogg).
2. Establish an ADB connection to your phone, note that the location we will be using to store your ringtone will be `/sdcard/ssm/audio/`.
3. Copy your desired ringtone from your PC to your phone using the following command: <br>`adb shell push /path/to/ringtone.ogg /sdcard/ssm/audio/ringtone.ogg`
4. Pull the configuration file from the phone using the following command: <br>`adb shell pull /sdcard/ssm/store/MC74.mp /MC74.mp`.
> Now's a great time to remove the old `MC74.nob` if you've previously modified your phone and you're updating / now customizing. <br>Run: `adb shell rm /sdcard/ssm/store/MC74.nob`
5. Open up your text editor of choice, and locate the line that says <br>`ringtone: '/system/media/audio/ringtones/Trad.ogg'` and change it to <br>`ringtone: '/sdcard/ssm/audio/yourringtone.ogg'`
6. Now push it back to the phone: `adb shell push /MC74.mp /sdcard/ssm/store/MC74.nob`.
7. Reboot your phone, *or* force-close and reopen wPhone from your home menu. Your changes should now be applied.

#### Changing the wallpaper

To change the wallpaper behind the dialer, perform the following steps;

1. **Ensure** your wallpaper is in `16:9` and at least 720p (1280x720).
2. Establish an ADB connection to your phone, note that the location we will be using to store your wallpaper will be `/sdcard/--STOP
