
#Originally simple example project for the IceZero Board
We are only interested in icezprog and icezprog-0x{offset} variants.
--------------------------------------------------------------------

The original example comes with its own IceZero programming tool (icezprog.c).

Original programming script by Kevin M. Hubbard can be found here:
https://github.com/blackmesalabs/ice_zero_prog

Which was then modified in the 2 parent forks, see those repositories for details.

The 2 different offset variants are provided in this fork.

Quick and dirty hack to support using learn-fpga with trenz icezero.
--------------------------------------------------------------------

Connect your IceZero with your Raspberry Pi or Raspberry Pi Zero and boot
the Raspberry Pi from the SD card.

Copy the files in this directory onto the Raspberry Pi or mount them by nfs 
and build the IceZero programming tools (most probably via ssh):


    cd icotools/examples/icezero
    make icezprog
    make icezprog-0x830000
    make icezprog-0x1000000

NB it really is 0x830000 and not 0x820000 as the fpga part used produces a larger 0 based image than on the icestick.

The advantage with using nfs is that you can switch on the overlay-fs support via raspi-config
and give your sd card better protection once you have settled on a stable way of working.

If you want to install these programs look at how the makefiles install_icezprog works
otherwise put them in your PATH or pick them up in your working directory with 
"./iceprog-0x830000 binfile" for example to load the learn-fpga execute from spiflash
image. Data is loaded at offset 0x1000000 in the learn-fpga examples using 
"./iceprog-0x1000000 binfile" 

This is a stop gap mesaure and if the trenz icezero learn-fpga port is adpoted I'll probably fix things up.