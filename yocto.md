# Embedded Systems Overview

## What is an Embedded System?

An **embedded system** is a combination of hardware and software designed to perform a **specific task**.  
Key characteristics:

- Equipped with a processing unit.
- Includes temporary memory and permanent storage.
- Part of a larger system.
- Customized for specific functionality.
- Lower cost compared to general-purpose systems.

Each component in such a system can be considered an embedded system when it has its own processor and memory.

---

#  Kernel Configuration: `make menuconfig` vs. Yocto Approach

When working with Linux kernel customization for embedded systems, there are two main ways to configure the kernel:

---

## . `make menuconfig` — Traditional Kernel Configuration

This is the **standard method** used when you're working directly with the Linux kernel source **outside** of Yocto.

#  What is Yocto Project?

The **Yocto Project** is an open-source collaboration project that helps developers **create custom Linux distributions** for embedded systems, regardless of the hardware architecture (e.g., ARM, x86).

> Yocto is not a Linux distribution — it's a set of tools for building your own.

---

##  What Can Be Customized in the Kernel with Yocto?

When using Yocto to build an embedded Linux image, you can **deeply customize the Linux kernel** to fit your device's requirements — from performance to memory footprint.

Here are the major kernel aspects you can customize:

###  1. **Boot Time Optimization**
- Remove unnecessary drivers or features to reduce kernel size.
- Disable debugging or logging features to speed up boot.
- Optimize init system and enable fast boot techniques like `read-only rootfs` or `initramfs`.

###  2. **Memory Usage**
- Disable memory-hungry subsystems not used by your application.
- Use lightweight libraries and a small `C` library like `musl` or `uClibc`.
- Optimize kernel page size and memory allocators (e.g., SLUB vs SLOB).

###  3. **Filesystem Type**
- Choose from multiple filesystems:
  - `ext4`, `squashfs`, `jffs2`, `ubifs`, `cpio`, etc.
- Read-only or read-write depending on use case.
- Enable/disable journaling to save flash wear.

###  4. **Device Drivers**
- Only include drivers relevant to your hardware (I2C, SPI, GPIO, UART, USB, etc.).
- Disable unneeded drivers to reduce image size and attack surface.
- Add support for custom drivers via `.bbappend` and `.cfg`.

###  5. **Security Features**
- Enable kernel hardening options:
  - Stack protector
  - ASLR (Address Space Layout Randomization)
  - SELinux or AppArmor
- Remove legacy or unused protocols to reduce attack surface.

###  6. **Processor and Architecture Specific Settings**
- Set correct CPU type (e.g., Cortex-A53, A72).
- Optimize for single-core or multi-core systems.
- Enable hardware floating point, NEON support, etc.

###  7. **Kernel Features and Subsystems**
- Networking stack features (IPv6, VLAN, etc.)
- Filesystem features (overlayfs, squashfs compression)
- USB gadget support, HID, CAN, BLE, etc.
- Real-time kernel features (PREEMPT-RT patch)

### 8. **Init System**
- Use minimal init systems like `busybox`, `systemd`, or `SysV`.
- Customize init scripts to boot directly into your application.

---

#  Desktop Kernel vs  Embedded Kernel

Understanding the difference between a desktop and embedded Linux kernel helps in making design decisions based on system requirements.

---

##  Key Differences

| Feature / Attribute            |  Desktop Kernel                          |   Embedded Kernel                         |
|-------------------------------|-------------------------------------------|--------------------------------------------|
| **Purpose**                   | General-purpose (laptops, PCs, servers)   | Application-specific (IoT, industrial, etc.)|
| **Size**                      | Large (many modules & drivers)            | Small, optimized for specific hardware     |
| **Drivers**                   | Includes support for a wide range         | Only includes required drivers             |
| **User Interface**            | Supports full GUI environments            | Often headless or minimal UI               |
| **Boot Time**                 | Not a priority (20–60 seconds)            | Critical (needs fast boot: < 5 seconds)    |
| **Memory Usage**              | Assumes large RAM (GBs)                   | Optimized for low RAM (MBs)                |
| **Filesystem**                | ext4, btrfs, xfs                          | squashfs, ubifs, jffs2, cramfs             |
| **Init System**               | systemd, upstart                          | busybox init, systemd (minimal), SysVinit  |
| **Security Model**            | Multi-user, complex permissions           | Often single-user or sandboxed application |
| **Update Mechanism**          | Package manager (apt, rpm)                | Image-based (OTA) or atomic updates        |
| **Real-Time Support**         | Optional, not default                     | Frequently patched with PREEMPT-RT         |
| **Build Process**             | Binary packages (Ubuntu, Fedora, etc.)    | Custom-built using Yocto, Buildroot        |

