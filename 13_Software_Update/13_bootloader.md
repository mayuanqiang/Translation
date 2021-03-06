# STM32 BootLoader


The code for the PX4 bootloader is available from the Github [Bootloader](https://github.com/px4/bootloader)repository.

## Supported Boards


* FMUv1 (PX4FMU, STM32F4)
* FMUv2 (Pixhawk 1, STM32F4)
* FMUv3 (Pixhawk 2, STM32F4)
* FMUv4 (Pixracer 3 and Pixhawk 3 Pro, STM32F4)
* FMUv5 (Pixhawk 4, STM32F7)
* TAPv1 (TBA, STM32F4)
* ASCv1 (TBA, STM32F4)


## Building the Bootloader

```
git clone https://github.com/PX4/Bootloader.git
cd Bootloader
make
```
After this step a range of elf files for all supported boards are present in the Bootloader directory.


## Flashing the Bootloader

>IMPORTANT: The right power sequence is critical for some boards to allow JTAG / SWD access. Follow these steps exactly as described. The instructions below are valid for a Blackmagic / Dronecode probe. Other JTAG probes will need different but similar steps. Developers attempting to flash the bootloader should have the required knowledge. If you do not know how to do this you probably should reconsider if you really need to change anything about the bootloader.

* Disconnect the JTAG cable
* Connect the USB power cable
* Connect the JTAG cable


## Using the right serial port



* On LINUX: `/dev/serial/by-id/usb-Black_Sphere_XXX-if00`
* On MAC OS: Make sure to use the cu.xxx port, not the tty.xxx port: `tar ext /dev/tty.usbmodemDDEasdf`

```
arm-none-eabi-gdb
  (gdb) tar ext /dev/serial/by-id/usb-Black_Sphere_XXX-if00
  (gdb) mon swdp_scan
  (gdb) attach 1
  (gdb) mon option erase
  (gdb) mon erase_mass
  (gdb) load tapv1_bl.elf
        ...
        Transfer rate: 17 KB/sec, 828 bytes/write.
  (gdb) kill
  ```

## Troubleshooting


If any of the commands above are not found, you are either not using a Blackmagic probe or its software is outdated. Upgrade the on-probe software first.+

If this error message occurs: `Error erasing flash with vFlashErase packet`

Disconnect the target (while leaving JTAG connected) and run

```mon tpwr disable
swdp_scan
attach 1
load tapv1_bl.elf```

This will disable target powering and attempt another flash cycle.