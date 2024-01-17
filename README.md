# steamdeck-scripts
A collection of scripts, tools and documentation related to SteamOS
___
## Backup and restore commands

#### Create an empty backup drive
* Open KDE Partition Manager
* Select the drive and click New Partition Table
* chose GPT and save
* Select the partition and select New / File System / ext4 and label it "SteamDeckBackup"

#### Backup to this backup drive
Run the following command from Konsole:
sudo rsync -aAXv /home/deck /run/media/deck/SteamDeckBackup/backup

#### Restore from this backup drive
Run the following command from Konsole:
sudo rsync -aAXv /run/media/deck/SteamDeckBackup/backup/deck /home/deck
___

## Utilities
### Decky Loader
Decky Loader adds a storefront to GameMode that can be used to install several tools for customizing featurs of SteamOS.
#### Instalation
* install decky plugins from https://decky.xyz/
* open the store in GameMode to install needed plugins
* Install plugins below
* restore config files from backups into ~/?????

#### Main Plugins
* TabMaster - Customize Library Tabs  
* CSS Loader - Customize UI for SteamOS
* DeckMTP - Enable USB data transer to and from PC
* SteamGridDB - Customize artwork for non-steam games
* Decky Recorder - Save videos of gameplay easily
* Bash Shortcuts - Allow execution of Bash scripts from GameMode

### ProtonUP
ProtonUp can be used to download specific versions of Proton for use running Windows games.
#### Instalation
* Install ProtonUp from discover store
* download any versions of Proton needed

### Common Redistributables
This is a collection of installers for the most common libraries needed by some Windows games. 
#### Instalation 
* Extract CommonRedist.zip from PC Backup into a folder in ~/downloads
* If a non-steam game will not run due to missing libraries, use Lutris WineTools to install required libraries
___

## Game Installers / Launchers
Below is a list of the game launchers and installers that I have used. Each has it's merits but I think that Lutris may be the only one needed. Try it first and add others if needed.

### Lutris Launcher
Lutris allows for the creation of new wine prefixes.Games or apps can be added to the prefix via an installer application or simply pointing to the already installed executable. 
Lutris also can link to several Game stores, allowing you to browse your library and install any games you have.

#### Installation
* Install Lutris from the discover store
* 
### Heroic Launcher
Heroic Launcher allows access to non-steam game libraries.  
#### Installation
* install Heroic Launcher from the discover store
* setup non-steam accounts
* update settings to add new games to Steam library

### itch Launcher
This app lets you download and install games from itch.io and add them to your Steam Library
#### Installation
* download from http://itch.io/app
* run the downloaded file
* launch the app and log into your itch.io account
* download and install any games you want
___



## Scripts

Here is what I use on my desktop Linux to switch between single screen and dual screen modes. It is bound to a key via the custom shortcuts in KDE settings. Hitting the key will toggle between the two modes. (Not tested on Steam Deck.)

```bash
#!/bin/bash

if xrandr -q | grep -q "HDMI-0 connected primary"
then
    # Currently in single screen mode. Switch to double screen mode.
    xrandr --output "DVI-D-0" --mode 1920x1080 --primary --pos 0x0 \
           --output "HDMI-0" --mode 1920x1080 --pos 1920x0
else
    # Currently in double screen mode. Switch to single screen mode.
    xrandr --output "DVI-D-0" --off \
           --output "HDMI-0" --primary --mode 1920x1080 --pos 0x0
fi

# Restart Plasma due to funky behavior after reconfiguring displays
plasmashell --replace >/dev/null 2>&1 &
```
___
The following can be saved as a .desktop file. This file can be added to steam as a non-steam game and will then execute the internal script when launched.

An example of such a file could look like this:
```
#!/usr/bin/env xdg-open
[Desktop Entry]
Name=Name You Want To See
Comment=Whatever You Like Here
Exec=/home/deck/path/to/your/script.sh
Type=Application
Encoding=UTF-8
Terminal=false
Categories=None;
```
This file will display as "Name You Want To See" and when you attempt to open it, it will run the script.sh you specify. In my case, I wrote it up to run rsync in the direction that I want (I have one for each direction - On To Deck, Off Of Deck), with the appropriate options to completely clobber the files, delete files in the destination not in the source, etc. Basically "make my files the new authoritative ones". But you could do whatever you like here, including additional pre/post work to fix things up as necessary.

Alternatively, you can use the Bash Shortcuts plugin above and execute your script file from there.
___
## zenity for displaying GUIs from bash
[zenity documentation](https://help.gnome.org/users/zenity/2.32/zenity-usage.html.en)

### Examples
Show a popup from inside a shell script using zenity.

`zenity --info --title "Hello World" --text "This script just ran!" --width 300 2>/dev/null`

Prompt for a file
```bash
#!/bin/sh

FILE=`zenity --file-selection --title="Select a File"`

case $? in
  0)
    echo "\"$FILE\" selected.";;
  1)
    echo "No file selected.";;
  -1)
    echo "No file selected.";;
esac
```

___
##Links
https://github.com/TheRealAlexV/steamdeck-scripts

https://gist.github.com/pudquick/981a3e495ffb5badc38e34d754873eb5

