# Small Installation Instruction for newbies (like me) who want to use KDE.

## Install Debian
(All steps also work on Debian based distros like Ubuntu)
### Boot Stick
#### On Windows
- Download the Debian ISO
- Insert a USB Stick
- Download and start [Rufus](https://rufus.ie/de/)
- Use Rufus to load a bootable Debian iso on the stick


### Boot with the boot stick
- Enter the `Boot menu` using F12 (The exact key might differ) on startup
- Start the stick in the `Boot menu`
- Install Debian based on the steps with `Install` or `Graphical Install`

## Install Graphics card drivers
### Nvidia
Install NVidia Drivers with following Guide https://wiki.debian.org/NvidiaGraphicsDrivers

!!!WARNING!!!
Check that SecureBoot is off, else follow the instructions on https://wiki.debian.org/NvidiaGraphicsDrivers about SecureBoot

### AMD
Coming soon...


## Configure Drive Mount on start with fstab

- `sudo blkid` lists all your partitions and drives.
- call `sudo blkid <name>` e.g. <name> => '/dev/sda2' to see information about that drive.
- copy the 'UUID'.

Now we use that UUID in fstab.
- call `sudo nano /etc/fstab` and insert a new line as following:
    - `UUID=<UUID>    /media/<driveName>  <filesystem>    defaults    0   0`
    - So for example you want to mount a drive as 'scrax' with 'ntfs' (default windows filesystem afaik), you would write
    - `UUID=<UUID>    /media/scrax    ntfs    defaults    0   0`

Perform that for every drive you want to mount on start (adding external drives, that might be disconnected on start might cause a delay in loading the drives and therefore also delays the startup. The default timeout is 90 seconds)



## Programs
### VSCode
Download a .deb file from https://code.visualstudio.com/Download and run it with `sudo apt install <code>.deb`

