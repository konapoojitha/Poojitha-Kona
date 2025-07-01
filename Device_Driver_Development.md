### UART DRIVER DEVELOPMENT
```c
1.OBJECTIVE
2.UART INTRODUCTION
3.HARDWARE REQUIREMENTS
4.SOFTWARE REQUIREMENTS
5.DOWNLOADING AND INSTALLING A NEW KERNEL
6.UART MODULE DEVELOPMENT
7.OUTCOME
```

### DOWNLOAD AND INSTALLING NEW KERNEL

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
              
