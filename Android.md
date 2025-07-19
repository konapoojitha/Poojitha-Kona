# Andriod
## Indroduction about Embedded System

An embedded system is a combination of hardware and software designed to perform a specific task or function.

```
 Why Embedded System?
 - Embedded systems will use only the necessary components for the task.By this it will improve efficiency and cost efficient.
 - It typically small and compact devices.Making ideal for any industry.
 - Embedded Systems consumes less power compared to General purpose systems. 
```

```
Is phone a completely embedded system?
- No, Phone is not a purely embedded system, but a complex computing device that integrates multiple embedded systems.
- Multiple Embedded sytems means Camera Module,Display Module,Fingerprint sensor...
```
## Classification of Embedded systems in terms of Safety

| Feature            | Safety-Critical (Non-Complex)      | Non-Safety-Critical (Complex)    |
|--------------------|------------------------------------|----------------------------------|
| **Risk**           | Failure may harm human life        | Failure causes no harm           |
| **Functionality**  | Performs essential tasks           | Non-critical tasks               |
| **Scheduling**     | Needs deterministic timing         | Not time-sensitive               |
| **User Input**     | Minimal interaction                | User-driven actions              |
| **Complexity**     | Internally simple logic            | More complex but safe            |
| **OS Type**        | FreeRTOS / Baremetal               | Linux / Android                  |
| **CPU Need**       | Simple processor                   | No high-end CPU required         |

## For any General OS stack for Embedded System
```
+-----------------------------+
|                             |  ← It is user interface(provides functionality to the user) 
|      Applications           |
|                             |         
+-----------------------------+
|                             |
|        Services             |   ← What services/options does the mmodule provides
|                             |   ← Ex: Camera module is used for photo and recording.
+-----------------------------+
|                             |
|        Libraries            |  ← It is a programs responsible for the task you want to perform 
|                             |   
+-----------------------------+
|                             |
|    System Call Interface    |  ← Bridge between the userspace and kernal space
+-----------------------------+
|                             |
|        Drivers              |  ← It is a software code in kernal space used to interact with the hardware 
|                             | 
+-----------------------------+
|                             |
|       Linux Kernal          |  ← It is the OS which manages everything like memory,device files and it interacts with hardware
|                             | 
+-----------------------------+

```
## Note:
<img src="/Andriod/1.png" alt="Embedded System" width="400"/>

A embedded system means we should not only customize the hardware but also to customize the software of user interface.

## Building of Android OS


### Build System
A build system is a set of tools, configuration files, and rules used to automate the process of transforming source code into a final executable program.

### Build Engine
A build engine is the part of the system that actually executes the build instructions with the help of tootal chain.

Ex: lets us assume a Function, function declaration will be Build engine and function defination will be Build Engine
```
         ## Inputs                                                                                ## Outputs 
+-----------------------------+            +-------------+                                    
|                             |            |             |                                    --> RoofFs Images
|      Andriod Source code    |            |             |            +-------------+         --> Kernal Images
|                             |            |  Build      |            |             |
+-----------------------------+            |  Engine     |            |             |          
|                             |            |( Bitbake/task |          |             |         --> Bootloader Images
|    BootLoader Source Code   |    ------> |  Executer)  | ------->   |  Toolchain  |         
|                             |            |  Build      |            |  ( gcc )    |
+-----------------------------+            |  System     |            |             |
|                             |            |(Yocto/Make) |            +-------------+         
|       BootROM Firmware      |            |             |                                    --> BootRAM firmware
|     ( Executable file )     |            |             |
+-----------------------------+            +-------------+             
|                             |                                   
|    Additional Chip Firware  |                                                               --> Additional Chip
|    ( Bluuetooth,WIFI)       |                                                                     Firmware
+-----------------------------+                      
```
- The build system converts the source file/code to executable or Images or Firmware.
- After generating the images or firmware it will be flashed into development board.
- BootRom firmware is flased to BootROM
- Additional Firmware is flased to Additional Chip Firmware.

## Difference between the Andriod and Linux Operating System.

### 1. Bionic C
- In andriod we use Bionic C .(It is a customized C standard library developed by Google)
- It is light weight.Glibc is large Bionic is lean, smaller in size & faster to load.
- It is designed for limited CPU memory & power usage.

