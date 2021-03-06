# Setting Up the Raspberry Pi 2

In order to use the RPi2 with openaps development tools, the RPi2 must have an operating system installed and be set up in a very specific way. There are two paths to the intial operating system instalation and WIFI setup.  Path 1 is reccomended for beginners that are very new to using command prompts or "terminal" on the Mac.  If you choose Path 1, it is recomended that you purchase your RPi2 as a Cana Kit, which includes  Path 2 is considered the most convenient approach for those with more experience with coding and allows the RPi2 to be set up without the use of cables, which is also known as a headless install. Either path will work and the path you choose is a matter of personal preference.

Forth the Path 1 GUI install you will need:

* A Raspberry Pi 2 Cana Kit, which includes several essential accessories in one package (See e.g. http://www.amazon.com/CanaKit-Raspberry-Complete-Original-Preloaded/dp/B008XVAVAW)
* USB Keyboard 
* USB Mouse 
* A TV or other screen with HDMI input  

For the Path 2 Headless install, you will need:

* Raspberry Pi 2
* 8 GB micro SD Card [and optional adapter so that you can plug in the micro SD Card into your computer]
* Low Profile USB WiFi Adapter
* 2.1 Amp USB Power Supply
* Micro USB cable 
* Raspberry Pi 2 Cana Kit

Path 1: GUI Setup

1.:  **Plug in Applicable peripherals**
First, insert your USB keyboard and USB mouse into the RPi2.  Then insert the included USB WIFI into the RPi2.  Next, connect your RPi2 to a monitor or T.V. using the included HDMI cable.  Next, insert the Micro SD Card included with your Cana Kit into the RPi2.  Finally connect your RPi2 using the power adapter.  You should see the GUI appear on sceen.  

2.:  **Install Raspbian and Connect WIFI**
At this point you can consult the color instruction pamphlet included with your Cana Kit, which will walk you through installing Raspbian and connecting the RPi2 to your WIFI router.  

Once you have installed Rasbian and connected to WIFI, you can disconnect the mouse, keyboard and HDMI cable.  You can now skip to Step #4 and SSH into your RPi2.  Remember to keep your RPi2 plugged in, just disconnect peripherals.  Also remember to never disconnect your RPi2, without shutting it down properly using the sudo shutdown -h now command


1.: **Getting Raspbian**
<br>Raspbian is the recommended operating system for OpenAPS. You can download the latest version of Raspbian [here](http://downloads.raspberrypi.org/raspbian_latest). Make sure to extract the ZIP file.

2.: **Writing Raspbian to the Micro SD Card**
<br>Please read this [excellent guide](http://elinux.org/RPi_Easy_SD_Card_Setup). Please view the sections on flashing the SD card using Windows, Mac OS X, or Linux, depending on which operating system you use.

3.: **Configuring WiFi Settings**
<br>_(a note for Mac users: You cannot access EXT4 partitions without using 3rd party software. The easiest alternative it is to temporarily connect RPi to a router with and ethernet cable, SSH in (see below), and continue setting things up in /etc/network/interfaces to get the wifi running.)_
<br><br>
Keep the SD card in the reader in your computer. In this step, the WiFi interface is going to be configured in Raspbian, so that we can SSH in to the RPi2 and access the device remotely, such as on a computer or a mobile device via an SSH client, via the WiFi connection that we configure. Go to the directory where your SD card is with all of the files for running Raspbian on your RPi2, and open this file in a text editor.
<br><br>
`/path/to/sd/card/etc/network/interfaces`
<br><br>
Edit the file so it looks like this: 
<br>

```
auto lo
iface lo inet loopback
iface eth0 inet dhcp

auto wlan0
allow-hotplug wlan0
iface wlan0 inet dhcp
wpa-ssid <your-network-name>
wpa-psk <your-password>
```

Replace `<your-network-name>` and `<your-password>` with your own credentials. Save the file (without adding any additional extensions to the end of the filename).
<br><br>
Now, put the SD card into the RPi2. Plug in the compatible USB WiFi adapter into a RPi2 USB port. Get a micro USB cable and plug the micro USB end into the side of the RPi2 and plug the USB side into the USB power supply. 
<br><br>
4.: **Testing SSH Access**

**Windows:** Make sure that the computer is connected to the same WiFi router that the RPi2 is using. Download PuTTY [here](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html). Hostname is `pi@raspberrypi.lan` and default password for the user `pi` is `raspberry`. The port should be set to 22 (by default), and the connection type should be set to SSH. Click `Open` to initiate the SSH session.

**Mac OS X:** Make sure that the computer is connected to the same WiFi router that the RPi2 is using.

Open Terminal and enter this command:

`ssh pi@raspberrypi.lan`

Default password for the user `pi` is `raspberry`

**Linux:** Make sure that the computer is connected to the same WiFi router that the RPi2 is using.

Open Terminal and enter this command:

`ssh pi@raspberrypi.lan`

Default password for the user `pi` is `raspberry`

**iOS:** First, you need to make your phone a hotspot and configure the WiFi connection in `step 3` to use the hotspot. Make sure that the iOS device is connected to the same WiFi network that the RPi2 is using. Download Serverauditor or Prompt 2 (use this if you have a visual impairment). Hostname is `pi@raspberrypi.lan` and the default password for the user `pi` is `raspberry`. The port should be set to 22 (by default), and the connection type should be set to SSH. 

**Android:** First, you need to make your phone a hotspot and configure the WiFi connection in `step 3` to use the hotspot. Make sure that the Android device is connected to the same WiFi network that the RPi2 is using. Download an SSH client in the Google Play store. Hostname is `pi@raspberrypi.lan` and the default password for the user `pi` is `raspberry`. The port should be set to 22 (by default), and the connection type should be set to SSH.

**Note:** If connecting to the RPi2 fails at this point, the easiest alternative it is to temporarily connect RPi to router with ethernet cable, and SSH in, given both the computer and the RPi2 are connected to the same router.

5.: **raspi-config**

Run

`sudo raspi-config` 

to expand filesystem, change user password and set timezone (in internalization options)

6.: **Password-less login**

Secure your RPi2. Log out by executing

`exit`

and copy your public SSH key into your RPi2 by entering

`ssh-copy-id pi@raspberrypi.lan`

Now you should be able to log in without a password. Repeat `step 4` and try to SSH into the RPi2 without a password.

**Don't have an SSH key?** Follow this guide from GitHub to obtain one. [Link](https://help.github.com/articles/generating-ssh-keys/)

7.: **SSH configuration**

Since we have no password, we need to disable password login. Open the `sshd_config` file in nano text editor as follows

`sudo nano /etc/ssh/sshd_config`

Change the following

```
PermitRootLogin yes
# PasswordAuthentication yes
```

to

```
PermitRootLogin no
PasswordAuthentication no
```

(note that the second line was previously commented out).

From now on you will be able to SSH in with your private SSH key only.

8.: **Update** 
<br>Update the RPi2.

`sudo apt-get update && sudo apt-get upgrade`

Be patient while the packages install.

9.: **Watchdog**
<br>Now we are going to install watchdog, which restarts the RPi2 if it becomes unresponsive.

Enter in:

`sudo apt-get install watchdog`

Then enter:

`sudo modprobe bcm2708_wdog`

Then enter this line to open up the following file:

`sudo nano /etc/modules`

At the bottom of the file add

`bcm2708_wdog`

Add watchdog to startup applications

`sudo update-rc.d watchdog defaults`

Edit its config file by opening up nano text editor

`sudo nano /etc/watchdog.conf`  <br />
<br />

Uncomment the following: (remove the # from the following lines):

```
max-load-1
watchdog-device
```

Start watchdog by entering

`sudo service watchdog start`
