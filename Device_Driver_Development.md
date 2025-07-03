### UART DRIVER DEVELOPMENT
```c
1.OBJECTIVE
2.UART INTRODUCTION
3.HARDWARE REQUIREMENTS
4.SOFTWARE REQUIREMENTS
5.DOWNLOADING AND INSTALLING A NEW KERNEL
6.UART MODULE DEVELOPMENT
7.OUTCOME
8.Challenges
```

### 1.OBJECTIVE
```c
To develop a UART (Universal Asynchronous Receiver/Transmitter) Linux device driver that allows serial communication between the host and a peripheral device. This involves:
            - Understanding the UART hardware.
            - Writing a Linux character device driver.
            - Transmitting and receiving data using UART.
            - Integrating and testing the driver in a Linux kernel environment.

```

### 2.UART INTRODUCTION
```c
            UART is a hardware communication protocol that uses two lines:
            
                        TX (Transmit)
                        RX (Receive)
                        Key Features: Asynchronous (no clock signal shared).
                                      Common in serial ports (RS-232, RS-485, TTL).
                                      Widely used in embedded systems, serial consoles, GPS, Bluetooth modules, etc.
                        UART Communication Parameters:Baud Rate (e.g., 9600, 115200)
                                                      Data bits (5–8)
                                                      Stop bits (1 or 2)
                                                      Parity (None, Even, Odd)
```

### 3.HARDWARE REQUIREMENTS
```c
            Two PCs with Serial (DB9) Ports
            Null Modem Serial Cable (DB9-to-DB9, Crossed)
            Linux Host System (on both PCs)
```

### 4.SOFTWARE REQUIREMENTS
```c
            Linux OS with development tools (Ubuntu or any distro).
            
            Kernel source or headers for the target system.
            
            Cross-compiler for embedded target (e.g., arm-none-eabi-gcc).
            
            Minicom/PuTTY for UART communication testing.
            
            Device tree source (DTS) files (if working with embedded platforms).
            
            insmod, rmmod, dmesg, mknod utilities for kernel module testing
```
                        


### 5. DOWNLOAD AND INSTALLING NEW KERNEL

```c
comments that are mentioned in '' are linux commands
```

```c
**step1 :** check the current kernel version using 'uname -r'
```
```c
**step2 :** Download the kernel from www.kernel.org
```
```c
**step3 :** Untar the downloaded kernel version file using 'tar -xvf linux-6.6.93.tar.xz'
```
```c
**step4 :** Go to that downloaded directory using 'cd linux-6.6.93/'
```
```c
**step5 :** Install some presequites
            'sudo apt-get install libncurses5-dev libncurses5-dev'
            'sudo apt-get install build-essential'
            'sudo apt-get install flex'
            'sudo apt-get install bison'
```
```c
**step6 :** then do 'make menuconfig'
```
```c
**step7 :** verify .config file is generated or not using 'ls -a'
```
```c
**step8 :** then do 'make' -->  its generates some header file missing errors 
            give ---> 'sudo apt-get install libssl-dev'
                      'make'
                      'sudo apt-get install libelf-dev'
                      'make'
                      'ln -s /usr/src/linux-source-6.6.93/debain debain' 
                       enter
                      'scripts/config --disable SYSTEM_TRUSTED_KEYS'
                       enter
                      'scripts/config --disable SYSTEM_REVOCATION_KEYS'
                       enter
                      'make'
```
```c
**step9 :**  'sudo make modules_install'
```
```c
**step10 :** 'sudo make install'
```
```c
**step11 :** 'sudo update-grub'
```
```c
**step12 :** 'sudo reboot'
             we cant see the grub menu after rebooting for that we have to goto grub file using 'sudo nano /etc/default/grub'
             do some changes in that file : GRUB_TIMEOUT_STYLE =  menu
                                            GRUB_TIMEOUT = 10
              CTRL+O --> save
              CTRL+X --> exit
```
```c
**step13 :** again update grub menu using 'sudo update-grub' and then 'sudo reboot'
             A grub menu appears,in grub menu select new kernel version
             if you get any error like not loading the kernel version then we have to press esc and then go to UEFI Firmware settings
             check boot sequence is enabled or not,if it is enabled--disable it and enter into new kernel.
             If the new kernel is opened then check version with 'uname -r'
```

### 6.UART MODULE DEVELOPMENT
```c
Step 1: Create UART Character Driver Skeleton
        static int __init uart_init(void)
        {
                printk(KERN_INFO "UART Driver Loaded\n");
                return 0;
        }
            
        static void __exit uart_exit(void)
        {
                printk(KERN_INFO "UART Driver Unloaded\n");
        }
            
        module_init(uart_init);
        module_exit(uart_exit);
        MODULE_LICENSE("GPL");
```

```c
Step 2: Register Character Device
        Use register_chrdev() or cdev_add() to register device.
        Implement file operations: open(), read(), write(), close()
```
```c
Step 3: Access UART Hardware
        Use ioremap() to map UART base address.
        Control UART via memory-mapped registers (e.g., LSR, THR, RBR).
        Configure baud rate, parity, stop bits, etc.
```
```c
Step 4: Testing
        Load module: sudo insmod uart_driver.ko
        Use dmesg to verify: dmesg | tail
        Create device node: sudo mknod /dev/uart0 c <major> 0
        Read/Write to UART: echo "Hello UART" > /dev/uart0
                            cat /dev/uart0
```

### 7.OUTCOME
```c
            Successfully developed a UART character driver.
            
            Achieved serial communication via /dev/uart0.
            
            Demonstrated reading/writing from UART using user-space apps.
            
            Understood kernel module programming and UART register handling.
            
            Created reusable UART module for embedded systems.
```
### 8.CHALLENGES

            - Successfully compiled and installed a custom UART driver module into the Linux kernel.
            
            - Unable to transmit/receive data after driver installation — the UART interface was not functioning as expected.
            
            - Suspected the issue could be related to UEFI Secure Boot being enabled in BIOS settings.
            
            - Since Secure Boot prevents loading unsigned kernel modules, attempted to load the custom kernel with our driver built-in — but this failed.
            
            - To resolve Secure Boot issues, generated custom trusted keys (X.509 certificate) and signed the kernel and modules using:
            
                                    openssl for key generation.
                                    
                                    ign-file utility for module signing.
            
            - Even after signing, simple test modules failed to load under Secure Boot.
            
            - To debug the root cause, attempted the same procedure on a clean/original Ubuntu system (with Secure Boot disabled).
            
            - Driver worked as expected on original Ubuntu — confirming that Secure Boot was blocking unsigned modules in the original setup.

            - To resolve the issue more robustly, **downloaded a different kernel version** manually.

            - Ran `make menuconfig` and modified the generated `.config` file.

            - Specifically **enabled** the following option:

                          - `CONFIG_EFI_MOKVAR=y` — to allow access to MOK (Machine Owner Key) variables from the kernel.
            
            - Recompiled the kernel using the following steps:

                          - `make`
            
                          - `make modules_install`
            
                          - `make install`

            - After installing the new kernel, wrote a **simple character device module** to test Secure Boot compatibility.

            - Generated **MOK (Machine Owner Key) pairs** using `openssl`:

                          - Used the key to sign both the **downloaded kernel** and the **character module**.

            - Attempted to install the `.ko` module using `insmod`.

            - The module **loaded successfully**, indicating that:

                          - The Secure Boot trust chain was now properly set up.
            
                          - The MOK keys were correctly enrolled and accepted by the system.


---



              
