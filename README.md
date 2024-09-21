This fork of icotools is only here for the Quick and Dirty hack needed for adding icezero pcb to my Learn-Fpga fork. Will find a better solution if icezero port is adpoted into the main tree Learn-Fpga was forked from.

Get hold of WiringPi if it's not already installed and then do the following on your raspberry pi.


cd examples/icezero/

make icezprog

make icezprog-0x830000

make icezprog-0x1000000


[Quick and Dirty Hack Directory](examples/icezero)
-------------------------------

Slightly more detailed notes [here](examples/icezero/README.md).