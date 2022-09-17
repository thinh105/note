---
id: 5ts6g3iswb2d5vexqa22z2q
title: Ubuntu
desc: ''
updated: 1663409164560
created: 1663409164560
isDir: false
title_imported: Ubuntu
updated_imported: '2022-04-12T03:46:57.000Z'
created_imported: '2019-10-30T16:03:21.000Z'
---

[TOC]

































# [disk usage - File system is 100% full - Ask Ubuntu](https://askubuntu.com/questions/405060/file-system-is-100-full)



# Okular
```
sudo apt install okular
```

## Set Okular dark theme follow the Gnome setting

```
sudo apt install qt5-style-plugins
```

and adding `QT_QPA_PLATFORMTHEME=gtk2` to `/etc/environment`

Restart the machine


# Install new fonts

Unpack fonts to 
`~/.local/share/fonts`
(or `/usr/share/fonts`, to install fonts system-wide)
and
`fc-cache -f -v`



# Error when installing in Ubuntu Store : `Unable to install “<PACKAGE>”: snap “<PACKAGE>” has “install-snap” change in progress`

```
$ snap changes

123  Doing   2018-04-28T10:40:11Z  -  Install "foo" snap
```
You can abort ongoing change(s):
```
sudo snap abort 123
```


# Change suspend time
Using dconf-editor

`org → gnome → settings-daemon → plugins → power`

When plugged in change the `sleep-inactive-ac-timeout option`

When on battery change `sleep-inactive-battery-timeout`.
# Restart to UEFI bios
	
Ubuntu
```
systemctl reboot --firmware-setup
```

Windows
```
shutdown /r /fw
```
# Kill Process in some port -  Error: listen EADDRINUSE: address already in use 0.0.0.0:3001



```
kill -9 $(lsof -t -i:6969)

#6969 is the port
```

# Kill a process with a phrase in its name

```js
pkill -9 chrome
```

# [Kill process](https://askubuntu.com/questions/13441/how-to-kill-applications)

Xkill : `Ctrl` + `ALt` + `Backspace` to kill process visually 

`Ctrl` + `Alt` + `F2`

```bash
killall <application_name>
killall chrome

kill -9 -1 #kill all process
```

# Mouse Scroll
http://www.webupd8.org/2015/12/how-to-change-mouse-scroll-wheel-speed.html

# VLC hotkey




# Update Joplin
```
wget -O - https://raw.githubusercontent.com/laurent22/joplin/master/Joplin_install_and_update.sh | bash
```

## Custom CSS Joplin

Rendered markdown can be customized by placing a userstyle file in the profile directory 
```
~/.config/joplin-desktop/userstyle.css
```
(This path might be different on your device - check at the top of the Config screen for the exact path).

This file supports standard CSS syntax. Joplin must be restarted for the new css to be applied, please ensure that Joplin is not closing to the tray, but is actually exiting.

Note that this file is used for both displaying the notes and printing the notes. Be aware how the CSS may look printed (for example, printing white text over a black background is usually not wanted).

Editor styles can be customized by placing a custom editor style file in the profile directory
```
~/.config/joplin-desktop/userchrome.css
```

# Sửa lỗi không thể delete file ổ đĩa ntfs ngoài
Vô windows set quyền cho everyone

# Sửa lỗi ổ ngoài ko delete vào Trash được
The solution was opening Disks, clicking on the NTFS partition -> on the little gears icon underneath (Additional partition options)-> "Edit Mount Options" and adding "uid=1000" (no quotes, separated with a comma) to the line above the Mount Point (see picture).Modifying fstab mount options through the Disk utility

![](https://i.stack.imgur.com/UBppU.png)

uid should be set to an alternative number from 1000 as returned by the "id" command from the terminal if you are not the original user, as mentioned here.

[source](https://askubuntu.com/questions/75154/cannot-move-file-to-trash-warning-when-trying-to-delete-a-file-in-nautilus/516825#516825)

# Backup toàn bộ Ubuntu
1. backup installed packages using Aptik
    
    For those wanting a nice. neat GUI...
...introducing Aptik.

All you need is a backup directory, stored locally or in the cloud. Aptik will backup PPAs, downloaded packages, software selections, application settings and themes and icons. Very useful.

You can install it through the ppa:

```
sudo apt-add-repository ppa:teejee2008/ppa
sudo apt-get update
sudo apt-get install aptik
sudo apt-get install aptik-gtk
```
Hope this helps out :)

https://askubuntu.com/questions/9135/how-to-backup-settings-and-list-of-installed-packages

2. backup Dash-to-Panel setting - Manual
3. backup VSCode - Settings Sync Addon
    https://mikefrobbins.com/2019/03/21/backup-and-synchronize-vscode-settings-with-a-github-gist/
4. Backup setting app - Mackup

    https://vitux.com/how-to-backup-application-settings-in-ubuntu-using-mackup/

5. Shortcut keyboard
    + Restart | `systemctl reboot` | Shift + Ctrl + ~
    + Shutdown | `systemctl poweroff` | Shift + Ctrl + Escape
    + Suspend | `systemctl suspend` | Ctrl + Escape
    + Flameshot | `flameshot gui` | print
    + xkill | `xkill` | Ctrl + Alt + Backspace

6. Save Gnome setting

![e25efbb6f538d527930d92a6ab8a0778.png](/assets/e25efbb6f538d527930d92a6ab8a0778-cuzk96wew214.png)

From the terminal, execute the following to save your gnome settings:

```
cd ~
dconf dump / > saved_settings.dconf
```
Keep the saved_settings.dconf file in a safe place so you can use it after you upgrade.

From the terminal, execute the following to restore your gnome settings:

```
cd ~
dconf load / < saved_settings.dconf
```
(I suggest doing a test to make sure this works for you: Save the settings as shown above in step 1. Then make a few test changes using Gnome Tweaks. Finally restore the settings, as shown above in step 2. If everything restores to the way you had it before, you can use the saved file to restore settings after you upgrade.)

7. For the reinstalltion of your packages, build and restore a list of installed packages.

To build the list of packages installed on your system the packages use:

```sudo dpkg --get-selections > package.list```

To restore the packages use:

```
sudo dpkg --set-selections < package.list

sudo apt-get dselect-upgrade
```


    
# What editor can I use as a simple vi/vim alternative?

nano


# Create a bootable USB stick

Search and install `Startup Disk Creator`
![](https://tutorials.ubuntu.com/es6-bundled/src/codelabs/tutorial-create-a-usb-stick-on-ubuntu/img/1fa9b46af539b512.png)


https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-ubuntu#1

# Lỗi khi unzip ubuntu

```bash
sudo nautilus
```

# Hotkey 

## Control the window application
    
## `Super` + `↑` &nbsp;/&nbsp; `↓`  : to Maximize &nbsp;/&nbsp; Unmaximize

## `Alt`+`Space` + `Space`   : to Minimize a menu

##  `Super` + `←` &nbsp;/&nbsp; `→` : &nbsp; Move to the left/right of monitor

## `Shift` + `Super` + `←` &nbsp;/&nbsp; `→`  : Move the window to &nbsp;/&nbsp;  monitor



## Workspace

## Moving between workspace : `ctrl` + `alt` + `↑` &nbsp;/&nbsp; `↓`

## Moving app to another workspace : `ctrl` + `shift` + `alt` + `↑` &nbsp;/&nbsp; `↓`

## Overview the workspace: &nbsp; `Super` 


![fee11005539efbcd10560b3c11b796e5.png](/assets/fee11005539efbcd10560b3c11b796e5-jbqcte46jao2.png)


