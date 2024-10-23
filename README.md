# Things to do after installing Fedora

Why would you install Fedora out of all 69 distro of Linux? Well..
- It's stable (unlike Arch Linux)
- It doesn't push snaps (unlike Ubuntu)
- The kernel and the drivers are up to date (unlike Debian)

## Install DNF5

Fedora by default uses the `dnf` package manager which is written in **Python** Which explains why its really slow when you need it.

The recent version `dnf5` is available and will be the default in Fedora 41. For today with Fedora 40 all we need to do is install it alongside dnf.

```sh
sudo dnf in dnf5
```

From now on when installing packages, use dnf5 instead of the dnf command.

Keep in mind for advanced features you still need to use regular dnf since dnf5 isn't very ready for advanced tasks like:
- Adding a repository
- Locally installing an rpm package
- Swapping a package with an alternative
- Installing a package group (dnf group install)
- Searching a package in the repos
- etc.

## Update your system and restart

You will have around 1000 packages to update, happens every time on a fresh Fedora install. 
Simply run:

```sh
sudo dnf5 update
```

If you get a kernel update as well, it's time to **reboot** afterwards. Even if that's not the case it is a good idea to reboot after a big update.

## Add RPM Fusion repositories

Fedora is an open source software oriented OS by default. So it wont have non-free repositories for you to install software from. But there are repos called **RPM Fusion repositories** which cover these repos.

Follow the link to [Fedora Docs](https://docs.fedoraproject.org/en-US/quick-docs/rpmfusion-setup/) for the source info.

And run these both commands:

```sh
# RPM Fusion Free Repository
sudo dnf install \
  https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm

# RPM Fusion Nonfree Repository
sudo dnf install \
  https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```


## Install NVIDIA drivers & Enable Hardware Acceleration

Fedora by default will use fully open source drivers, this is not an issue for AMD GPUs since the open source driver is superior. But for NVIDIA GPUs, open source drivers are not so ready, so we are going to install the proprietary driver.

The source for commands below are found at [RPM Fusion Docs](https://rpmfusion.org/Howto/NVIDIA)

After reading, install the packages below with this command:

```sh
# The NVIDIA driver
sudo dnf5 in akmod-nvidia 

# For CUDA/NVDEC/NVENC support (NVENC is important!!!)
sudo dnf5 in xorg-x11-drv-nvidia-cuda 

# For hardware acceleration with NVIDIA (VAAPI)
sudo dnf5 in nvidia-vaapi-driver libva-utils vdpauinfo

# For hardware acceleration with Intel iGPU
sudo dnf5 in intel-media-driver
```


## Install Multimedia Codecs

Fedora will have some codecs out of the box but it will not be enough for daily usability standards. So we install multimedia codecs

```sh
# Swap woke codec to working codec  
sudo dnf swap ffmpeg-free ffmpeg --allowerasing

# Install additional multimedia codecs
sudo dnf group install Multimedia
```

> This is necessary for being able to play various types of video and audio.