---


#  How Linux Boots (Boot Process Overview)

The Linux boot process consists of several stages that transform a powered-off device into a fully running Linux system.

---

##  Linux Boot Stages 

1. **Power On / Reset**    

2. **Bootloader (e.g., GRUB, U-Boot)**  

3. **Linux Kernel**  

4. **Init System (e.g., systemd, SysVinit, BusyBox)**   

5. **User Space / Shell / Application**  


### 1️ Power-On and Reset (ROM Code)
- Processor starts executing from a fixed ROM location.
- Initializes clocks, RAM, and basic peripherals.
- Loads the **first-stage bootloader** (if required).

### 2️ Bootloader (e.g., GRUB, U-Boot)
Responsible for:
- Initializing board-specific hardware (RAM, UART, SD card, etc.)
- Loading the **Linux kernel** into RAM
- Loading the **Device Tree Blob** (`.dtb`) for ARM-based systems
- Loading an **initramfs/initrd** (optional)
- Passing control to the kernel

**Desktop**: GRUB loads `vmlinuz`  
**Embedded**: U-Boot loads `zImage` or `uImage`

---

### 3️ Linux Kernel Initialization
Once loaded:
- Sets up memory management (MMU, paging)
- Initializes device drivers (console, storage, network, etc.)
- Mounts the **root filesystem**
- Starts the **init process** (PID 1)

📄 Files loaded:
- Kernel image (`vmlinuz`, `zImage`, `uImage`)
- Optional: `initrd` / `initramfs`
- Device Tree (`.dtb`) — ARM only

---

### 4️ Init System (PID 1)
The **first user-space process** started by the kernel.

- Reads config files (`/etc/inittab`, `/etc/systemd/`, etc.)
- Starts system services (networking, sshd, logging, GUI)
- Mounts additional filesystems
- Launches user applications or login shell

Examples:
- `systemd` (desktop)
- `BusyBox init`, `SysVinit` (embedded)

---

### 5️ User Space & Applications
Finally:
- You get a login prompt, shell, or GUI
- In embedded systems, it may auto-launch a specific application (e.g., GUI, kiosk app, sensor logic)

---