### 2. Completely Fair Scheduling (CFS)
    - In linux we generally se Round Rbbin or Priority based Scheduling.
    - In CFS the task will be run on Virtual runtime.
    - It follows red-black tree Algorithm.
       - Leftmost node always has smaller vruntime.
#### Step:1
  - Cpu based on the priority or critical code of the process it will assigned the nice value to the processes.
  - Nice value (-20 to 19) - it is not fixed
     - (-20) -> Higher Priorty
     - 19 -> lowest priority
#### Step:2
 - CFS depends on Vruntime manner.
 - Vruntime = CPU_time * weight
 - Lowest Vruntime will have more CPU time.
 - Based on Nice value there will be a weight to it.
   
#### Step:3 
 Lets take an Example 

 |            |   Nice Value  |     Weight |
 |------------|---------------|------------|
 | **Task A** |        0      |     1.0    |
 | **Task B** |       -5      |     0.8    |
 | **Task C** |       +5      |     1.25   |

- Whenever a new process will created initially it will have lesser runtime.The cpu will run until it will match with other process.
- Until then it will always be a left most in the tree.

### 3.Low Memory Killer
- When Andriod system detects that available RAM is running Low.It will trigger LOW MEMORY KILLER(LMK)
- It checks the enough memory for important operations based on priority score.
- The lower priority tasks will be terminated.

#### Out of Memory Score(priority Score)
- Foreground process
- Background process
- Cached process

Linux uses OOM killer(Out of Memory)
- It comes into picture where the system runs when it is completely out of memory.
- It free up the memory

### 4.Sleep prone Kernal
- When you click the lock button the phone will be off.The kernal will enter into Sleep Mode/you can even suspend it.
- It will also happen in linux but it is less utilized.

### 5. WakeLocks
- You can wake the process/task from Suspended to perform the critical code.
- It is mechanism that allows an app/service to keep the device awake- either keeping CPU running, screen on when the user is not interacting.
Ex: 
   - playing music when screen turn off
   - Tracking GPS in fitness app.

### 6.Binder IPC
- linux uses the System V IPC like message queues,named pipes...
- It is a Remote procedure call based communication.It creates the server-client mechanism between the process.

### 7. Hardware Abstraction Layer
- Proper usage of hardware Abstraction is done in Andriod i.e. usage of kernal resources.
- Whereas in Linux you dont have much access/Much usage of hardware Layer.

 ### 8. Loging Mechanism
 - In Linux we use dmseg to see the kernal logs, journalctl for service logs.
 - In andriod we use dmesg to see the kernal logs, logcat for service logs.It uses circular buffer for writing the logs.

# Example Module of Linux Archietecture
<div align = "center">
    <img src="/Andriod/2.png" alt="Linux Architecture" width="300"/>
</div>

- Application Layer:
    - It is a user-space application that interacts with Bluetooth functionality.
    - Ex: bluetoothctl — a CLI tool to configure and control Bluetooth devices.
- Services:
    - Inter-Process Communication is used to communicate with system services.
    - A service that manages device discovery, pairing, etc.
- Libraries:
  - The service uses a Bluetooth library, such as the one provided by BlueZ (Linux Bluetooth protocol stack).
  - This is application-level logic written in C, providing APIs to manage Bluetooth.
- System Call Layer:
  - Functions like open(), read(), write(), ioctl(), and poll() are Linux system calls.
  - These bridge user space and kernel space, allowing access to hardware via drivers.
- Kernal Drivers:
  - RFCOMM Driver: Handles Bluetooth communication over serial protocols (like virtual serial ports).
  - UART Driver: Communicates with the physical Bluetooth hardware over UART (serial port interface).
- Bluetooth Hardware: The actual Bluetooth chip/module (e.g., via UART) that transmits and receives data wirelessly.

  Step 1 to Step 4 is used to customize the software/User version, Step 5 and 6 used to customize the hardware version.
 
# Android Archietecture

<div align = "center">
    <img src="/Andriod/3.png" alt="Andriod Architecture" width="600"/>
</div>

- From above image we can see that Andriod kernal space is mostly similar to Linux Kernal space.
- There are additional Changes like Wavelocks,CFS Scheduler,ashmem contains BinderIPC and there is no SysV IPC etc..

