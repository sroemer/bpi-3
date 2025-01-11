# bpi-f3_doc

Some (hopefully) useful stuff related to the
[Banana Pi BPI-F3](https://docs.banana-pi.org/en/BPI-F3/BananaPi_BPI-F3) development board.
The BPI-F3 is a development board based on an [SpacemiT K1](https://docs.banana-pi.org/en/BPI-F3/SpacemiT_K1_datasheet)
8-core RISC-V chip.

There is some additional information in my [posts related to the BPI-F3](https://senvang.org/tags/bpif3/) on my
website.

## Hardware

Here some notes related to the hardware, which I did not find in the official documentation or which weren't clear.

### Serial console

The boards provides some ouput via the `UART0 Debug` connector. A USB to serial adapter (3.3V TTL) is required and the
settings are `115200-8-N-1`.

On a new device with empty eMMC and no SD card inserted the output looks as shown below:

```
␀sys: 0x0
try sd...
bm:3
ERROR:   CMD8
ERROR:   sd f! l:76
bm:0
ERROR:   emmc: invalid bootinfo magic code:0x0
ERROR:   invalid bootinfo image.
ERROR:   entering download mode.
ROM: usb download handler
usb2d_initialize : enter
Controller Run
```

### Boot select switch

The boot device (SD, eMMC, ...) can be selected via the DIP switches `SW1`. The function of the switches is not well
documented in the official documentation (but the information is shown in the
[BPI-F3 schematic](https://raw.githubusercontent.com/sroemer/bpi-3/refs/heads/main/doc/bpi-f3_schematic.pdf) provided in the Banana Pi
documentation).

The factory settings work for booting from SD card and eMMC (with the SD card having priority).

The [BPI-F3 schematic](https://raw.githubusercontent.com/sroemer/bpi-3/refs/heads/main/doc/bpi-f3_schematic.pdf) also states 2 resistors
related to switches 3 and 4 are as `NC/10k` and those are missing / not connected. Therefore switches 3 and 4 have no
function.

I created following drawing and table to clarify the function of the switches:

```
# BPI-F3 SW1 functions

-------------
|   off   on |
| 1 -o------ | - boot select 1 (QSPI_DATA0)
| 2 -o------ | - boot select 2 (QSPI_DATA1)
| 3 -o------ | - not connected
| 4 -o------ | - not connected
-------------
     ^---------- factory defaults (all 4 switches set to off)

Boot select:
     1  |  2  | function
    ====|=====|====================
    off | off | TF card -> eMMC (factory default)
    off | on  | TF card -> SPI NAND
    on  | off | TF card -> SPI NOR
    on  | on  | TF card only
```

## Links

### Banana Pi

- [Banana Pi Documentation](https://docs.banana-pi.org/en/BPI-F3/BananaPi_BPI-F3)

### Bianbu Linux (Board Support Package provided by `SpacemiT`

- [Bianbu Linux Downloads](http://archive.spacemit.com/image/k1/version/bianbu/)
- [Bianbu Linux Documentation](https://bianbu-linux.spacemit.com/en/)
- [Bianbu Linux Git repositories](https://gitee.com/bianbu-linux)

### Gentoo Linux

- [Gentoo Image](https://dev.gentoo.org/~lu_zero/riscv/) (inofficial build from `lu_zero`)
