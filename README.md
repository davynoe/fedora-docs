# Things to do after installing Fedora 41

Why would you install Fedora out of all 600 distros of Linux? Well..
- It's stable (unlike Arch Linux)
- It doesn't push snaps (unlike Ubuntu)
- The software, kernel and drivers are up to date (unlike Debian)

## Update your system and restart

You will have around 1000 packages to update, happens every time on a fresh Fedora install. 
Simply run:

```sh
sudo dnf update
```

If you get a kernel update as well, make sure to **reboot** afterwards. Even if that's not the case it is a good idea to reboot after a big update.

## Add RPM Fusion repositories (Nonfree software)

Fedora only provides free and open source software by default. So it wont have nonfree repositories for you to install software from. But there are repos called **RPM Fusion repositories** which cover these repos.

Follow the link to https://docs.fedoraproject.org/en-US/quick-docs/rpmfusion-setup/ for the source info.

And run these both commands:

```sh
# RPM Fusion Free Repository
sudo dnf install \
  https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm

# RPM Fusion Nonfree Repository
sudo dnf install \
  https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```


## Install NVIDIA drivers & Enable Hardware Acceleration (For NVIDIA users)

Fedora by default will use fully open source drivers, this is not an issue for AMD GPUs since the open source driver is superior. But for NVIDIA GPUs, open source drivers are not so ready, so we are going to install the proprietary driver.

The source for commands below are found at https://rpmfusion.org/Howto/NVIDIA

After reading, install the packages below with this command:

```sh
# The NVIDIA driver
sudo dnf5 in akmod-nvidia 

# For CUDA/NVDEC/NVENC support (NVENC is important!!!)
sudo dnf in xorg-x11-drv-nvidia-cuda 

# For hardware acceleration with NVIDIA (VAAPI)
sudo dnf in nvidia-vaapi-driver libva-utils vdpauinfo

# For intel iGPU users
sudo dnf in intel-media-driver
```

## Install Multimedia Codecs

Fedora will have some codecs out of the box but it will not be enough for daily usability standards. So we install multimedia codecs
Reference from https://rpmfusion.org/Howto/Multimedia
```sh
# Swap woke codec to working codec  
sudo dnf swap ffmpeg-free ffmpeg --allowerasing

# Install additional multimedia codecs
sudo dnf group install Multimedia

# If the command above doesn't work
sudo dnf update @multimedia --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin
```

> This is necessary for being able to play various types of video and audio.
