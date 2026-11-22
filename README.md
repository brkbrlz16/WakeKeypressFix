## **DISCLAIMER**
I don't know the origin of this script or who owns it. I found it in [Andrej-Antipov](https://github.com/Andrej-Antipov)'s [repo](https://github.com/Andrej-Antipov/X99-K9-Machinist-Hackintosh) 4 or 5 years ago and I used it from Big Sur through Monterey and Ventura. The original script doesn't seem to have received any updates beyond Ventura support. With the help of Claude AI, I updated it, and it now solves the issue on my configuration running macOS Tahoe.

The script is provided as is and has only been tested on my own laptop. Tested under macOS Tahoe 26.7.1, with proper S3 sleep working on a Huawei MateBook 16 (AMD Ryzen 7 5800H).

## **The Issue**
Normally, macOS can transition from DarkWake to Full Wake (display on) on its own if it detects a wake-capable event, such as an HID event. On Hackintoshes, however, this may not always happen. In such cases, the system gets stuck in DarkWake until a wake-capable event is triggered manually (e.g., by pressing a key or clicking a mouse or touchpad input). If no input is detected, the system goes back to sleep.
Pressing power button on your laptop will probably not turn on your display and system will get stuck in DarkWake because PWRB is not defined as wake-capable in ACPI. In such case, system will probably need a key press or a touchpad click for the display to turn on and then system transitions from DarkWake to Full Wake. 

On laptops, opening the lid always triggers a wake-capable HID event. Therefore, when the lid is opened, the system can transition from DarkWake to Full Wake without any additional user input, and the display should turn on automatically.
If your keyboard is connected over an internal USB port, you might be able to solve the issue once and for all using [USBWakeFixup](https://github.com/osy/USBWakeFixup).

This script watches for sleep/wake events. It doesn't trigger if Power Nap wakes the system up for routine tasks where system goes from Sleep to DarkWake. But, if the script detects a real wake event initiated by the user it runs "caffeinate -u" to declare a user input and wakes the display up.

## **Installing**
1- Unzip, then run "xattr -cr WakeUpScr.app" to clear the download quarantine. The app is unsigned.

2- Double-click it and look for the "Wake Helper STARTED" dialog.

3- If Tahoe shows a "Background Items Added" notice, allow it under System Settings → General → Login Items & Extensions.

## **Troubleshooting**
If the display gets stuck at dark screen (or if the system hangs/crashes) after wake:

1- Force a reboot

2- Stop the script by double clicking it 

3- Start the script again by double clicking it and then wait for a few seconds

4- Paste one of these commands below into terminal, wait for ~30 seconds and then try sleep-wake cycle again to see if display wakes up.

echo "fix 1" > ~/Library/Application\ Support/WakeUpScr/mode

echo "fix 2" > ~/Library/Application\ Support/WakeUpScr/mode

echo "fix 3" > ~/Library/Application\ Support/WakeUpScr/mode

echo "fix 4" > ~/Library/Application\ Support/WakeUpScr/mode

Fix 3 means the script will wait for 3 seconds before sending the wake event for the display. 
Don’t delete the “WakeUpScr.app”. You’re going to need it if you ever want to/need to stop the script. Stopping the script will automatically delete everything related to it.
