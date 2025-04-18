# Using a Companion Computer with Pixhawk Controllers

PX4 running on Pixhawk-series flight controllers can connect to a companion computer using any free configurable serial port, including the Ethernet port (if supported).

See [Companion Computers](../companion_computer/README.md) for information about supported hardware and general setup.


## Companion Computer Software

The companion computer needs to run software that communicates with the flight controller, and which routes traffic to ground stations and the cloud.

Common options are listed in [Companion Computers > Companion Computer Setup](../companion_computer/README.md#companion-computer-software).

## Ethernet Setup

Ethernet is the recommended connection, if supported by your flight controller.
See [Ethernet Setup](../advanced_config/ethernet_setup.md) for instructions.

## Serial Port Setup

These instructions explain how to setup the connection if you're not using Ethernet.

### Pixhawk Configuration

PX4 is configured by default to connect to a companion computer connected to the `TELEM 2` serial port.
No additional PX4-side configuration should be required if you use this port

To enable MAVLink to connect on another port see [MAVLink Peripherals (GCS/OSD/Companion)](../peripherals/mavlink_peripherals.md) and [Serial Port Configuration](../peripherals/serial_configuration.md).

### Serial Port Hardware Setup

If you're connecting using a serial port, wire the port according to the instructions below.
All Pixhawk serial ports operate at 3.3V and are 5V level compatible.

:::warning
Many modern companion computers only support 1.8V levels on their hardware UART and can be damaged by 3.3V levels.
Use a level shifter.
In most cases the accessible hardware serial ports already have some function (modem or console) associated with them and need to be *reconfigured in Linux* before they can be used.
:::

The safe bet is to use an FTDI Chip USB-to-serial adapter board and the wiring below. This always works and is easy to set up.

TELEM2 | | FTDI | &nbsp;
--- | --- | --- | ---
1 | +5V (red)| | DO NOT CONNECT!
2 | Tx  (out)| 5 | FTDI RX (yellow) (in)
3 | Rx  (in) | 4 | FTDI TX (orange) (out)
4 | CTS (in) | 6 | FTDI RTS (green) (out)
5 | RTS (out)| 2 | FTDI CTS (brown) (in)
6 | GND     | 1 | FTDI GND (black)

### Serial Port Software setup on Linux

On Linux the default name of a USB FTDI would be like `\dev\ttyUSB0`. If you have a second FTDI linked on the USB or an Arduino, it will registered as `\dev\ttyUSB1`. To avoid the confusion between the first plugged and the second plugged, we recommend you to create a symlink from `ttyUSBx` to a friendly name, depending on the Vendor and Product ID of the USB device. 

Using `lsusb` we can get the vendor and product IDs.

```sh
$ lsusb

Bus 006 Device 002: ID 0bda:8153 Realtek Semiconductor Corp.
Bus 006 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 005 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 004 Device 002: ID 05e3:0616 Genesys Logic, Inc.
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 004: ID 2341:0042 Arduino SA Mega 2560 R3 (CDC ACM)
Bus 003 Device 005: ID 26ac:0011
Bus 003 Device 002: ID 05e3:0610 Genesys Logic, Inc. 4-port hub
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 001 Device 002: ID 0bda:8176 Realtek Semiconductor Corp. RTL8188CUS 802.11n WLAN Adapter
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
```

The Arduino is `Bus 003 Device 004: ID 2341:0042 Arduino SA Mega 2560 R3 (CDC ACM)`

The Pixhawk is `Bus 003 Device 005: ID 26ac:0011`

:::note
If you do not find your device, unplug it, execute `lsusb`, plug it, execute `lsusb` again and see the added device.
:::

Therefore, we can create a new UDEV rule in a file called `/etc/udev/rules.d/99-pixhawk.rules` with the following content, changing the idVendor and idProduct to yours.

```sh
SUBSYSTEM=="tty", ATTRS{idVendor}=="2341", ATTRS{idProduct}=="0042", SYMLINK+="ttyArduino"
SUBSYSTEM=="tty", ATTRS{idVendor}=="26ac", ATTRS{idProduct}=="0011", SYMLINK+="ttyPixhawk"
```

Finally, after a **reboot** you can be sure to know which device is what and put `/dev/ttyPixhawk` instead of `/dev/ttyUSB0` in your scripts.

:::note
Be sure to add yourself in the `tty` and `dialout` groups via `usermod` to avoid to have to execute scripts as root.
:::

```sh
usermod -a -G tty ros-user
usermod -a -G dialout ros-user
```
