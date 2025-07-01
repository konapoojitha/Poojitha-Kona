
**What is kernel?**
```
	The kernel is the core part of an operating system that manages hardware and allows software (like apps or services) to interact with the hardware.
	Acts as a bridge between user applications and hardware
	Manages:    CPU scheduling
                    Memory
                    Processes
                    I/O devices (like USB, sensors, etc.)
                    File systems
                    Inter-process communication (IPC)
```

**Types of Kernels**
```
**Monolithic Kernel:** Everything (drivers, filesystem, etc.) runs in kernel space
		       → Example: Linux kernel
				
**Microkernel:** Minimal core, with many services running in user space
		       → Example: QNX, MINIX
				
**Hybrid Kernel:** Combines both approaches
		 → Example: Windows NT, XNU (used in macOS)

```

**Roles of Kernel**

```
 **Process Management**               Manages running programs (processes),handles creation, scheduling, and termination.Ensures fair CPU sharing and multitasking.
 
 **Memory Management**                Allocates and protects RAM between processes, handles virtual memory and paging.                                               
 **Device Management**                Uses device drivers to communicate with hardware like keyboards, displays, sensors, etc.                                      
 
 **File System Management**           Manages how data is stored and retrieved on storage devices (e.g., ext4, FAT32).                                                
 **System Call Interface**            Provides APIs so user-space applications can request services (like open files, create processes, etc.).                       
 **Security and Access Control**      Controls which process/user can access what; enforces permissions and isolation.                                               
 **Interrupt and Exception Handling** Responds to hardware interrupts and software exceptions in real time.
``` 

 **What is the Booting Process?**

```
	 The booting process is the sequence of steps that occur when a system is powered on, leading up to the loading of the operating system kernel and user applications.

  **Step 1**: BIOS/UEFI 
              The BIOS or UEFI firmware is the first to run.
              It checks your hardware (keyboard, RAM, disk).
              It then finds the bootloader on the selected boot device (like SSD or USB).

 **Step 2**: Bootloader takes over
             The bootloader (like GRUB in Linux) is now in control.
             It shows a menu (if multiple OSes exist).
             It loads two things into RAM:The kernel (e.g., vmlinuz)
                                          The initramfs (initial filesystem with drivers/modules)

**Step 3**: Kernel gets to work
            The Linux kernel decompresses itself.
            It sets up low-level hardware (CPU, memory, I/O devices).
            Initializes device drivers (like USB, disk, etc.)
            Loads the initramfs and uses it as a temporary root filesystem.
            Searches for and mounts the real root filesystem (e.g., / from disk).

**Step 4**: First User Process Starts
            The kernel now starts the first user-space program, usually:/sbin/init, or systemd (on most modern Linux distros)

**Step 5**: The init system:
            Starts background services (network, Bluetooth, display).
            Mounts filesystems.
            Brings the system to the default runlevel or target (multi-user or GUI).

System is Ready to Use
You see a login prompt or desktop GUI.
System is fully booted and ready.
```
**Device Driver Management**
```
Device driver management is how the operating system (OS) handles communication between the hardware (like keyboards, USB, camera) and software applications. A device driver is a special program that acts as a bridge between the OS and a specific hardware device.
```
**What is a Device Driver**
```
A device driver is a piece of code (usually part of the kernel or a kernel module) that allows the operating system to recognize, communicate with, and control a hardware device.
```
**Types of Drivers**

| Type                 | Example               | Description                      |
| -------------------- | --------------------- | -------------------------------- |
| **Character Driver** | Keyboard, serial port | Reads/writes one byte at a time. |
| **Block Driver**     | Hard disk, SSD        | Reads/writes blocks of data.     |
| **Network Driver**   | Ethernet, Wi-Fi       | Handles packet transmission.     |
| **Virtual Driver**   | RAM disk, loopback    | Software-simulated hardware.     |

**What is a Character Driver**
```
A character driver is a type of device driver that allows communication between the operating system and character-based devices — devices that transfer data one character (byte) at a time.
```
**Examples of Character Devices**
```
Keyboard,Mouse,Serial ports (COM ports),UART devices,Sensors,LED control interfaces
```
**What is UART**
```
UART stands for Universal Asynchronous Receiver/Transmitter. It's a hardware communication protocol used to transmit and receive serial data between devices — like a microcontroller and a PC.
```
	A UART driver is a character device driver that allows the OS (or your embedded system) to:
	
		Configure UART hardware (baud rate, stop bits, parity)
		
		Send/receive data over the UART interface
		
		Handle interrupts or polling for data

**UART Communication Flow**
```
			[ Application Layer ]
			        ↓ read(), write()
			[ UART Driver Layer ]
			        ↓ register_chrdev, file_operations
			[ Hardware (UART Controller) ]
```
**Core Responsibilities of a UART Driver**

| Functionality          | What It Does                                              |
| ---------------------- | --------------------------------------------------------- |
| **Initialization**     | Set baud rate, data bits, stop bits, parity, enable TX/RX |
| **Data Transmission**  | Send characters to UART TX register                       |
| **Data Reception**     | Read from UART RX register (polling or interrupt)         |
| **Interrupt Handling** | Manage RX/TX done interrupts                              |
| **Buffer Management**  | Optional circular buffer for RX/TX queues                 |

**Character Driver Development**
**step1 :** Download and Install the kernel version from www.kernel.org
**step2 :**




