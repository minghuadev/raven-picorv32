# ---- Test patterns for project raven ----

.SUFFIXES:

PATTERN = raven_adc raven_adc2 client

all:  ${PATTERN:=.vcd}

hex:  ${PATTERN:=.hex}

%.vvp: %_tb.v %.hex
	iverilog -I. -I.. $< -o $@

%.vcd: %.vvp
	vvp $<

%.elf: %.c ../sections.lds ../start.s
	/ef/apps/bin/riscv32-unknown-elf-gcc -march=rv32imc -Wl,-Bstatic,-T,../sections.lds,--strip-debug -ffreestanding -nostdlib -o $@ ../start.s $<

%.hex: %.elf
	/ef/apps/bin/riscv32-unknown-elf-objcopy -O verilog $< /dev/stdout | sed -e '1 s/@00000000/@00100000/; 2,65537 d;' > $@

%.bin: %.elf
	/ef/apps/bin/riscv32-unknown-elf-objcopy -O binary $< /dev/stdout | tail -c +1048577 > $@


client: client.c
	gcc client.c -o client

# ---- Clean ----

clean:
	rm -f *.elf *.hex *.bin *.vvp *.vcd

.PHONY: clean hex all

