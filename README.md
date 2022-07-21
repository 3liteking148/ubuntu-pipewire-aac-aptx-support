# Pipewire Bluetooth plugin with AAC and aptX support for Ubuntu 22.04

## Getting Started
Pipewire is partially pre-installed in Ubuntu 22.04, and it can replace PulseAudio using [this tutorial](https://ubuntuhandbook.org/index.php/2022/04/pipewire-replace-pulseaudio-ubuntu-2204/).

### Prerequisites
Add universe and multiverse repositories (needed for codecs).

```
sudo add-apt-repository universe
sudo add-apt-repository multiverse
sudo apt update
```

### Installation
The .deb package can be downloaded in the releases section.

```
sudo dpkg -i path/to/libspa-0.2-bluetooth_0.3.48-1ubuntu1_amd64.deb

# Optional: prevent updates
sudo apt-mark hold libspa-0.2-bluetooth
```

## Building
### Setting up a chroot jail (optional)
#### Creating a chroot jail
```
mkdir chroot-jail
sudo debootstrap jammy chroot-jail

# build fails without mounting /proc
sudo mount -t proc /proc /chroot-jail/proc

sudo chroot chroot-jail /bin/bash
```

#### Preparing the chroot jail
```
root:/# export LC_ALL=C
root:/# apt install devscripts equivs software-properties-common wget
root:/# add-apt-repository universe multiverse && apt update
```

### Building pipewire
NOTE: Instructions are for pipewire_0.3.48

```
# Download pipewire sources
wget http://archive.ubuntu.com/ubuntu/pool/main/p/pipewire/pipewire_0.3.48-1ubuntu1.dsc
wget http://archive.ubuntu.com/ubuntu/pool/main/p/pipewire/pipewire_0.3.48.orig.tar.bz2
wget http://archive.ubuntu.com/ubuntu/pool/main/p/pipewire/pipewire_0.3.48-1ubuntu1.debian.tar.xz

# Extract sources
dpkg-source -x pipewire_0.3.48-1ubuntu1.dsc
cd pipewire-0.3.48

# Apply patch
patch -p1 < path/to/enable-aac-and-aptx-codecs.patch

# Install dependencies
sudo mk-build-deps -i

# Build pipewire (unsigned .deb)
debuild -us -uc
```