### INTRODUCTION TO YOCTO 
![Image](https://github.com/user-attachments/assets/86f25d42-cd86-4dc0-a0e9-b74ded2dce5a)

#  Yocto Build System Overview

##  What is a Build System?

A **build system** automates the process of:

- Taking source code  
- Compiling it  
- Packaging it into **binaries**  
- Creating a bootable **Linux image** or **SDK**

In the **Yocto Project**, the build system consists of tools like **BitBake**, **OpenEmbedded**, and configuration **metadata** that guide the entire build process.

---

##  What is OpenEmbedded?

**OpenEmbedded (OE)** is a **build framework** for embedded Linux systems.

- It defines **recipes** (instructions for building packages).
- Manages **dependencies** and **cross-compilation**.
- Provides **layers** (collections of recipes) to customize builds for different hardware.

> Think of it as the "**database** + **rules**" for building embedded Linux distributions.

---

##  What is BitBake?

**BitBake** is the **build engine** of the Yocto Project and OpenEmbedded.

- It reads **metadata** (recipes, configurations).
- Decides **what to build and how**.
- Calls the appropriate **compiler/toolchain** to build the code.

>  BitBake is like the `make` tool for Yocto — it interprets instructions and does the actual building.

---

##  What are Binaries?

**Binaries** are the compiled output files generated from source code.

Yocto/BitBake generates:

- **Kernel image** (e.g., `zImage`, `Image`, `uImage`)
- **Bootloader** (e.g., `U-Boot`)
- **Root filesystem** (`rootfs.ext4`, `rootfs.tar.gz`)
- **Device Tree Blobs** (`.dtb`)
- **Kernel modules** (`.ko`)
- **Application binaries** (`/usr/bin/myapp`)

---
#  Yocto Project Basic Terminology

##  General Terms

| Term            | Description |
|------------------|-------------|
| **Yocto Project** | An open-source project that provides templates, tools, and methods to create custom embedded Linux distributions. It’s **not a Linux distro**, but a **build system framework**. |
| **Poky**          | The **reference distribution** of the Yocto Project. It combines **BitBake**, **OpenEmbedded-Core**, and sample configurations. Poky is the default build environment. |
| **BitBake**       | The **build engine** (like `make`) that reads recipes and executes tasks to fetch, configure, compile, and package software. |
| **OpenEmbedded**  | A **layered build framework** used by Yocto to organize recipes and configurations. Yocto uses **OpenEmbedded-Core (OE-Core)** as its base metadata layer. |

---

##  Metadata Structure Terms

| Term       | Description |
|------------|-------------|
| **Metadata** | A collection of instructions, configurations, and descriptions that tell BitBake how to build packages. Includes `.bb`, `.bbappend`, `.inc`, `.conf`, and `layer.conf` files. |

---

##  Files in Metadata

| File Type        | Description |
|------------------|-------------|
| **`.bb` (Recipe)**       | A recipe file that defines how to **fetch, configure, build, and install** a single software package. |
| **`.bbclass` (Class)**   | A class file that contains **reusable functions or variables** shared across recipes. For example, `autotools.bbclass` adds default steps for Autotools-based packages. |
| **`.bbappend` (Append)** | Used to **modify or extend** an existing recipe (usually from another layer) without editing the original `.bb` file. |
| **`.inc` (Include)**     | An include file with **shared variables or functions**. Recipes or config files can include this using `require` or `include`. Helps avoid duplication. |
| **`.conf` (Config)**     | Defines **build-time settings**, such as target machine, distro, or build features. Examples: `local.conf`, `distro.conf`, `machine.conf`. |

---

##  Layers

| Term       | Description |
|------------|-------------|
| **Layer**  | A directory that groups related metadata (**recipes**, **configs**, **classes**). Layers help organize and isolate features (e.g., `meta-networking`, `meta-qt5`). |

Each layer usually contains:
- `recipes-*` folders (e.g., `recipes-core/`)
- `conf/layer.conf` to register the layer
- Optional `.bb`, `.bbappend`, `.bbclass` files

---

### YOCTO PROJECT ARCHITECTURE
![Image](https://github.com/user-attachments/assets/ede9cadf-f8c4-4920-91db-fe0b828d9167)

---
## How Yocto Fetches Source Files (with SRC_URI)
Where SRC_URI Is Defined
You define it inside a BitBake recipe file (.bb), like this:

```bitbake

SRC_URI = "git://github.com/uclibc/uclibc-ng.git;protocol=https;branch=master"
```
This tells BitBake to fetch the source code from the specified Git repository using HTTPS and the master branch.

Once a Git Repo is Downloaded — Where Is It Stored?
When BitBake fetches a Git repository (as defined in SRC_URI), it stores it in a structured subdirectory inside DL_DIR (typically downloads/).

 Git Repository Storage Path
Yocto stores cloned Git repositories in:
```bash
<build-dir>/downloads/git2/<sanitized-git-url>/
```
 Example
If your SRC_URI is:

```bitbake
SRC_URI = "git://github.com/uclibc/uclibc-ng.git;protocol=https;branch=master"
```
Then Yocto saves the repo under:

```bash
<build-dir>/downloads/git2/github.com.uclibc.uclibc-ng.git/
```
## What Happens If Fetch Fails in Yocto?
# BitBake Raises an Error
```bitbake
ERROR: Fetcher failure for URL: 'git://github.com/uclibc/uclibc-ng.git;protocol=https;branch=master'
ERROR: Unable to fetch URL from any source.

```
# 1. User Configuration in Yocto

##  What is it?

User configuration refers to settings defined in the `conf/local.conf` file of your Yocto build environment.  
This file customizes how BitBake builds your target image.

---

##  Key Things Defined

###  `MACHINE`
Defines the target hardware platform.
> Example: `raspberrypi4`, `quectel-sbc`, `beaglebone`, etc.

###  `PACKAGE_CLASSES`
Sets the packaging format used in the build.
> Supported formats: `ipk`, `deb`, `rpm`

###  `EXTRA_IMAGE_FEATURES`, `IMAGE_INSTALL_append`
Adds or customizes packages and features in the image.
> Example: Enable SSH server, debug tools, etc.

###  Parallelism
Improves build performance by utilizing multiple CPU cores.
- `BB_NUMBER_THREADS`: BitBake parallelism
- `PARALLEL_MAKE`: Makefile parallelism

---

##  Example `local.conf` Snippet

```conf
MACHINE = "raspberrypi4"
PACKAGE_CLASSES = "package_ipk"

EXTRA_IMAGE_FEATURES = "debug-tweaks ssh-server-dropbear"
IMAGE_INSTALL_append = " nano i2c-tools"

BB_NUMBER_THREADS = "8"
PARALLEL_MAKE = "-j8"

```
#  What is Yocto Metadata 

**Metadata** is a collection of instructions that tells Yocto:

> “Here is how to download, build, install, and package software for your Linux image.”

---

##  Metadata Includes:

| File Type     | Description                                      | Example Filename              |
|---------------|--------------------------------------------------|-------------------------------|
| `.bb`         | A recipe file – tells how to build a package     | `nano_5.9.bb`                 |
| `.bbappend`   | Extends or modifies an existing recipe           | `nano_%.bbappend`             |
| `.bbclass`    | Shared logic reusable by many recipes            | `autotools.bbclass`           |
| `.conf`       | Configuration files for layers, machines, etc.   | `layer.conf`, `raspberrypi4.conf` |
| Patch files   | Code fixes or changes added before building      | `fix-tab-width.patch`         |

---

##  Where Do These Go?

All of these files live inside **layers** like:

- `meta/`
- `meta-openembedded/`
- `meta-yourcustomlayer/`

---

##  Example: `nano_5.9.bb` with Patch

Here’s a `.bb` recipe for the **nano** text editor **with a patch** included:

```bitbake
SUMMARY = "Nano is a small and friendly text editor"
LICENSE = "GPL-3.0-or-later"
LIC_FILES_CHKSUM = "file://COPYING;md5=ccf4f2a7fc04f033f79f403db35f5c7b"

SRC_URI = "https://nano-editor.org/dist/v5/nano-${PV}.tar.xz \
           file://fix-tab-width.patch"

SRC_URI[sha256sum] = "6f1f4fd705e942e5104294c186be7ae2a8d831ee89fc63be4e7c2f79f38b3487"

inherit autotools

# Optional: apply patch if needed
do_patch[depends] += "patch-native:do_populate_sysroot"

# Compilation step (usually autotools handles this, but here for clarity)
do_compile() {
    oe_runmake
}

# Installation step
do_install() {
    install -d ${D}${bindir}
    install -m 0755 src/nano ${D}${bindir}/nano
}

}

 ```

#  3. Machine (BSP) Configuration in Yocto

##  What is it?

**Machine configuration** defines the hardware-specific settings for your target board.  
This is part of the **Board Support Package (BSP)** in Yocto.

---
---

##  What Does It Include?

| Component            | Purpose                                                             |
|---------------------|---------------------------------------------------------------------|
| Kernel settings      | Kernel version, device tree, and kernel image type                 |
| Bootloader           | U-Boot, GRUB, or other bootloader configurations                   |
| Serial console       | UART configuration for debug console                               |
| Features             | Hardware features like Wi-Fi, Bluetooth, GPU                       |
| Root filesystem      | Output image types like `ext4`, `wic`, `tar.xz`                    |
| Default packages     | Board-specific firmware or drivers                                 |
| Machine overrides    | For conditional logic in recipes (`raspberrypi4:`)                 |
| **CPU architecture** | Target architecture like `arm`, `aarch64`, `x86`                   |
| **Tuning**           | Optimized compiler flags for performance (`armv8a`, `cortex-a72`)  |
| **Toolchain options**| Sets ABI and libc type (e.g., `eabi`, `glibc`, `musl`)             |

---

##  Updated Example: `raspberrypi4.conf`

```conf
# Machine identity
MACHINEOVERRIDES =. "raspberrypi4:"
DESCRIPTION = "Raspberry Pi 4 Model B"

# CPU and architecture
DEFAULTTUNE = "cortexa72-crypto"
TUNE_FEATURES = "aarch64 cortexa72 crypto"
TARGET_ARCH = "aarch64"

# Serial console config
SERIAL_CONSOLE = "115200 ttyS0"

# Hardware features
MACHINE_FEATURES += "wifi bluetooth gpu vc4graphics"

# Kernel and device tree
KERNEL_IMAGETYPE = "Image"
KERNEL_DEVICETREE = "bcm2711-rpi-4-b.dtb"

# Bootloader
UBOOT_MACHINE = "rpi_4_defconfig"

# GPU settings
USE_VC4GRAPHICS = "1"

# Extra firmware
MACHINE_EXTRA_RRECOMMENDS += "linux-firmware-brcm43430"

# Toolchain options
ABI = "aarch64"

```

# 🔹 4. Policy Configuration in Yocto

## 🧾 What is Policy Configuration?

Policy configuration in Yocto defines the high-level rules and preferences for how your Linux system is built — like which package format to use, what features to include (e.g., systemd, Wi-Fi), which licenses are allowed, and how the toolchain should behave.

- Packaging
- Licensing
- SDK/toolchain behavior

>  Think of it as the place where you say:  
> "All my images should use `systemd`, package as `.ipk`, and avoid GPLv3."

---

##  Where Is It Defined?

Policy configurations are usually found in:

- `conf/distro/*.conf` → distro-specific policies (e.g., `poky.conf`)
- `conf/bitbake.conf` → global build policy defaults

---

##  What Can You Define Here?

| Setting               | Description                                                                 |
|-----------------------|-----------------------------------------------------------------------------|
| `PACKAGE_CLASSES`     | Format of output packages (`ipk`, `deb`, `rpm`)                             |
| `DISTRO_FEATURES`     | Features enabled across the distro (`systemd`, `bluetooth`, `wifi`, etc.)   |
| `INHERIT`             | Automatically include specific BitBake classes (`rm_work`, `buildstats`)    |
| `INCOMPATIBLE_LICENSE`| Blacklist licenses you want to avoid (e.g., `GPLv3`)                        |


---

##  Example: `poky.conf`

```conf
DISTRO_NAME = "Poky (Yocto Project Reference Distro)"
PACKAGE_CLASSES = "package_ipk"
DISTRO_FEATURES += "systemd wifi bluetooth"
INCOMPATIBLE_LICENSE = "GPLv3"
INHERIT += "rm_work buildstats"
PREFERRED_PROVIDER_virtual/kernel = "linux-yocto"

```
##  .rpm, .deb, and .ipk Are Types of Packages — Not the Package Itself

> A **package** refers to a **bundle of built software** — such as binaries, libraries, documentation, config files, etc.  
> `.rpm`, `.deb`, and `.ipk` are just **different formats** in which that bundle is **compressed, structured, and delivered**.

---

##  Package Format Comparison

| **Package Format** | **Common Use**                     | **Managed By**       |
|--------------------|------------------------------------|-----------------------|
| `.rpm`             | Red Hat, Fedora, Yocto (optional)  | `rpm`, `yum`, `dnf`   |
| `.deb`             | Debian, Ubuntu, Yocto (optional)   | `dpkg`, `apt`         |
| `.ipk`             | OpenEmbedded, Yocto (default)      | `opkg`                |

---

##  Summary

- All three formats serve the same purpose: **install software packages** on a system.
- They differ in:
  - **Packaging structure**
  - **Compression method**
  - **Supported package manager**
- In **Yocto**, the format is chosen using `PACKAGE_CLASSES` in `local.conf`:

```conf
PACKAGE_CLASSES ?= "package_ipk"
```

You can also use:

```conf
PACKAGE_CLASSES ?= "package_rpm"
PACKAGE_CLASSES ?= "package_deb"
```
##  Real Examples of Software Packages

###  1. Application Packages (Runtime)

| **Package Name** | **Description**                              |
|------------------|----------------------------------------------|
| `busybox`        | A lightweight Unix command-line toolset      |
| `dropbear`       | A small SSH server and client                |
| `curl`           | Tool to transfer data via HTTP/FTP           |
| `nano`           | Text editor for terminal                     |
| `python3`        | Python 3 interpreter                         |
| `htop`           | Interactive process viewer                   |

---

##  What Are Package Feeds in Yocto?

###  Definition:
Package feeds are **directories or remote servers** containing the **binary packages** (`.ipk`, `.deb`, `.rpm`) that Yocto builds and stores — ready to be installed later on the target device using a package manager (`opkg`, `apt`, or `rpm`).

---

###  Where Are They Located?

After building an image with BitBake, you’ll find the package feeds in:

```bash
tmp/deploy/ipk/
tmp/deploy/rpm/
tmp/deploy/deb/
```

Each of these contains architecture-specific folders with the actual packages and index files (like `Packages.gz`) used by package managers.


## From Package Feeds  Image & SDK

# BitBake Builds Packages

BitBake recipes (`.bb` files) fetch source code using `SRC_URI`.

The code is then:

1. Configured
2. Compiled
3. Installed

Finally, it's split into multiple **binary packages**.
 **Output** → `.ipk`, `.deb`, or `.rpm`

 **Stored in**:
```bash
tmp/deploy/ipk/
```

These are your **Package Feeds**.

---

##  Root Filesystem Creation (Image)

BitBake then uses these package feeds to construct the **root filesystem (rootfs)** of the embedded Linux image.

- `IMAGE_INSTALL` controls **which packages** go into the image.

 ##  Adding Packages to Your Image with `IMAGE_INSTALL`

You specify which packages should be included in your final image using the `IMAGE_INSTALL` variable.

This is usually done in a configuration file like:

- `conf/local.conf` (for temporary or quick changes)
- or in your **custom image recipe** (`.bb` file)

---

###  Example in `conf/local.conf` or image recipe:

```conf
IMAGE_INSTALL += "busybox dropbear curl nano"
```

This tells Yocto to include the following packages in your root filesystem:

- `busybox` – core Unix utilities
- `dropbear` – lightweight SSH server
- `curl` – command-line tool for HTTP/FTP
- `nano` – terminal-based text editor
 

BitBake pulls these packages from:

- `tmp/deploy/ipk/` (package feed)
 **Filesystem types can be**:

- `ext4`
- `wic`
- `tar.gz`
- etc.

##  Assembling Everything into a Final Bootable Image

This is where Yocto’s **`wic` tool** comes into play — if enabled via the `IMAGE_FSTYPES` variable.

###  Enable `wic` Image Format:
```conf
IMAGE_FSTYPES = "wic ext4"
```

---

###  What Does `wic` Do?

The `wic` tool **combines multiple components** into a single, bootable disk image.

It takes:

- **Bootloader** (e.g., `u-boot`)
- **Linux Kernel** (e.g., `Image`)
- **Root Filesystem** (e.g., `.ext4`)

 And assembles them into a complete bootable image, such as:

- `.wic`
- `.sdimg`
- `.img`

---

###  Example Output Location:
```bash
tmp/deploy/images/<machine>/core-image-minimal-<machine>.wic
```

This `.wic` file can be directly written to an SD card or eMMC for booting your embedded device.

##  What Is the SDK in Yocto?

The **SDK (Software Development Kit)** is a standalone toolchain and environment that allows you to build applications for your embedded image **outside** of Yocto.

###  It includes:
-  Cross-compiler (GCC, binutils)
-  Libraries and headers for the target system
-  Environment setup script to configure the build environment

---

## ⚙️ How SDK Is Generated from Package Feeds

---

###  1. BitBake Builds Packages First

All the packages (`.ipk`, `.deb`, `.rpm`) are built using BitBake recipes:

```bash
bitbake core-image-minimal
```

This stores binary packages in:

```bash
tmp/deploy/ipk/
```

These packages include:

-  Libraries (e.g., `libc`, `libssl`)
-  Binaries (e.g., `busybox`, `dropbear`)
-  Dev files (e.g., `libssl-dev`, `zlib-dev`)

 These **dev packages** are used when generating the SDK.

---

###  2. Generate the SDK

You generate the SDK with the following command:

```bash
bitbake core-image-minimal -c populate_sdk
```

This will:

- Build a cross-toolchain
- Collect all necessary headers and libraries from the package feeds
- Generate an SDK installer `.sh` script
- Include an environment setup script

---

 **SDK Output Location**:

```bash
tmp/deploy/sdk/
└── poky-glibc-x86_64-core-image-minimal-aarch64-toolchain-<date>.sh
```


---
## Getting Started with the Yocto Project

1. Visit the official [Yocto Project website](https://www.yoctoproject.org/) and go to the **Source Repositories**.
2. Select the **Poky** repository and choose the `scarthgap` branch.
3. Clone the repository using:

   ```bash
   git clone git://git.yoctoproject.org/poky

  
![image](https://github.com/user-attachments/assets/2a82cb7d-6552-4ad0-bb81-6405e0fefecb)  

## <u>Environment setup and downloading poky reference</u> :
- Install visual studio and create workspace for our project
### Build host packages:
- You must install essential host packages on your build host. The following command installs the host packages based on an Ubuntu distribution:
- `sudo apt update
sudo apt install gawk wget git-core diffstat unzip texinfo gcc-multilib \
     build-essential chrpath socat cpio python3 python3-pip python3-pexpect \
     xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa \
     libsdl1.2-dev pylint xterm curl
`
## Yocto project basic configuration and exploring the poky source
- The Yocto Project uses bitbake to process software packages and generate images.

- First, source the environment script to set up the build environment:
  
  `source oe-init-build-env`
  
- After sourcing you will be automatically entered into build directory.
  
  ### Build Folder:
  - In Build directory we have two important configuration files 1)bblayers.conf  2)local.conf
    ### 1)bblayers.conf:
    BBLAYERS ?= " ... "
  - Main variable: tells BitBake which layers to include in the build.
  - Each path points to a Yocto layer (a folder with recipes, classes, and configs).
  - You can add/remove layers here based on your project requirements.
    
    - ![Screenshot 2025-07-04 231121](https://github.com/user-attachments/assets/e58f95a4-c18d-48d8-aab6-80a3c9bb4806)
  ### 2)local.conf:
  - This file is used to set machine type, image name, build parallelism, and other configuration settings specific to your       build.
  - here machine name means board configuration.
  - here already the poky distribution will give us some examples qemuarm,qemuarm64,qemux86,qemux86-64 etc
  - But here the default machine given is qemux86-84
    ![image](https://github.com/user-attachments/assets/d8fe7c3f-a0a1-4125-9925-54c2deb543c6)
  - assiging our own board configuration for raspberry pi5 is:  
    ![image](https://github.com/user-attachments/assets/5b878aa7-fc6a-497a-8502-f36b7cc4e724)

   
- Let's take an example to do bitbake for an software package
  ### Bitbake a software package(drop bear):
- The Dropbear recipe is located in: `meta/recipe-core/dropbear`
- Run the command to build it: `bitbake dropbear`
- The following steps are executed internally from the recipe file (e.g., dropbear_2022.83.bb):  
     1)Fetch and downloads the source code .  
     2)Configures and compiles the source for the target architecture.   
     3)Install rootfs for our target board and perform some quality issues checks

  After build complition of dropbear ssh server we can observe the created folders:
  
  ![image](https://github.com/user-attachments/assets/d2a85fff-3348-453b-b665-d8ec0db939d7)

  ![Screenshot 2025-07-06 163621](https://github.com/user-attachments/assets/e1042884-e44f-4990-a646-607ac78a1564)

## AppArmor Error While Bitbaking a Yocto Recipe

An **AppArmor error while bitbaking a Yocto recipe** usually indicates that your system’s **AppArmor security policies** are interfering with some operations that the **BitBake build system** is trying to perform — especially operations that:

- Access unusual directories
- Use `pseudo` (fakeroot)
- Use `chroot`
- Use custom sysroots

These restrictions may lead to permission denials, build failures, or unexpected errors during the Yocto build process.

### Example Error Messages

You might encounter messages like:

- `apparmor="DENIED" operation="open" ...`
- `audit[bitbake]: type=1400 audit(...) ...`
- `Permission denied due to AppArmor profile ...`

###  Common Fixes

```bash
sudo systemctl stop apparmor
sudo systemctl disable apparmor
```
##  Steps to Permanently Disable AppArmor via GRUB

This disables **AppArmor** from **boot time**.

---

### 1. Edit the GRUB config:

```bash
sudo nano /etc/default/grub
```

Look for the line:

```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
```

Modify it to:

```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash apparmor=0"
```

>  `apparmor=0` disables AppArmor at kernel boot time.

---

### 2. Update GRUB:

Run the following command to apply the change:

```bash
sudo update-grub
```

---

### 3. Reboot your system:

```bash
sudo reboot
```

---

### 4. Confirm AppArmor is Disabled:

After reboot, verify the AppArmor status:

```bash
sudo aa-status
```

It should return:

```
apparmor module is not loaded.
```

Alternatively, check kernel parameters:

```bash
cat /proc/cmdline
```

Look for `apparmor=0` in the output.

---

>  You have now disabled AppArmor permanently via GRUB.

## PATHS FOR NANO 

===> poky/meta-openembedded/meta-oe/recipes-support/nano
--->poky/meta/classes/autotools.bbclass
  ____________________________________________________________________________________________________________________



#  What Is a Layer in Yocto?

In the **Yocto Project**, a **layer** is a modular collection of:

- Recipes (`.bb`, `.bbappend`)
- Configuration files
- Machine definitions
- Image customizations

Think of each layer as a **plugin** that contributes some functionality to your build. The build system **stacks multiple layers** to generate the final embedded Linux image.

---

#  Purpose of a Custom Layer

A **custom layer** (often named `meta-yourname`) is created to **add your own code and customizations** without touching the official Yocto layers.

It’s like creating your **own workspace** within Yocto to keep your work clean, maintainable, and upgrade-friendly.

---

#  Problems Without a Custom Layer

If you directly modify existing layers (like `meta`, `meta-openembedded`, `poky`, etc.), you run into serious issues:

|  Problem |  Description |
|-----------|----------------|
| **Updates break your changes** | When you pull a new version of Yocto or its layers, your edits may be overwritten or cause merge conflicts. |
| **No modularity** | Yocto layers are supposed to be modular and reusable. Direct modifications break this structure. |
| **Hard to debug** | Your custom changes are mixed with upstream code. It's hard to identify what changed. |
| **No collaboration** | Team members cannot reuse or track changes effectively without a dedicated layer. |
| **Portability issues** | You cannot easily port your changes to another Yocto build or project. |

---

#  Benefits of a Custom Layer

|  Benefit |  Why It Matters |
|-----------|-------------------|
| **Safe and isolated** | Your changes are cleanly separated from upstream Yocto metadata. |
| **Reusable** | You can copy your layer to other projects or share it with teammates. |
| **Trackable in Git** | A custom layer can have its own Git repo, history, and version control. |
| **Compatible with updates** | You can safely update Yocto core layers without breaking your customizations. |
| **Supports overrides and extensions** | Use `.bbappend`, image extensions, and patches without touching originals. |

---
#  How to Create a Custom Layer in Yocto

Creating a custom layer in Yocto allows you to add your own recipes, modify existing packages, and customize images — **without touching core layers**.

---

#  Copy and Customize a Yocto Layer (Using `meta-skeleton` as Base)

This guide shows you how to:

-  Copy an existing Yocto layer (`meta-skeleton`)
-  Rename it to `meta-mycustom`
-  Add it to your Yocto build
-  Use VS Code to edit and manage your layer

---

##  Example Folder Structure (Inside `poky` Directory)

```bash
poky/
├── meta/
├── meta-poky/
├── meta-skeleton/         ← [Original layer to copy]
├── meta-mycustom/         ← [Your new custom layer]
├── build/                 ← [Your build directory]
└── ...
```

#  How to Create a Custom Recipe in Yocto and Add It to an Image

##  1. Create a Custom Layer (Already Done)

## Create a New Recipe in Your Custom Layer
We will create a simple C program called helloworld.
##  Folder Structure:
```
meta-mycustomlayer/
├── conf/
│ └── layer.conf
├── recipes-example/
│ └── helloworld/
│ ├── helloworld_1.0.bb
│ └── files/
│ └── helloworld.c
```
#  Create a Custom Recipe in Yocto and Add It to an Image

---

##  helloworld.c

```c
#include <stdio.h>

int main() {
    printf("Hello from Yocto custom recipe!\n");
    return 0;
}
##  Create the BitBake Recipe
```

### helloworld_1.0.bb

```bitbake
SUMMARY = "Simple Hello World app"
DESCRIPTION = "This is a simple hello world C application"
LICENSE = "MIT"
LIC_FILES_CHKSUM = "file://${COMMON_LICENSE_DIR}/MIT;md5=0835c1d3c5c70ffb9797d8d2e2c9f09b"

SRC_URI = "file://helloworld.c"

S = "${WORKDIR}"

do_compile() {
    ${CC} ${CFLAGS} ${LDFLAGS} -o helloworld helloworld.c
}

do_install() {
    install -d ${D}${bindir}
    install -m 0755 helloworld ${D}${bindir}
}
```
##  Create or Modify a Custom Image Recipe

###  Create the image recipe file:
`
```bitbake
require recipes-core/images/core-image-minimal.bb

IMAGE_INSTALL += "helloworld"
```
##  Step 1: Add Qt6 to Your Yocto Build

Ensure your Yocto setup supports **Qt6**:

- Use **Yocto Hardknott** or newer.
- Add the `meta-qt6` layer to your build environment.

###  Clone the `meta-qt6` Layer

```bash
git clone https://github.com/meta-qt6/meta-qt6.git
```
##  Add `meta-qt6` to `bblayers.conf`

Edit your `conf/bblayers.conf` file and append the path to `meta-qt6`:

```conf
BBLAYERS += "path/to/meta-qt6"
```
##  Create a Qt GUI App (Basic Example)

**File: `hellogui.cpp`**

```cpp
#include <QApplication>
#include <QLabel>

int main(int argc, char *argv[]) {
    QApplication app(argc, argv);

    QLabel label("Hello, Prawin!");
    label.resize(200, 100);

    // Set pink background and black text
    label.setStyleSheet("QLabel { background-color: pink; color: black; font-size: 20px; font-weight: bold; }");

    label.setAlignment(Qt::AlignCenter); // Center the text
    label.show();

    return app.exec();
}
```
##  Custom Layer Directory Structure

Put this inside your custom layer `meta-mycustomlayer`:
```
meta-mycustomlayer/
└── recipes-qt/
└── hellogui/
├── hellogui_1.0.bb
└── files/
└── hellogui.cpp
```
##  BitBake Recipe for Qt GUI App

**File:** `hellogui_1.0.bb`

```bitbake
SUMMARY = "Simple Qt GUI showing name"
LICENSE = "MIT"
LIC_FILES_CHKSUM = "file://${COMMON_LICENSE_DIR}/MIT;md5=0835c1d3c5c70ffb9797d8d2e2c9f09b"

DEPENDS += "qtbase"

SRC_URI = "file://hellogui.cpp"

S = "${WORKDIR}"

inherit qt6

do_compile() {
    ${CXX} ${CXXFLAGS} ${LDFLAGS} -fPIC -o hellogui hellogui.cpp \
    `${OE_QMAKE_PATH_HOST_BINS}/qt-cmake -find-package Qt6Widgets Qt6`
}

do_install() {
    install -d ${D}${bindir}
    install -m 0755 hellogui ${D}${bindir}
}
```
##  Include GUI App in Predefined Image Recipe

To include the `hellogui` Qt GUI application in a **predefined image recipe**, such as `core-image-minimal`, **do not create a new custom image recipe**.  
Instead, **append the required packages** in your local configuration file (`local.conf`):

###  Step: Edit `conf/local.conf`

```bitbake
IMAGE_INSTALL:append = " hellogui qtbase"
```

