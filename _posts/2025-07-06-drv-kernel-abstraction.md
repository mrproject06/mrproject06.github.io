---
title: "Linux Driver Model - Kernel Abstraction"
date: 2025-07-06 00:00:00  +0500
categories: [Linux]
tags: [Driver]
---

## Driver Architecture - Peripheral Devices/Slave

```
┌───────────────────────┐       ┌───────────────────────┐
│          APP          │       │                       │
└───────────┬───────────┘       │                       │
            ⇅                   │                       │
┌───────────▼───────────┐       │                       │
│ Kernel Abstraction    │       ┼───────────────────────┤
└───────────┬───────────┘       │                       │
            ⇅                   │                       │
┌───────────────────────┐       │                       │
│  High Level Drv.      │       │         CPU           │
│  ──────────────────── │◄──────┼───────────────────────┤
│   Slave Controller    │       │                       │
└───────────┬───────────┘       │                       │
            ⇅                   │                       │
┌───────────────────────┐       │                       │
│   Mid Level Drv.      │       │                       │
│  ──────────────────── │◄──────┼───────────────────────┤
│     Bus Manager       │       │                       │
└───────────┬───────────┘       │                       │
            ⇅                   │                       │
┌───────────────────────┐       │                       │
│   Low Level Drv.      │       │                       │
│  ──────────────────── │◄──────┼───────────────────────┤
│    Bus Controller     │       │                       │
└───────────┬───────────┘       └───────────┬───────────┘
            ⇅                               ⇅
┌───────────┴───────────────────────────────┴───────────┐
│                                                       │
│                     Bus Controller                    │
│                                                       │
└───────────┬───────────────────────────────┬───────────┘
            ⇅                               ⇅
    ┌───────▼───────┐               ┌───────▼───────┐
    │   Slave #1    │               │   Slave #2    │
    └───────────────┘               └───────────────┘
            ⇅
    ┌───────▼───────┐
    │   Slave #3    │
    └───────────────┘
```

Where should code for kernel abstraction be setup ?

It should be done when driver init function get called i.e. probe function.

Driver init function get called with device object as argument and will do the following:-

1. First will read the device configuration from device struct which it is receving as argument.

2. Setup the operation and register with kernel abstraction only if physical device gets detected.

High level is getting register with MID-LAYER as well as KERNEL ABSTRACTION in below order:

Bottom layer first and upper layer next. 

What is kernel abstraction and what's the purpose?

It means driver operation gets registered with kernel abstraction.

Through system call we expose the functionality of driver. 

It provides multiple option to driver to setup its operation and make them avaiable to the application.
There are multiple ways and it is called kernel driver model according to type of device and driver.

Driver dev can add new system call and use those system call to provide kernel abstraction to the driver
operations. 

But this model is not scalable as for every driver too many system call, application programmer
has to look for documentation for system call which comes as part of driver model.

Exisiting system call or kernel abstraction to reach the driver.

Open/Read/Write : VFS 

### Character Driver Stack

We are using existing kernel VFS as a abstraction layer and exposing driver operations to the app.

`App <-> Common File API <-> VFS <-> Char Driver | Bus Infrastructure <-> Device`

If VFS is kernel abstraction chosen for device then it is called char driver model and it is nothing
to do with the device and bus infrastructure it is sitting on.

### Block Driver Stack

Various subsytem is supported by kernel. Open and read a file from the disk. 
Open you get a file descriptor 

`System call -> File system layer to read that file data. -> Disk Driver`

File system is disk management or storage manager. It will internally fallback to driver.
USB pendrive : Operation of driver expose to block device subsystem.

List of subsystem is going on since kernel is not only bound to desktop or server but embedded can
also run.

### Network Driver Stack

Hand them over to Network device subsystem to manage these driver expose its service to protocol stacks
and those protocol stacks has socket API for application to communicate. 


Linux Kernel has various device management subsystem.

Example: 

- MTD Subsystem (especially flash storage Memory Techonolgy Devices)
- Input Subsystem 
- GPIO Subsystem
- ALSA Subsystem (Mainly for sound devices)

**Note**: Kernel abstraction is called kernel driver model.






















