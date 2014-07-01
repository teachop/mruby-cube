###Setting Up for mruby-cube

TODO

###Flash Programming STM32F429I-DISCO from OS X

References:
- http://grafixmafia.net/updated-using-the-stm32f4-discovery-board-with-mac-osx-10-9-mavericks/
- https://github.com/texane/stlink/tree/master/doc/tutorial

For interface to the kit built-in jtag port, use this stlink utility from git.
```
$ brew install libusb autogen automake wget pkg-config [ as needed... ]
$ git clone https://github.com/texane/stlink.git
$ cd stlink
$ ./autogen.sh
$ ./configure
$ make
$ sudo make install
```

Do a quick test to be sure stlink finds the board.  Use ctrl-C to exit st-util.
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

For simply flashing the discovery kit, the command format is as follows.  STM32F4 flash starts at 0x0800 0000 (this kit has 2Megs).
```
st-flash [--reset] {read|write} path addr <size>
```
```
$ st−flash write output.bin 0x8000000
```

###Building mruby for the STM32F429I-DISCO

References:
- https://github.com/crimsonwoods/mirb-stm32f4discovery
- https://github.com/mruby/mruby/tree/master/examples/targets

First clone the mruby repository in the same parent folder containing the mruby-cube repository.

The mruby project is designed for embedding and easily supports cross-building.  All that is required to build mruby (rake) is to use the local build_config.rb.  The local config will be passed to the mruby build using the rake task "mruby":
```
mruby-cube $ rake mruby
```

###Compiling the STM32CubeF4 HAL Drivers on OS X

References:
- https://github.com/fboris/STM32CubeMx_GNU_toolchain

Unzip the STmicroelectronics STM32Cube_FW_F4_xxxx in the same parent folder containing the mruby-cube repository.  Within the mruby-cube folder, build the library using the Rakefile included here.
```
mruby-cube $ rake hal
```



