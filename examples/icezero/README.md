#Originally simple example project for the IceZero Board

<s>We are only interested in icezprog and icezprog-0x{offset} variants.</s>


The original example comes with its own IceZero programming tool (icezprog.c).

Original programming script by Kevin M. Hubbard can be found here:
https://github.com/blackmesalabs/ice_zero_prog

Which was then modified in the 2 parent forks, see those repositories for details.

The 2 different offset variants are provided in this fork.

<s>Quick and dirty hack to support using learn-fpga with trenz icezero.</s>
--------------------------------------------------------------------

Connect your IceZero with your Raspberry Pi or Raspberry Pi Zero W and boot
the Raspberry Pi from the SD card.

Copy the files in this directory onto the Raspberry Pi or mount them by nfs 
and build the IceZero programming tools (most probably via ssh):

**<u>oldway</u>**

    cd icotools/examples/icezero
    make icezprog
    make icezprog-0x20000  #for olimex 1kevb learn-fpga spiflash programs
    make icezprog-0x30000
    make icezprog-0x100000

or with new general programming tool with offset for trenz icezero and olimex 8kevb and 1kevb

**<u>newway</u>**

    cd icotools/examples/icezero
    make bbiceprog


**bbiceprog -h** for help

NB it really is 0x30000 and not 0x20000 as the fpga part used produces a larger 0 based image than on the icestick.

The advantage with using nfs is that you can switch on the overlay-fs support via raspi-config
and give your sd card better protection once you have settled on a stable way of working.

If you want to install these programs look at how the makefiles install_icezprog works
otherwise put them in your PATH or pick them up in your working directory with 
"./iceprog-0x30000 binfile" for example to load the learn-fpga execute from spiflash
image. Data is loaded at offset 0x1000000 in the learn-fpga examples using 
"./iceprog-0x100000 binfile" 

Since the stop gap mesaure for trenz icezero learn-fpga port I've also added olimex boards.

So now there is a generic version of the programmer with offset capability

   cd icezprog/examples/icezero
   make bbiceprog

olimex raspberrypi zerow/zero2w header pins assignments with olimex IDC JWF cable to 8kevb

        1   2
        3   4
        5   6
        7   8   ORG
        9   10  YEL
        11  12
        13  14
        15  16
        17  18
        19  20
        21  22
        23  24
        25  26
        27  28
    GRN 29  30
    VLT 31  32  BLK
    GRY 33  34
        35  36  WHT
    BLU 37  38
    RED 39  40

    BRN BROWN   N/C
    RED RED     GND
    ORG ORANGE  TX
    YEL YELLOW  RX
    GRN GREEN   CFG_DONE
    BLU BLUE    CFG_RST
    VLT VIOLET  CFG_SI
    GRY GREY    CFG_SO
    WHT WHITE   CFG_SCK
    BLK BLACK   CFG_SS


olimex raspberrypi zerow/zero2w header pins assignments with olimex IDC JWF cable to 1kevb

        1   2
        3   4
        5   6
        7   8   {TX NON JWF, wire to DIO UEXT see below}
        9   10  {RX NON JWF, wire to DIO UEXT see below}
        11  12
        13  14
        15  16
        17  18
        19  20
        21  22
        23  24
        25  26
        27  28
    GRN 29  30
    VLT 31  32  BLK
    GRY 33  34
        35  36  WHT
    BLU 37  38
    RED 39  40

    BRN BROWN   N/C
    RED RED     GND
    ORG ORANGE  N/C
    YEL YELLOW  N/C
    GRN GREEN   CFG_DONE
    BLU BLUE    CFG_RST
    VLT VIOLET  CFG_SI
    GRY GREY    CFG_SO
    WHT WHITE   CFG_SCK
    BLK BLACK   CFG_SS

olimex 1kevb + DIO (UART TX and RX on UEXT of DIO)

No need to connect GND as PI.GPIO.39 already conneted to 1kevb GND over JWF IDC Ribbon Header

    Layout of UEXT pins on DIO addon board

        1   2
    TXD 3   4 RXD
        5   6
        7   8
        9   10

    Crossed pair for UART serial comms
    PI.GPIO.8{TX}<->OLIMEX.EVB1K.DIO.UEXT.4{RXD}
    PI.GPIO.10{RX}<->OLIMEX.EVB1K.DIO.UEXT.3{TXD}

    or if you don't want to pay for DIO board

    Look for PIO2_9/TXD and PIO2_8/RXD on the botton row of GPIO1 of 1kevb
    PI.GPIO.8{TX}<->evb1k.GPIO1.16{RXD}
    PI.GPIO.10{RX}<->evb1k.GPIO1.14{TXD}

TODO: possibly never as only relevant to really big binary flashings
for top speed rewrite version for olimex boards and ice4pi using real spi hardware and document pinout that would entail for olimex
