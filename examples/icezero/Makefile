
#all: icezero.bin
#
#prog: icezero.bin icezprog
#	./icezprog icezero.bin
#
#reset: icezprog
#	./icezprog .

all: clean-progs bbiceprog icezprog icezprog-0x20000 icezprog-0x30000 icezprog-0x100000 bbiceprog

bbiceprog: bbiceprog.c
	gcc -o bbiceprog -Wall bbiceprog.c -lwiringPi -lrt -lstdc++

icezprog: icezprog.c
	gcc -o icezprog -Wall icezprog.c -lwiringPi -lrt -lstdc++

icezprog-0x100000: icezprog-0x100000.c
	gcc -o icezprog-0x100000 -Wall icezprog-0x100000.c -lwiringPi -lrt -lstdc++

icezprog-0x30000: icezprog-0x30000.c
	gcc -o icezprog-0x30000 -Wall icezprog-0x30000.c -lwiringPi -lrt -lstdc++

icezprog-0x20000: icezprog-0x20000.c
	gcc -o icezprog-0x20000 -Wall icezprog-0x20000.c -lwiringPi -lrt -lstdc++

install_icezprog: icezprog
	cp icezprog          /usr/local/bin/
	cp icezprog-0x20000  /usr/local/bin/
	cp icezprog-0x30000  /usr/local/bin/
	cp icezprog-0x100000 /usr/local/bin/

icezero.blif: icezero.v memdata.dat defines.vh
	yosys -p 'synth_ice40 -top top -blif icezero.blif' icezero.v

icezero.asc: icezero.blif icezero.pcf
	arachne-pnr -d 8k -P tq144:4k -p icezero.pcf -o icezero.asc icezero.blif

icezero.bin: icezero.asc
	icetime -d hx8k -c 25 icezero.asc
	icepack icezero.asc icezero.bin

memdata.dat: generate.py
	python3 generate.py

defines.vh: memdata.dat

testbench: testbench.v icezero.v
	iverilog -o testbench testbench.v icezero.v ../../icosoc/common/sim_sram.v $(shell yosys-config --datdir/ice40/cells_sim.v)

testbench.vcd: testbench
	./testbench

clean-progs:
	rm -f bbiceprog icezprog icezprog-0x20000 icezprog-0x30000 icezprog-0x100000


clean:
	rm -f testbench testbench.vcd
	rm -f icezero.blif icezero.asc icezero.bin
	rm -f memdata.dat defines.vh

.PHONY: all prog reset clean

