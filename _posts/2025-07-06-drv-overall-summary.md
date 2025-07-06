---
title: "Linux Driver Model - Summary"
date: 2025-07-06 00:00:00  +0500
categories: [Linux]
tags: [Driver]
---

How the hardware is interface with the system?

In Linux where to position your driver. 

They are very specific to hardware as many i2c implementation are there. 

I2C might be on PCI or on USB or direct memory mapped.

This is scope of customization.

x86  PCI is base on I/O bus.

SOC : Bus controller are directly memory mapped on System BUS.

Device Controller : Network Controller, Bluetooth, Video & Audio Controller


Bus Controller Driver is 3 layer
Device Controller is single layer

/drivers/i2c/buses -- these many drivers for chipset or various implementation of i2c apdater chipset
and all these driver will get compiled when chosen i2c adatper support.

Kernel image is built and booting this image, there would be init function in driver boot time 
all driver get intialized but only 1 among them physical hardware matching with compatilbily list.

This is wasting memory by intialization all driver. 

To address the only boot time corresponding driver should get intialized for which we have hardware.

What are the options ? 

Is this problem already solved anywhere else.

Mid layer  check the list of hardware currently detected and if matching found then it initialized
the corresponding high level driver.

hot plug intialization is present for mid-layer. 


How is this implemented ?

We program the high level driver to register with mid-layer with some detail like address, vendor id
device id through its init function. 

High Level -> Bus Manager -> If devices are found it is calling driver init.


Low Level should also do the same. 


Lets introduce one more layer under low level driver. 

That layer is called platform core it must provide interface for lld when that layer detects the 
hardware it should call the init function of low level driver exactly same as mid-layer.



Low level is passing to mid-layer


Platform Descriptor That layer need to be hard programmed. 


For each hardware platform this layer would be hard programmed. 

That layer is called Kernel BSP. 

How it is different from driver. 

It contains information about the hardware. 

arch/arm/kernel-bsp code for each family of SoC different kernel BSP

make ARCH=arm menuconfig

that bsp code is responsible for initializating low level driver.

No of SOC has increased and then kernel BSP code should be present. 

100 of folder to support the new SoC. 

Then they thought why to hardcode the hardware description in C code.

Adapating from PowerPC, hardware description in script and compile in binary file bootloader
to load kernel binary and this binary and now this BSP code can be modify to read this binary from
particular address.

To support SoC you don;t have to change kernel. 2.6 kernel 

dEVICE Tree : Device Tree Binary File by comp

Generic ARM BSP will read from specifc Device Tree in runtime. 

core implementation of BSP is common to reach out to device tree. 

++++
Platform Core SoC BUS MANAGER : especially intiialization drivers for those devices which are memory
mapped. 
++++
^
|
++++++
Kernel BSP : Kernel BSP to platform core. 

++++++

dEVICE TREE: prvoides info to Kernel BSP

++++++