**Lets Understand the UserSpace or Software Stack of the Android Archeitecture**

### Application Layer
- The Application Layer is the topmost layer in the Android architecture. It includes all the apps that the user interacts with — both system apps (preinstalled) and user apps (downloaded).
- Applications are divided into 3 different categories.
   - System Applications
   - User Applications
   - Privileged Applications
 
 #### Sysytem Applications
 - System apps are essential for the basic functioning of the device.
 - They handle core operations like calling, messaging, managing settings,System UI etc.
 - This cannot be uninstalled by regular users.
 - It has system level permissions and information present in /system/permissions/priv-app.xml
 - Ex: App install & Management,System security & Configuration like brightness,ringtones,Network & connectivity, Device Hardwares & sensors. 

#### User Applications
 - Apps installed from the playStore or AppStore.
 - Installed by the user to add extra features and perform specific tasks like communication, social networking, media, and productivity.
 - It can access only the public APIs

#### Privileged Applications
 - Privileged apps have additional permissions that regular apps don’t have.
 - They can access restricted APIs and perform actions that are usually blocked for normal apps. These are usually installed by the manufacturer.
 - Ex: SIM ToolKit, System Updater, Themes ,Security etc...

### Android API's
- An Android API (Application Programming Interface) is a collection of predefined code libraries provided by the Android operating system.
- These tools include pre-written code for common tasks like sending messages, playing music, or using GPS.
- These APIs allow developers to build apps that can interact with the Android system and access various features such as the camera, location services, sensors,   file storage, without needing to directly control the underlying hardware.

### Android FrameWork
 <img src="/Andriod/Andriod_frameWork1.png" alt="Andriod Framework" width="600"/>

- Application uses Android APIs in order to communicate Android framework i.e.(classes). 
- The Android Framework will receives an request and assign the appropriate system services.
- The system services consists of Activity Manager,Notification Manager,Tephone Mangager etc...
- The Android frame only defines what action it has to perform.If you want to click the photo it has to access the hardware using HAL layer.
- This layer is wriiten in Java/kotlin and can be called as Java framework.

**Note: There are deamon process in System Services.The zygote initilizes these deamons**
 1) installd ---> for installing applications
 2) vold ---> deleting and measuring volume partitions in android.
 3) adbd ---> Android debug Deamon
 4) update_engine ---> Over the air update

<link src = "https://general-pusher-cms.s3.amazonaws.com/tutorial/constraint_Layout_2_chain_weight_2_76b73ec721.gif">

### Android RunTime Layer
- This layer is divided into Core Libraries and Dalvik Virtual Machines.
- The core Library package contains the APIs like Basic Classes, utilities(androi.util), File Access(java.io), Network Access(HTTPs,TCP), Clib...
- Basically Android Runtime/Dalvik Virtual Machines will convert the all these into .dex files

#### Dalvik Virtual Machines

 <img src="/Andriod/DVM.png" alt="Andriod Framework" width="350"/>
 
- Dalvik Virtual Machine Register Based VM.It provides the environment in which every Android application runs.
- Dalvik has been written such that a device can run multiple Virtual Machines efficiently.It uses multithreading concepts.
- . java ---> . class(byte code) ---> . dex ---> .odex ---> Application Lauch 
- It uses Dx tools to convert . class to . dex
- Dalvik, starting from Android 2.2, included a JIT compiler that translates bytecode into machine code at runtime to improve performance.
- . java code will be similar to public/private classes in C++ and .class will be similar to Assembly code and .dex will be similar to register based in CPU(v0,v1 and v2 will created).By compressing it 60% of memory is saved.
- For every Application a separate virtual machines will be created.Since we have different virtual machines, even one appication crashes it will not affect the other applications.

#### Android Run time
- ART replaced Dalvik starting from Android 5.0 as the default runtime.
- . java ---> . class(byte code) ---> . dex this is the common process for Android Runtime
- In Runtime ART it uses AOT(Ahead of time) it converts into .oat file (i.e. transulates into machinecode(decode)) so it can executes fastly and lauches the   application
- .java ---> .class(byte code) ---> .dex ---> .oat ---> Application Launch.

### Zygote
- Zygote is a mother of all applications services
- Zygote is a daemon process that starts by init in the Android boot process.
  
