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
- My recommendation is using KDE as the
- If you logged in, you should add your user to the sudoers file
  - open the konsole / terminal
  - run `su` to login as `root`
  - run `sudo adduser <username> sudo` to add your user to the sudoers file
  - exit the session `exit`

## Install Graphics card drivers

### Check your system

use `lscpu` to see, what kind of system you have.
If the field `Vendor ID:` equals `GenuineIntel` (or `AuthenticAMD` on AMD CPU), then you have a AMD64 system.

I can't provide any information to the other systems. The guides that I linked to usually handle those.

### Nvidia

[Ensure whether you use a AMD64 system or not](#check-your-system)

Install NVidia Drivers with following Guide https://wiki.debian.org/NvidiaGraphicsDrivers.

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

Most programs can be obtained using the built in `Discover` application.

### Games

It's necessary to disable the composer for gaming. Therefore I suggest the [Autocomposer Extension](#autocomposor) in the Discover store. It disables the Composer automagically as soon as you open a fullscreen application and therefore boosts performance.

You should also enable the performance mode

```bash
sudo echo performance | sudo tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
```

Edit the "performance" string to powersave to restore powersave mode.

### Autocomposor

Find the Autocomposer [here](https://store.kde.org/p/1502826)

### VSCode

Download a .deb file from https://code.visualstudio.com/Download and run it with `apt install <code>.deb` as root

### Discord

- Download the Installer as '.deb' file https://discord.com/api/download?platform=linux&format=deb
- run `apt install ./discord<...>.deb` as root

### Steam

- This should be done already, if the base setup is done with help of this guide
  - add the following line to `/etc/apt/sources.list` (`sudo nano /etc/apt/sources.list`): `deb http://deb.debian.org/debian/ bookworm main contrib non-free`
- Enable Multiarch `sudo dpkg --add-architecture i386`
- `sudo apt update`
- `sudo apt install steam-installer`
- `sudo apt install mesa-vulkan-drivers libglx-mesa0:i386 mesa-vulkan-drivers:i386 libgl1-mesa-dri:i386`
- Run the `Steam installer`

might be necessary:

- `sudo apt install nvidia-driver-libs:i386`
- `sudo apt-get upgrade steam -f`
- `sudo apt install libgl1-mesa-dri:i386 libgl1:i386`

### Docker

https://docs.docker.com/engine/install/ubuntu/

#### It might be necessary to replace the os release

For further info, check out the [Docker Website](https://docs.docker.com/engine/install/debian/#installation-methods)

```bash
$(. /etc/os-release && echo "$VERSION_CODENAME")
```

#### Setup Docker

Set up Docker's apt repository

```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

Install the Docker packages

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Verify that the Docker Engine installation is successful by running the hello-world image.

```bash
 sudo docker run hello-world
```

### CUDA

https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=Debian&target_version=12&target_type=deb_network

Debian 12 Guide:

- wget https://developer.download.nvidia.com/compute/cuda/repos/debian12/x86_64/cuda-keyring_1.1-1_all.deb
- sudo dpkg -i cuda-keyring_1.1-1_all.deb
- sudo add-apt-repository contrib
- sudo apt-get update
- sudo apt-get -y install cuda-toolkit-12-5
- To install the open kernel module flavor:
  - sudo apt-get install -y nvidia-kernel-open-dkms
  - sudo apt-get install -y cuda-drivers
