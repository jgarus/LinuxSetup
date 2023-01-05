Getting linux set up for gaming

### Steam
The following troubleshooting steps are used if using the flatpak version of Steam.

#### Steam flatkpak wiki
https://github.com/flathub/com.valvesoftware.Steam/wiki


#### How to fix games closing as soon as they are launched
This a *Vessel* compatibility issue with proton. *CompatibilityTool version of Proton must be installed.*
* in terminal run `flatpak search steam` and install either **GE version** or the **community version** (community usually more stable)


#### How to fix tray icon not appearing
* First check if com.valvesoftware.Steam is in location */home/\<your username>\/.var/app/* (**tested on KDE Neon**):
  * If it is in there, you can try installing an icon pack to fix the issue.
   * If it is **NOT**, run this command: `ln -s ~/.var/app/com.valvesoftware.Steam/.local/share/Steam ~/.local/share/Steam`




### <br><br>Monitoring system stats
To monitor temps and resource usage download **Mangohud** and **Goverlay**

#### Mangohud installation
* https://github.com/flightlessmango/MangoHud
* In terminal enter `flatpak search steam`
    - Find *...Utility/Mangohud* in the list
    - Install it by running `flatpak install flathub com.valvesoftware.Steam.Utility.MangoHud`

#### Goverlay installation
Two methods of doing this
1. Easiest method is to follow instructions here https://github.com/benjamimgois/goverlay
2. Or download/`wget` from http://ftp.us.debian.org/debian/pool/main/g/goverlay/. Check site for updates to packages.

<br>Method 2:
* Once downloaded run `dpkg --install` on the downloaded deb package
  > If there's dependency issues you can try to run `apt --fix-broken install`

  > If dependencies are not installed using above command, you can try to install the dependencies separately which are usually listed in the terminal error log.
    The most common missing dependencies are `mesa-utils` and/or `vulkan-tools`.
    In terminal run `sudo apt get ` on `mesa-utils` and/or `vulkan-tools`

#### Goverlay configuration
1. Run goverlay, and configure it to your liking and **save**. Steam will read this file when games are run.
2. Enable **Globally Enable** button and restart
3. copy the config file from `/home/<your username>/.config/Mangohud` to `/home/<your username>/.var/app/com.valvesoftware.Steam/config/MangoHud/`


### <br><br>Optional
#### How to mount another drive
Useful when planning to keep games in a separate drive. This will also keep the drive available even after restarting PC.
1. You will need the *name*, *UUID*, and file system *type* of drive. In terminal do `sudo blkid`
2. Create a mount point for drive `sudo mkdir /mnt/<name-of-your-drive>`
3. Edit **fstab** file (run this in a seperate terminal window) run `sudo nano /etc/fstab`
4. Enter the data displayed from *blkid* command
    * Make sure to seperate these using *tab* **ONCE** not spaces
    * `UUID=<your uuid>  <mount-point> <file-system-type>    <mount-option>  <dump>  <pass>`
    * The following example has standard default `file-system-type`, `mount-option`, `dump`, and `pass`, so use these
      * Example: `UUID=123-456-789-0123    /mnt/<name-of-your-drive>    ext4    defaults    0   2`
5. Exit nano
    * when done editing file hit *escape* then *ctrl+x*. Enter `y` when asked if you want to save, press enter again
6. See if it works
    * run `sudo mount -a`


#### How to give Steam (flatpak version) access to mounted drive
This will allow Steam to download games to that newly mounted drive
1. In terminal run `flatpak override --user --filesystem=/mnt/<your-fstab-mounted-drive> com.valvesoftware.Steam`
    * You will now be able to add this drive in your Steam Library Folders in *settings > downloads*

### Keychron setup
This will make the fn keys as second, meaning f keys will act as f keys and fn will provide the secondary function
 * During usage of keyboard, make sure switch is set to *Win/Android*
1. Create a config file if it does not exist by typing 
  * `sudo nano /etc/modprobe.d/hid_apple.conf`

2. Enter the following text
  * `options hid_apple fnmode=2`

3. Hit *Ctlr+x* to prompt saving, hit *y* when prompted and press *enter*

4. Close all applications and enter the following command (Linux will reboot)
  * On Debian based distros - `sudo update-initramfs -u && reboot`
  * On Fedora - `sudo dracut --regenerate-all --force`

5. Once rebooted, fn keys should be secondary - if it does not work, try the following
  * Hold the following keys together until the keyboard stops flashing red: *fn+x+l*
      * do this twice - test them
        * First time makes fn keys as first (Apple-like)
        * Second time makes fn keys second (Windows/Linux-like)

### Android Studio flatpak version terminal fix
In terminal options, replace value in **Shell Path** with desired terminal `flatpak-spawn --host --env=TERM=xterm-256color zsh`
