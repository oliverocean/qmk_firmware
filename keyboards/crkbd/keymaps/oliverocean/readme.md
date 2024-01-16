# Build instructions
https://docs.qmk.fm/#/newbs_getting_started

install system packages and qmk
```zsh
# system packages
$ sudo pacman --needed -S git python-pip libffi

# QMK
$ sudo pacman -S qmk
```

setup repo and config
```zsh
# clone repo
$ git clone <repo on github>

# install udev rules and reboot
$ sudo cp ~/src/oo/qmk_firmware/util/udev/50-qmk.rules /etc/udev/rules.d/.
# note: udev rules can be skipped if you flash with another system (i.e. MacOS)

# config home
$ qmk setup -H <path to repo>
# yes to set qmk home and to clone submodules

```

create a keymap and compile
```zsh
# set default keeb
qmk config user.keyboard=crkbd

# set default keymap
qmk config user.keymap=oliverocean

# review config
$ qmk config
find.keymap=default
mass_compile.keymap=default
user.keyboard=crkbd
user.keymap=oliverocean
user.qmk_home=/home/poseidon/src/oo/qmk_firmware

# edit/modify keymap
nvim keyboards/crkbd/keymaps/oliverocean/keymap.c

# or create new keymap (if needed)
qmk new-keymap

# compile
qmk compile

# copy new `hex` file to `bins` folder for posterity (and to flash via another system)

```

Flash
```zsh
# Elite-C is an Atmel atmega32u4 chip, model ATm32U4DFU

$ qmk flash

# new boards may need to be force erased first

Checking file size of crkbd_rev1_oliverocean.hex                                                    [OK]
 * The firmware size is fine - 25716/28672 (89%, 2956 bytes free)
Flashing for bootloader: atmel-dfu
Bootloader Version: 0x00 (0)
Checking memory from 0x0 to 0x6FFF...  Empty.
Chip already blank, to force erase use --force.
Checking memory from 0x0 to 0x647F...  Empty.
0%                            100%  Programming 0x6480 bytes...
[ X  ERROR
Memory write error, use debug for more info.

$ dfu-programmer atmega32u4 erase --force
Erasing flash...  Success
Checking memory from 0x0 to 0x6FFF...  Empty.

# run flash again and it should work
$ qmk flash

Ψ Compiling keymap with make -r -R -f builddefs/build_keyboard.mk -s flash KEYBOARD=crkbd/rev1 KEYMAP=oliverocean KEYBOARD_FILESAFE=crkbd_rev1 TARGET=crkbd_rev1_oliverocean INTERMEDIATE_OUTPUT=.build/obj_crkbd_rev1_oliverocean VERBOSE=false COLOR=true SILENT=false QMK_BIN="qmk"

avr-gcc (GCC) 13.2.0
Copyright (C) 2023 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

Compiling: tmk_core/protocol/lufa/lufa.c                                                            [OK]
Compiling: tmk_core/protocol/usb_descriptor.c                                                       [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Class/Common/HIDParser.c                                       [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/AVR8/Device_AVR8.c                                        [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/AVR8/EndpointStream_AVR8.c                                [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/AVR8/Endpoint_AVR8.c                                      [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/AVR8/Host_AVR8.c                                          [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/AVR8/PipeStream_AVR8.c                                    [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/AVR8/Pipe_AVR8.c                                          [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/AVR8/USBController_AVR8.c                                 [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/AVR8/USBInterrupt_AVR8.c                                  [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/ConfigDescriptors.c                                       [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/DeviceStandardReq.c                                       [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/Events.c                                                  [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/HostStandardReq.c                                         [OK]
Compiling: lib/lufa/LUFA/Drivers/USB/Core/USBTask.c                                                 [OK]
Compiling: tmk_core/protocol/lufa/usb_util.c                                                        [OK]
Linking: .build/crkbd_rev1_oliverocean.elf                                                          [OK]
Creating load file for flashing: .build/crkbd_rev1_oliverocean.hex                                  [OK]
Copying crkbd_rev1_oliverocean.hex to qmk_firmware folder                                           [OK]
Checking file size of crkbd_rev1_oliverocean.hex                                                    [OK]
 * The firmware size is fine - 25716/28672 (89%, 2956 bytes free)
Flashing for bootloader: atmel-dfu
Bootloader Version: 0x00 (0)
Checking memory from 0x0 to 0x6FFF...  Empty.
Chip already blank, to force erase use --force.
Checking memory from 0x0 to 0x647F...  Empty.
0%                            100%  Programming 0x6480 bytes...
[>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>]  Success
0%                            100%  Reading 0x7000 bytes...
[>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>]  Success
Validating...  Success
0x6480 bytes written into 0x7000 bytes memory (89.73%).

```

Notes
```zsh
# convert from json to c
$ qmk json2c -o keymap.c keymap.json

# convert from c to json
$ qmk c2json -o keymap.json keymap.c
```

# eof
