---
title: "Linux Driver Model - Advance - Part III"
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

How peripheral driver will setup with mid-layer ? 

### Example 1: USB HIGH LEVEL DRIVER

```
static struct usb_driver my_usb_driver = {

    .probe = test_probe;
    .disconnect = test_disconnect;
    .id_table = dev_table;
};
```

when device is connected which is hot plugged then probe function will get called and when device
is removed then test _ disconnect will get called. 

id table is a pointer of type usb _ device _ id and this id table is supposed to be initialized with the
address of usb _ device _ id where high level driver is supposed to specific vendor id device id and 
class id with respect to usb standard.


It will hold the array of usb _ Device _ id which might have single or n number of usb devices containing
vendor and device id. And until that deice id is found probe should not called. 


When module is deployed it has its own initialize routine and we are supposed to pass this as argument
to register with mid layer.
module _ usb _ driver (my _ usb _ driver);

```
static struct usb_device_id dev_table[] = {
	{USB_DEVICE(VENDOR,DEVICE)},	/* device */
	{}				/* Null terminator (required) */
};


MODULE_DEVICE_TABLE(usb, dev_table);

/* structure to register with usb-core  */
static struct usb_driver my_usb_driver = {
	.name = "usb-test",
	.probe = test_probe,  // driver's init call
	.disconnect = test_disconnect, // driver's exit call
	.id_table = dev_table,// table of devices we are looking for
};

/* Helper Macro to replace module init/exit to get driver object registered
   with Usb core */
module_usb_driver(my_usb_driver);

```

init function --> usb-core--> when deivce find --> probe

driver intialization vs module initialization
probe vs module _ init function

sample code 

```
static int module_init()
{
    register_usb_Driver(&my_usb_driver);
    return 0;
}

static void module _exit()
{
    deregister_usb_driver();
}
```

### Example 2: I2C HIGH LEVEL DRIVER

As per i2c bus devices are identified by slave address.

On the bus if driver finds the device with slave id - 0x50 then call probe function.

I2C BUS specific address, kernel relies on something about device tree, if it decalres this device
is existing then kernel assume it exists and called the probe.

```

/*seek Access to device with address 0x50 and enumerated with string "24c08"*/
static struct i2c_device_id eeprom_ids[] = {
        { "24c08",0x50 }, // Device tree tells this device exists
	{ } // end of the list
};

static struct i2c_driver eeprom_driver = {
	.driver = {
		.name = "eeprom",
		.owner = THIS_MODULE,
	},
	.probe = eeprom_probe,
	.remove = eeprom_remove,
	.id_table = eeprom_ids,
};

module_i2c_driver(eeprom_driver);

```

### Example 3: SPI HIGH LEVEL DRIVER

In SPI, slaves are identify through chip select, the name with which the kernel identify the salve 
device name, driver should use the same name to claim the device. 

```

static struct spi_driver ad7879_spi_driver = {
	.driver = {
		.name	= "ad7879", // name with which the kernel identify the slave device name
		.owner	= THIS_MODULE,
		.pm	= &ad7879_pm_ops,
	},
	.probe		= ad7879_spi_probe,
	.remove		= ad7879_spi_remove,
};

module_spi_driver(ad7879_spi_driver);


```

### Example 4: PCI

DEVICES are idenified by 

```
/* list of objects describing device and vendor id's that this driver supports */
static struct pci_device_id dev_table[] = {
        {0x10ec, 0x8139, PCI_ANY_ID, PCI_ANY_ID},
	{} // end of the list
};

/* pci driver structure to register with pci core */
static struct pci_driver rtl_driver  = {
        .name                   = DRV_NAME,
        .id_table               = dev_table,
        .probe                  = drv_init,
        .remove                 = drv_exit,
};

/* helper macro that expands to init/exit functions that carry out
   register/unregister ops with pci core */

module_pci_driver(rtl_driver);

```

Driver inti fucntion are reentrant because it is called every time when matching device is found.

When slave is deteced, during the low level driver enumeration then device structure is getting
populated. 










