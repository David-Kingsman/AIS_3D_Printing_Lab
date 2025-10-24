# BambuStudio Linux Compile Guide

## Overview

This guide provides instructions for compiling BambuStudio on Linux systems, specifically for Ubuntu, Fedora, and Solus distributions. BambuStudio is the official slicing software for Bambu Lab 3D printers.

**Reference**: [BambuStudio GitHub Repository](https://github.com/bambulab/BambuStudio)

## 1. Environment Setup

### 1.1 Ubuntu

Install the following tools before building:

```bash
sudo apt-get install cmake clang git g++ build-essential libgl1-mesa-dev m4 libwayland-dev libxkbcommon-dev wayland-protocols extra-cmake-modules pkgconf libglu1-mesa-dev libcairo2-dev libgtk-3-dev libsoup2.4-dev libwebkit2gtk-4.0-dev libgstreamer1.0-dev libgstreamer-plugins-good1.0-dev libgstreamer-plugins-base1.0-dev gstreamer1.0-plugins-bad libosmesa6-dev nasm yasm libx264-dev
```

**Note**: `gstreamer1.0-plugins-bad` provides the h264 decoder in gstreamer for liveview functionality.

### 1.2 Fedora

Install the following tools before building:

```bash
sudo dnf install -y https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
sudo dnf install gcc gcc-c++ cmake
sudo dnf install mesa-libGL-devel mesa-libGLU-devel
sudo dnf install m4
sudo dnf install perl
sudo dnf install extra-cmake-modules
sudo dnf install wayland-devel wayland-protocols-devel libxkbcommon-devel
sudo dnf install gtk3-devel webkit2gtk3-devel
sudo dnf install gstreamer1-devel gstreamer1-plugins-base-devel
sudo dnf install gstreamer1-plugin-openh264 #for linux liveview decoder requirement
sudo dnf install mesa-libOSMesa-devel
sudo dnf install nasm yasm x264-devel #for ffmpeg
```

### 1.3 Solus

Install the following tools before building:

```bash
sudo eopkg it -c system.devel
sudo eopkg it git llvm-clang libglvnd-devel wayland-devel libxkbcommon-devel wayland-protocols-devel extra-cmake-modules libcairo-devel libgtk-3-devel libsoup-devel libwebkit-gtk-devel gstreamer-1.0-devel gstreamer-1.0-plugins-base-devel gstreamer-1.0-plugins-good gstreamer-1.0-plugins-bad gstreamer-1.0-plugins-bad-devel mesalib-devel libx11-devel glew-devel curl-devel
```

## 2. Building the Dependencies

You need to build the dependencies of BambuStudio first. (Only needed for the first time)

**Setup Instructions:**

1. Suppose you download the codes into `/home/username/work/projects/BambuStudio`
2. Create a directory to store the dependencies: `/home/username/work/projects/BambuStudio_dep`
   (Please make sure to replace `username` with the one on your computer)

```bash
cd BambuStudio/deps
mkdir build
cd build

cmake ../ -DDESTDIR="/home/username/work/projects/BambuStudio_dep" -DCMAKE_BUILD_TYPE=Release -DDEP_WX_GTK3=1
make -jN  # N can be a number between 1 and the max cpu number
```

### JPEG Version Compatibility

**Note**: This will use libjpeg with version 80 by default. If your system's gstreamer has a different jpeg version, try to add `-DJPEG_VERSION=N` (N=6 or 7 or 8).

For example, if your gstreamer uses libjpeg version 6:

```bash
cmake ../ -DDESTDIR="/home/username/work/projects/BambuStudio_dep" -DCMAKE_BUILD_TYPE=Release -DDEP_WX_GTK3=1 -DJPEG_VERSION=6
```

**How to check the libjpeg version used by gstreamer:**

1. Go to your gstreamer library directory (e.g., Fedora at `/lib64/gstreamer-1.0`, Ubuntu at `/lib/x86_64-linux-gnu/gstreamer-1.0/`)
2. Execute `ldd libgstjpeg.so` and find the `libjpeg.so.x` used
3. If x is 62, then set `JPEG_VERSION` to 6
4. If x is 7, then set `JPEG_VERSION` to 7
5. If x is 8, then set `JPEG_VERSION` to 8 or not set

**Note**: This flag only affects the liveview of P1P. If you don't care about this function, just skip it.

## 3. Building BambuStudio

Create a directory to store the installed files:

```bash
cd BambuStudio
mkdir install_dir
mkdir build
cd build

cmake .. -DSLIC3R_STATIC=ON -DSLIC3R_GTK=3 -DBBL_RELEASE_TO_PUBLIC=1 -DCMAKE_PREFIX_PATH="/home/username/work/projects/BambuStudio_dep/usr/local" -DCMAKE_INSTALL_PREFIX="../install_dir" -DCMAKE_BUILD_TYPE=Release
cmake --build . --target install --config Release -jN  # N can be a number between 1 and the max cpu number
```

## 4. Using Scripts (Ubuntu and Fedora)

Currently, we support building using scripts under Ubuntu and Fedora:

```bash
cd BambuStudio
./BuildLinux.sh -u  # install dependency packages
./BuildLinux.sh -dsi  # build and create appimage
```

The binary and AppImage will be generated.

## Verification

These steps have been verified on Ubuntu and Fedora.

For more Linux platforms or script building, you can find details or help at [BambuStudio build for Linux](https://github.com/bambulab/BambuStudio) (Thanks to all the contributors).

## Features

BambuStudio includes the following key features:

- **Basic slicing features & GCode viewer**
- **Multiple plates management**
- **Remote control & monitoring**
- **Auto-arrange objects**
- **Auto-orient objects**
- **Hybrid/Tree/Normal support types, Customized support**
- **Multi-material printing and rich painting tools**
- **Multi-platform (Win/Mac/Linux) support**
- **Global/Object/Part level slicing parameters**

## License

BambuStudio is licensed under the GNU Affero General Public License, version 3. BambuStudio is based on PrusaSlicer by PrusaResearch.

## Support

- **GitHub Repository**: [https://github.com/bambulab/BambuStudio](https://github.com/bambulab/BambuStudio)
- **Issues**: Report issues on the GitHub tracker if they aren't already present
- **Documentation**: See the wiki and documentation directory for more information

---

*Last Updated: 2024*
*For AIS Construction Lab, McGill University*
