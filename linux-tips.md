Table of Contents
=================

   * [Table of Contents](#table-of-contents)
   * [General](#general)
      * [Configuring GnuPG Keyservers](#configuring-gnupg-keyservers)
      * [Copying files into parent and children subfolders](#copying-files-into-parent-and-children-subfolders)
      * [Generating ToCs for markdown files!](#generating-tocs-for-markdown-files)
      * [Processes](#processes)
         * [Killing processes or jobs](#killing-processes-or-jobs)
      * [Tree-view using "ls"](#tree-view-using-ls)
         * [For directories and files](#for-directories-and-files)
         * [For directories only](#for-directories-only)
   * [Arch Linux](#arch-linux)
      * [Installation](#installation)
         * [Pre-installation](#pre-installation)
            * [Fixing the "no public key" issue during the pre-installation phase](#fixing-the-no-public-key-issue-during-the-pre-installation-phase)
         * [Mount the file systems](#mount-the-file-systems)
   * [Ubuntu/Debian-based](#ubuntudebian-based)
      * [Ubuntu/Mint Tips](#ubuntumint-tips)
      * [Customisation](#customisation)
         * [1. Changing Windows Cursors for Ubuntu](#1-changing-windows-cursors-for-ubuntu)
         * [2. Changing the lockscreen background (Ubuntu 20.04)](#2-changing-the-lockscreen-background-ubuntu-2004)
         * [3. DeadBeeF Custom Layout](#3-deadbeef-custom-layout)
         * [4. Getting Apps to Obey System Themes (Using Qt5 and GTK-2)](#4-getting-apps-to-obey-system-themes-using-qt5-and-gtk-2)
         * [5. Creating custom terminal commands](#5-creating-custom-terminal-commands)
         * [6. Adding a dock to your second monitor](#6-adding-a-dock-to-your-second-monitor)
      * [Shortcuts (Ubuntu 20.04)](#shortcuts-ubuntu-2004)
         * [1. Moving Windows between Workspaces](#1-moving-windows-between-workspaces)
         * [2. Custom Key Mappings](#2-custom-key-mappings)
      * [Printing SysInfo](#printing-sysinfo)
         * [Ubuntu 17.04 and up](#ubuntu-1704-and-up)
         * [Ubuntu 16.10 and below](#ubuntu-1610-and-below)
      * [Installing Packages](#installing-packages)
         * [Installing Android Studio](#installing-android-studio)
         * [Installing fonts](#installing-fonts)
      * [Missing System Tray Icons?](#missing-system-tray-icons)
      * [Wireless Devices](#wireless-devices)
         * [Auto-connect Bluetooth Devices](#auto-connect-bluetooth-devices)
         * [Low volume on wireless headphones?](#low-volume-on-wireless-headphones)
      * [Problems/Errors when running sudo apt update and sudo apt upgrade](#problemserrors-when-running-sudo-apt-update-and-sudo-apt-upgrade)
         * [Skipping acquire of configured file](#skipping-acquire-of-configured-file)

***Credits go to their respective repo owners, contributors, and maintainers for providing solutions to issues I've come across. This repo only serves as a mini-library of the questions and answers I've found useful and would like to compile them into a list.***

# General

This section is meant for all Linux distros and their derivatives!

## Configuring GnuPG Keyservers

1. This assumes you have installed or already have GnuPG on your system. You could verify the installation by typing this `gpg --version` into a terminal!

2. Open a terminal and type `sudo apt-get install gnupg-curl` for 16.04 and earlier or `sudo apt install gnupg-curl` for 18.04 and later. 

3. Changing the working directory and downloading the SKS CA (Certificate Authorities) by typing this `cd ~/.gnupg; wget https://sks-keyservers.net/sks-keyservers.netCA.pem`

4. (OPTIONAL) Verify the CA by going to [this website](https://sks-keyservers.net/verify_tls.php) and getting the OpenPGP key. Note that you could continue verifying it if needed, but [read this post](https://superuser.com/a/1362835) to see why you don't have to. 

5. Add this line `keyserver hkps://hkps.pool.sks-keyservers.net` to `~/.gnupg/gpg.conf` by typing in the terminal `gedit ~/.gnupg/gpg.conf`. 

6. (FOR GnuPG 1 ONLY) Add this line `keyserver-options ca-cert-file=/home/.../.gnupg/sks-keyservers.netCA.pem` to `~/.gnupg/gpg.conf`.

7. (FOR GnuPG 2 ONLY) Add these lines to `~/.gnupg/dirmngr.conf`: (remember to replace `...` with your username!)

   ```bash
   keyserver hkps://hkps.pool.sks-keyservers.net
   hkp-cacert /home/.../.gnupg/sks-keyservers.netCA.pem
   ```

8. Then, type `gpg --recv-keys 4AA4767BBC9C4B1D18AE28B77F2D434B9741E8AC` to verify the keyserver was configured correctly.

9. If that doesn't work, then try searching for and adding the key manually by typing this:

   ```bash
   curl 'http://pgp.surfnet.nl:11371/pks/lookup?op=get&search=4AA4767BBC9C4B1D18AE28B77F2D434B9741E8AC' > name.key
   gpg --import name.key
   ```

10. Enjoy! :)

11. You could refer to [this website](https://riseup.net/en/security/message-security/openpgp/best-practices#use-the-sks-keyserver-pool-instead-of-one-specific-server-with-secure-connections) for **really** detailed instructions!

[Go to Top](#Table-of-Contents)

## Copying files into parent and children subfolders

`find . -type d -exec cp filename.ext {} \;`

Replace 'filename' and 'ext' with the proper filename! Like so:

`find . -type d -exec cp helloworld.txt {} \;`

Breakdown of the above: 

1. `find` is a itemsearch utility
2. `.` denotes the current directory
3. `-type` acts as a filter for item types
4. `d` denotes a directory
5. `-exec` is short for "execute"
6. `cp` is short for "copy"
7. `{}` indicates (or contains*) the results from the `find` expression. 
8. `\` is the common "escape" symbol. In this case the backslash is used to allow `;` to be parsed by the `-exec` flag.

[Go to Top](#Table-of-Contents)

## Generating ToCs for markdown files!

1. Fire up a terminal and paste this in: 

   `wget https://raw.githubusercontent.com/ekalinin/github-markdown-toc/master/gh-md-toc`

2. Then this:

   `chmod a+x gh-md-toc`

3. Then in your desired dir, paste this in:

   `~/gh-md-toc ~/path/to/your/file.md`

4. Copy and paste the resulting output into your `markdown` file and enjoy :)

[Go to Top](#Table-of-Contents)

## Processes

### Killing processes or jobs

1. Fire up a terminal and find the troublemaker's process ID using ps: 
`ps -eaf | grep process_name`
2. Kill it:
`kill process_id`

*or*

1. Find a process using pgrep:
    `pgrep processname`

2. Kill the process (in the case of multiple processes): 

  `kill process_id`

*or* 

1. If the job is the only one running, i.e only one "rclone" is running, then: 
    `pkill process_name`

[Go to Top](#Table-of-Contents)

## Tree-view using "ls"

### For directories and files

1. Paste this into that window you opened!

   `find . -print | sort | sed 's;[^/]*/;|---;g;s;---|; |;g'`

### For directories only

1. Paste this!

   `ls -R | grep ":$" | sed -e 's/:$//' -e 's/[^-][^\/]*\//--/g' -e 's/^/   /' -e 's/-/|/'`

*Will update this section with a breakdown of how it works when I understand bash better.

[Go to Top](#Table-of-Contents)

# Arch Linux

## Installation

### Pre-installation

#### Fixing the "no public key" issue during the pre-installation phase

1. On Debian-based operating systems, GnuPG is already installed as part of the distro. GnuPG for modern systems kinda takes away the User ID of each key so issues may occur. To fix this, we have to import the public key `4AA4767BBC9C4B1D18AE28B77F2D434B9741E8AC` mentioned in the terminal output when trying to run this command  `gpg --keyserver-options auto-key-retrieve --verify archlinux-version-x86_64.iso.sig`.

2. Download the Arch Linux .iso file and its corresponding PGP key from the [downloads page](https://www.archlinux.org/download/).  

3. Open up a terminal, and type this `gpg --keyserver pool.sks-keyservers.net --recv-keys 4AA4767BBC9C4B1D18AE28B77F2D434B9741E8AC` to import the Arch Linux public key.

4. Then type this `gpg --keyserver-options auto-key-retrieve --verify archlinux-version-x86_64.iso.sig`, replacing `archlinux-version-x86_64.iso.sig` with the downloaded signature filename to verify the file's signature. 

5. Once this is done the output should be similar to this: 

   ```bash
   gpg: assuming signed data in 'archlinux-2020.05.01-x86_64.iso'
   gpg: Signature made Fri 01 May 2020 13:33:57 +08
   gpg:                using RSA key 4AA4767BBC9C4B1D18AE28B77F2D434B9741E8AC
   gpg: Good signature from "Pierre Schmitz <pierre@archlinux.de>" [unknown]
   gpg: WARNING: This key is not certified with a trusted signature!
   gpg:          There is no indication that the signature belongs to the owner.
   Primary key fingerprint: 4AA4 767B BC9C 4B1D 18AE  28B7 7F2D 434B 9741 E8AC
   ```

6. You're done!

### Mount the file systems 

1. To create the EFI mount point, type this `mkdir -p /mnt/efi` in the terminal.
2. Then to mount the directory, type this `mount /dev/sda1 /mnt/efi`. 

[Go to Top](#Table-of-Contents)

# Ubuntu/Debian-based

## Ubuntu/Mint Tips

Goto [easylinuxtips](https://easylinuxtipsproject.blogspot.com/p/1.html) and follow the guides.

## Customisation

### 1. Changing Windows Cursors for Ubuntu

1. Find the .cur and .ani files you want to use.
2. Put them in a folder.
3. Open this: [CursorCreate](https://github.com/isaacrobinson2000/CursorCreate/releases)
4. Alternatively, open this: CursorCreateWindows.zip from the Utilities folder (either external harddrive or from sakae-crypt or kenji-crypt) OR this folder [CursorCreate](/home/tatsuei/Downloads/CursorCreate)
5. Run the program according to the README file.
6. Use the template provided for a quickstart.
7. ***Use the latest release for newer Ubuntu distros***, if not you could try the older releases too!

[Go to Top](#Table-of-Contents)

### 2. Changing the lockscreen background (Ubuntu 20.04)

***Note: GDM3 won't accept filenames with spaces!!! Change filenames to include dashes ( - ) or underscores ( _ ) to work with filenames with spaces :)***

1. In a terminal, type `sudo apt install libglib2.0-dev-bin` and press Return/Enter.
2. Then type `curl -L -O github.com/thiggy01/ubuntu-20.04-change-gdm-background/raw/master/ubuntu-20.04-change-gdm-background` or `curl -L -O github.com/ttsuei/ubuntu-20.04-change-gdm-background/raw/master/ubuntu-20.04-change-gdm-background` if the first one doesn't work anymore.
3. Type ` chmod +x ubuntu-20.04-change-gdm-background` to make the script executable.
4. set the wallpaper with `sudo ./ubuntu-20.04-change-gdm-background /location/of/wallpaper.jpg` ***Change "wallpaper" to the name of your image!
5. It would ask if you would like to restart the GDM service, so type `Y`. If it doesn't, type `systemctl restart gdm.service` to restart it manually.
6. Wait for a bit and BOOM! You got yoself a nice new lockscreen :D

[Go to Top](#Table-of-Contents)

### 3. DeadBeeF Custom Layout

[deadbeef Arc Darker Theme](/home/tatsuei/Downloads/deadbeef-arc-darker-theme.txt)

[Go to Top](#Table-of-Contents)

### 4. Getting Apps to Obey System Themes (Using Qt5 and GTK-2)

1. Open up a terminal and run this first: `sudo apt-get update`
2. Then type this once that's done: `sudo apt-get install qt5-style-plugins`
3. Set all Qt5 applications to use the system theme by doing: `echo "export QT_QPA_PLATFORMTHEME=gtk2" >> ~/.profile` 
4. Once that's squared away, L O G O U T.
5. Log back in... and??? :money_mouth_face:

[Go to Top](#Table-of-Contents)

### 5. Creating custom terminal commands

1. Open a terminal window and create a .bash_aliases file in your home directory: `gedit ~/.bash_aliases`
2. Once the editor is open, add this: `alias your_alias_name='your_terminal_command; second_terminal_command'`
3. Once you're done, save the file: `Ctrl + S` and exit.
4. In the terminal window, type this: `. ~/.bashrc` to refresh your .bashrc file.
5.  Try using your new alias in terminal by typing `your_alias_name` in the terminal to confirm it works.
6. E N J O Y :smiley:

[Go to Top](#Table-of-Contents)

### 6. Adding a dock to your second monitor

1. Do check if you have `dconf-editor` installed before doing this :) 
2. Installing `dconf-editor` can be done by running this in a terminal: `sudo apt update -y && sudo apt install dconf-editor -y`
3. Change your working directory to the `/planks/docks/` folder: `cd ~/.config/planks/docks` and make a new folder: `mkdir dock2`, then copy the launcher items from your main dock: `cp dock1/launchers dock2/launchers` 
4. Then open a terminal and open the `configuration editor` by typing `dconf-editor` in the terminal
5. There's gonna be a window popping up, with a message warning you that some apps would break with modified settings. Press `"I'll be careful."`
6. In the top centre bar, press the magnifying glass and enter `/net/launchpad/plank/docks/` 
7. In the results, there will be an entry for `enabled-docks`. Click that T H A N G!
8. Click the `Use default value` switch to enable custom values, and add `,'dock2'` to `'dock1'`.
9. Once that's done, click `Apply` on the bottom left hand corner.
10. Let's take a short detour back to the terminal, and get the second monitor's name by `plank --preferences`.
11. A new window will pop up, and there will be a row with a `On Primary Display` switch. Next to it is the active monitors in a dropdown menu. Click on the dropdown menu, and it should list the displays in this manner: `eDP-1` `HDMI-X`. Usually the second monitor would be the `HDMI-X` entry. Note this down!
12. Now, click on the `plank` folder up top (upper left corner), click on `docks`, then on `dock2`. There's a buncha settings we gotta change!
13. First, you might want to change the icon size (`icon-size`)to a suitable size for your external monitor (if you're using a smaller screen like a laptop screen). Next, change the `monitor` value to reflect the value you got in step 11. Finally, change the `theme` to match your current dock theme. That's it!
14. Back to your terminal! Let's restart the `Plank` dock shall we? `pkill plank` and then `plank &` and press `Enter` or `Return` once it loads up!
15. You'll be able to see your second dock now! Enjoy :D 

[Go to Top](#Table-of-Contents)

## Shortcuts (Ubuntu 20.04)

### 1. Moving Windows between Workspaces

1. Ctrl + Alt + Arrowkey to switch between workspaces
2. Ctrl + Alt + Shift + Arrowkey to move the current window between workspaces

[Go to Top](#Table-of-Contents)

### 2. Custom Key Mappings

1. In your upper right hand corner of the screen, click on the arrow facing downwards.
2. Click the "Settings" option near the bottom of the dropdown menu which appears.
3. A new window will appear. Try scrolling through the categories on the left pane till you find "Keyboard Shortcuts" or something to that effect :stuck_out_tongue_closed_eyes:. 
4.  

[Go to Top](#Table-of-Contents)

## Printing SysInfo

1. Install `neofetch` if you haven't done yet! Here's [a link](https://github.com/dylanaraps/neofetch/wiki/Installation#universal-install) to the instructions! Or you could follow the steps below.

### Ubuntu 17.04 and up

1. Update yo repositories! `sudo apt update`
2. Get that stuff in you! `sudo apt install neofetch`

[Go to Top](#Table-of-Contents)

### Ubuntu 16.10 and below

1. Add the official PPA! `sudo add-apt-repository ppa:dawidd0811/neofetch`
2. Update yo repositories! `sudo apt update`
3. Get that stuff in you! `sudo apt install neofetch`

[Go to Top](#Table-of-Contents)

## Installing Packages

### Installing general *.deb packages using `dpkg`

1. Navigate to the folder containing the downloaded `.deb` file. E.g: `cd ~/Downloads`
2. Fire up a terminal and type this in: `sudo dpkg -i /path/to/deb/file`
3. Remember to substitute the filename for `path/to/deb/file`!

### Installing Android Studio

1. Download the latest package from the [official Android Developer website](https://developer.android.com/studio).
2. Unpack this `tar.gz` archive into `/usr/local/` for yourself or `/opt/` if you wanna get every user in on the party! Skip `step 3` if you prefer working from your current directory.
3. Fire up a terminal, and paste this in: `sudo cp "<folder_name>" /usr/local/ -r`
4. Navigate "`cd`" into `bin`. Let's perform a check before going ahead and running the installer: `chmod +rx studio.sh`. 
5. Run the installer, and choose if you want to import your previous settings, or start fresh!
6. In the new Android Studio window, click `Configure`, and select `Create Desktop Entry` for easy access from the App Dashboard.
7. Next step would be to read the `docs` or start building apps! 

[Go to Top](#Table-of-Contents)

### Installing fonts

1. Download your desired fonts, and extract them out of the ZIP file(s) if necessary.
2. Double click each of them and press *Install* for each .ttf or .otf file.
3. Open a terminal, and type `fc-cache -f -v`
4. Enjoy your new fonts!

[Go to Top](#Table-of-Contents)

## Missing System Tray Icons?

1. Install one of these two: [TopIcons Plus](https://extensions.gnome.org/extension/1031/topicons/) or [KStatusNotifierItem/AppIndicator Support](https://extensions.gnome.org/extension/615/appindicator-support/) either from source (on Github) or from the Gnome Extensions page.

[Go to Top](#Table-of-Contents)

## Wireless Devices

### Auto-connect Bluetooth Devices

**Note:**
This has been tested on my `Galaxy Buds` OS: `Ubuntu 20.04` as of 8th May 2020.

Try pairing your BT device and computer. Once they're paired, follow the steps below: 

1. Open Terminal and run `bluetoothctl`
2. The output would be something like this:

```
tatsuei@eijihaus:~$ bluetoothctl
[NEW] Controller xx:xx:xx:xx:xx:xx [default]
[NEW] Device aa:bb:cc:dd:ee:ff Galaxy Buds (4FB9)
Agent registered
[bluetooth]# 
```

3. In the above output "Galaxy Buds (4FB9)" is paired but not yet connected.. So to connect to this device we do: `connect aa:bb:cc:dd:ee:ff` at the prompt `[bluetooth]#` with `aa:bb:cc:dd:ee:ff` as your BT device address.

Example:

```
[bluetooth]# connect aa:bb:cc:dd:ee:ff
Attempting to connect to aa:bb:cc:dd:ee:ff
[CHG] Device aa:bb:cc:dd:ee:ff Connected: yes
Connection successful
[CHG] Device aa:bb:cc:dd:ee:ff ServicesResolved: yes
[Galaxy Buds (4FB9)]#
```

This means if you can run the command `bluetoothctl` and then at the `[bluetooth]#` prompt if you can input `connect aa:bb:cc:dd:ee:ff` The Bluetooth Device will connect.

So this can be done with a single command in terminal like this, after your first login open Terminal and run this command.

```
echo "connect aa:bb:cc:dd:ee:ff" | bluetoothctl
```

Example:

```
tatsuei@eijihaus:~$ echo "connect aa:bb:cc:dd:ee:ff" | bluetoothctl
[NEW] Controller xx:xx:xx:xx:xx:xx i7-4770 [default]
[NEW] Device aa:bb:cc:dd:ee:ff Galaxy Buds (4FB9)
Agent registered
[bluetooth]# connect aa:bb:cc:dd:ee:ff
Attempting to connect to aa:bb:cc:dd:ee:ff
Agent unregistered
[DEL] Controller xx:xx:xx:xx:xx:xx [default]
tatsuei@eijihaus:~$
```

so the command `echo "connect aa:bb:cc:dd:ee:ff" | bluetoothctl` is working..

This means if we can run this command at login without human  interaction.. the BT device which is paired and already turned on at the time of boot will connect in the above manual way..

------

4. `mkdir ~/bin` (Create this directory if you dont have already.. Otherwise Ignore this step)
5. `touch ~/bin/btautoconnect.sh` 
6. `gedit ~/bin/btautoconnect.sh` and paste this: 

```
#!/bin/bash

bluetoothctl
sleep 10
echo "connect aa:bb:cc:dd:ee:ff" | bluetoothctl
sleep 12
echo "connect aa:bb:cc:dd:ee:ff" | bluetoothctl
exit
```

7. Save the file, and then exit the editor.

8. Set the executable permission for this script: `chmod +x ~/bin/btautoconnect.sh`

9. Create a .desktop file named `btautoconnect.desktop` in `~/.config/autostart/` 
10. `touch ~/.config/autostart/btautoconnect.desktop` 
11. Edit the file, `gedit ~/.config/autostart/btautoconnect.desktop`, and paste the following into the file.

```
[Desktop Entry]
Type=Application
Exec=/home/tatsuei/bin/btautoconnect.sh
Hidden=false
NoDisplay=false
X-GNOME-Autostart-enabled=true
Name=BTAutoConnect
X-GNOME-Autostart-Delay=5
Comment=Connects to Galaxy Buds
```

12. Save the file, and then exit the editor.
13. Reboot your computer, and see the magic happen(?) :)

[Go to Top](#Table-of-Contents)

### Low volume on wireless headphones? 

1. **Important!!** **Remember to substitute the MAC address by going into the Settings menu and find Bluetooth devices**
2. Try this: 
    `dbus-send --print-reply --system --dest=org.bluez /org/bluez/hci0/dev_<mac_address_of_headphones> org.bluez.MediaControl1.VolumeUp`
3. If that works, skip steps 4-6 :)
4. Another fix would be: 
    `sudo gedit /lib/systemd/system/bluetooth.service`
5. Add this:
    `--noplugin=avrcp`
    to this:
    `ExecStart=/usr/lib/bluetooth/bluetoothd`
6. Then run: 
    `sudo systemctl daemon-reload`
    `sudo systemctl restart bluetooth`
7. Reconnect your headphones and enjoy!

[Go to Top](#Table-of-Contents)

## Problems/Errors when running `sudo apt update` and `sudo apt upgrade`

### Skipping acquire of configured file

1. This is something I came across with `PostgreSQL`, so feel free to change the package name to match the package producing the error.

2. Fire up a terminal and paste these commands in one after the other!

   `sudo grep -i postgresql /etc/apt/sources.list`

   *or*

   `sudo grep -i postgresql /etc/apt/sources.list.d/*.list`

3. It'll pop up with a filename along with its directory, so let's edit the file:

   `sudo -H gedit /etc/apt/sources.list` 

   *or*

   `sudo -H gedit /etc/apt/sources.list.d/found_filename.list` 

   *If you're using the second command, do remember to exclude the trailing `:deb` from the filepath.

   **Also, ignore any error messages that may pop up.

4. Once the editor is open, find the line which starts with: 

   `deb https://` *or* `deb http://`.

5. Change the above line to:

   `deb [arch=amd64] https://` *or* `deb [arch=amd64] http://`

6. Et voila! Run your `update` and `upgrade` commands without issues.

[Go to Top](#Table-of-Contents)