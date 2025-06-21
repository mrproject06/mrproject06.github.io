---
title: "Linux Driver Model - Advance"
date: 2025-06-22 00:00:00  +0500
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

Idea is to write a driver for slave 1. As part of driver code it should know how to communicate to 
host controller or bus controller i.e. what kind of register configuration, operation it supports.
It should also know how to program bus controller for specific operation, such driver is called
`Bus Controller` driver.

This driver should be managing bus controller as this will be relaying data to particular slave which
is on the peripheral bus of either of type USB, PCI and SPI etc which has its own set of communication
protocol, hence bus controller should received the data or packet which is understood by bus as 
per bus protocol. In order to achieve this there is one more driver called `Bus Manager` driver.

For example slave 1 is storage device such as pendrive, hence in order to know what kind of functionality,
operations are possible with this slave. To support this `Slave Controller` driver is required.


Hence in summary, this driver architecture comprise of 3 following type of drivers.
-   1. Slave Controller
-   2. Bus Manager
-   3. Bus Controller

This complexity was not there for device which is memory mapped.

Suppose slave 1 can be usb mouse or keyboard then bus manager and bus controller can be re-used, only
change required is slave controller. Hence these 3 are implements as separate driver.

-   1. High Level (Specific to functionality and operation supported by slave)
-   2. Mid-Level (Implementation of bus communication protocol i.e. packet & de-packetization)
-   3. Low-Level (Responsible for how to program the bus controller)

These drivers will take instruction from application for example operation like read so and so file
from usb pendrive. 


High level driver convert file read request to usb command and forwarded to bus manager which then
create a packet as per protocol and forward to bus controller and deliver to slave 1. 

I2C adapter or I2C bus controller need to unique for 2 different SoC hence this is chip specific 
but I2C protocol is same, hence bus manager will remain same.

When it comes to driver development, possibility is to write high level and low level only as there
are any high chances mid level already provided by linux. 


### Example

```
┌───────────────────────────────┐
│      Slave Driver #1          │
│   ──────────────────────────  │
│      High Level Driver        │
└──────────────┬────────────────┘
               ⇅
┌──────────────▼────────────────┐
│          SPI Core             │
│   ──────────────────────────  │
│      Mid Level Driver         │
└──────────────┬────────────────┘
               ⇅
┌──────────────▼────────────────┐
│       SPI Controller Drv      │
│   ──────────────────────────  │
│       Low Level Driver        │
└──────────────┬────────────────┘
               ⇅
┌──────────────▼────────────────┐
│        SPI Controller         │
└──────────────┬────────────────┘
               ⇅
       ┌───────┴───────┐
┌──────▼──────┐ ┌─────▼──────┐
│Slave #1     │ │Slave #2    │
└─────────────┘ └────────────┘



```
Bus controller should be initialize during the boot time then it should be static linked to the 
kernel. 

Job of bus controller as soon as it get initialize is to confgure the chipset to do bus scan or bus
probe. Its a hardware feature which can scan the bus to identify the slave i.e. enumeration and 
no of slave. 

And this driver should set up the transmission and reception and also notify its presence to mid
layer driver. 

Slave controller can be loadable module as when required it can be initialized.