#### Responsibilities
- When Zygote is started first it will spawns the system services.And the system server spawns the other system services.
- It preloads the process and resources in Android Frameworks and Services.
- Zygote is responsible to start all the applications.

#### APP Launch
- Zygote does not run the apps itself. It only creates (forks) new processes for each app.
- Launcher -> In Activity manager checks the app is suspended or terminated.If it is suspended it resumes it.
- If not it tells zygote create the virtual machine to whatsapp or youtube.
- The parent Zygote returns to waiting for the next request.

### Hardware Abstraction Layer
- HAL (Hardware Abstraction Layer) acts as a bridge between the Android user space (application framework) and the kernel space (Linux kernel and device drivers).
- By the name, it abstracts hardware-specific details from the Android framework and applications. 
- HAL is implemented Low level Language which includes C/C++ and its compiled form is stored in shared object files (.so)
- HAL modules follow predefined interfaces defined by Android, and each hardware component has its own HAL module (e.g., camera HAL, audio HAL, sensors HAL)

**Android Framework in order to communicates with HAL layer it uses HIDL and AIDL**
 - Before Android 8 we use in process communication calls dlopen() + dlsym().
 - From Android 8 to Android 10 HIDL primarily facilitates communication between the Hardware Abstraction Layer (HAL) and the Android framework.
 - After Android 10+ AIDL focuses on inter-process communication (IPC) within the Android system i.e.Binder IPC, including communication between the framework and HALs.
 - The funtionality wise both AIDL and HIDL 

**Major Difference AIDL and HIDL**
- AIDL uses a Java-like syntax to define interfaces.HIDL uses Custom IDL syntax for implemetation it uses C++.
- AIDL (Android Interface Definition Language) uses only Binder IPC for interprocess communication.
- HIDL (HAL Interface Definition Language) supports both Binder IPC and passthrough modes for interprocess communication. While Binder IPC in HIDL provides secure communication, the passthrough mode allows direct method calls without the overhead of IPC.

```
Client process --> Binder IPC --> HAL process 
Client process --> direct method calls --> HAL
```

## Example of Android Archieteture
<img width="230" height="413" alt="image" src="https://github.com/user-attachments/assets/966a55ed-8d9a-4328-827f-ce9b3393cec8" />

- Previleged Application requests deep sleep request Android Framework through AIDL.
- Request goes through the Power Management Service and Car Power Management Service.
- Passed to the Power Management HAL via HIDL.
- HAL calls enter_deep_sleep().
- PMIC Driver and I2C drivers execute to go to deep sleep at the hardware level.

## Booting process
### linux
```
+-----------------------------+
|                             |   ← Bootcode or firmware is stored in EEPROM/ROM. 
|        Bootcode             |   ← Responsible for initializing hardware: RAM, CPU, keyboard, timers, etc.
|                             |         
+-----------------------------+
|                             |   ← The CPU is reset.The Program Counter (PC)  maps to firmware ROM (boot code).
|         Powerup             |   ← Bootcode is the very first code that executes when a device is powered on.
|                             |   ← It loads the bootloader.
+-----------------------------+
|                             |   ← Bootloader is divided into primary bootloader,Secondary bootloader and tertiary bootloader 
|        Bootloader           |   ← Roles: > Sets the minimum hardware like to enable access to RAM, Initializing basic input/output ports and initilization of kernal .
|                             |            > It provides for user the Boot menu which OS or kernal menu to load.           
|                             |            > Loads the Linux kernel and initilization of drivers into RAM
+-----------------------------+
|                             |   ← The kernel is the core of the operating system.
|       Linux Kernal          |   ← It sets up CPU scheduling, memory management, device drivers.
|                             |   ← It starts the first userspace process
+-----------------------------+
|                             |
|      Linux Userspace        |   ←  The init process invokes the remaining process
|                             |   ←  It runs the daemons process,background services etc..
+-----------------------------+
|                             |
|       Application           |   ← By entering the Password to username you access the applications like terminal,browser etc..
|                             | 
+-----------------------------+

```

### Android

- <img width="736" height="475" alt="image" src="https://github.com/user-attachments/assets/a06012ad-92f1-414f-8299-0854218857ae" />
