
### DOWNLOAD AND INSTALLING NEW KERNEL

**step1 :** check the current kernel version using uname -r
**step2 :** Download the kernel from www.kernel.org
**step3 :** Untar the downloaded kernel version file using tar -xvf linux-6.6.93.tar.xz
**step4 :** Go to that downloaded directory using cd linux-6.6.93/
**step5 :** Install some presequites
            sudo apt-get install libncurses5-dev libncurses5-dev
            sudo apt-get install build-essential
            sudo apt-get install bison
**step6 :** then do make menuconfig
**step7 :** verify .config file is generated or not using ls -a
**step8 :** then do make -->  its generates some header file missing errors 
            give ---> sudo apt-get install libssl-dev
                      make
                      sudo apt-get install libelf-dev
                      make
                      ln -s /usr/src/linux-source-6.6.93/debain debain 
                      enter
                      scripts/config --disable SYSTEM_TRUSTED_KEYS
                      enter
                      scripts/config --disable SYSTEM_REVOCATION_KEYS
                      enter
                      make
**step9 :**  sudo make modules_install
**step10 :** sudo make install
**step11 :** sudo update-grub
**step12 :** sudo reboot
             we cant see the grub menu after rebooting for that we have to goto grub file using sudo nano /etc/default/grub
             do some changes in that file : GRUB_TIMEOUT_STYLE =  menu
                                            GRUB_TIMEOUT = 10
              CTRL+O --> save
              CTRL+X --> exit
**step13 :** again update grub menu using sudo update-grub and then sudo reboot
             A grub menu appears,in grub menu select new kernel version
             if you get any error like not loading the kernel version then we have to press esc and then go to UEFI Firmware settings
             check boot sequence is enabled or not
              
