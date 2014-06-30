###Setup for STM32F429I-DISCO on Mac OS X Mavericks.

- http://grafixmafia.net/updated-using-the-stm32f4-discovery-board-with-mac-osx-10-9-mavericks/
- https://github.com/crimsonwoods/mirb-stm32f4discovery

###Build stlink utility from git.
```
$ brew install libusb autogen automake wget pkg-config [ as needed... ]
$ git clone https://github.com/texane/stlink.git
$ cd stlink
$ ./autogen.sh
$ ./configure
$ make
$ sudo make install
```

Test to be sure it finds the board.
```
$ st-util
2014-06-07T08:51:21 INFO src/stlink-usb.c: -- exit_dfu_mode
2014-06-07T08:51:21 INFO src/stlink-common.c: Loading device parameters....
2014-06-07T08:51:21 INFO src/stlink-common.c: Device connected is: F42x and F43x device, id 0x10036419
2014-06-07T08:51:21 INFO src/stlink-common.c: SRAM size: 0x30000 bytes (192 KiB), Flash: 0x200000 bytes (2048 KiB) in pages of 16384 bytes
Chip ID is 00000419, Core ID is  2ba01477.
Target voltage is 2883 mV.
Listening at *:4242...
```
Use ctrl-C to exit st-util.

###Flashing the discovery kit.
Command format is as follows.
```
st-flash [--reset] {read|write} path addr <size>
```
STM32F4 flash starts at 0x0800 0000 (kit has 2Megs).
```
$ st−flash write output.bin 0x8000000
```
