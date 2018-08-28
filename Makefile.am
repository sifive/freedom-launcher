# Copyright 2018 SiFive, Inc
# SPDX-License-Identifier: Apache-2.0

bin_SCRIPTS = freedom-launch \
              launch_openocd-@MACHINE_NAME@ launch_qemu-@MACHINE_NAME@ \
              launch_simv-@MACHINE_NAME@

freedom-launch: scripts/launch @ELF2HEX@
	cat $< > $@
	chmod +x $@

# Generates a openocd configuration file that specific to a target machine.
$(libdir)/launch_openocd-@MACHINE_NAME@.cfg: @OPENOCDCFG_GENERATOR@ @MACHINE_NAME@.dtb
	$< --dtb $(filter %.dtb,$^) --output $@

# In order to run qemu that's actually compatible with a machine we must
# pass the march to launcher that coorespond to the hardware.
# This is handled by generating a makefile fragment, including it.
-include @MACHINE_NAME@.mk
@MACHINE_NAME@.mk: @MAKEATTRIBUTES_GENERATOR@ @MACHINE_NAME@.dtb
	$< --dtb $(filter %.dtb,$^) --output $@

# external tool.
@MACHINE_NAME@.dtb: @DTC@ @MACHINE_DTS@
	$< $(filter %.dts,$^) -o $@ -O dtb -I dts

launch_openocd-@MACHINE_NAME@: $(libdir)/launch_openocd-@MACHINE_NAME@.cfg
	echo "@OPENOCD@ -f ${libdir}/launch_openocd-@MACHINE_NAME@.cfg & @GDB@ program.elf -batch -ex \"set remotetimeout 240\" -ex \"target extended-remote localhost:3333\" -ex \"monitor reset halt\" -ex \"monitor flash protect 0 64 last off\" -ex \"load\" -ex \"monitor resume\" -ex \"monitor shutdown\" -ex \"quit\"" > $@
	chmod +x $@

MARCH_WIDTH=$(shell echo "${FRAMEWORK_BOARD_DTS_MARCH}" | sed 's/[^0-9]//g')
launch_qemu-@MACHINE_NAME@:
	echo "qemu${MARCH_WIDTH} -machine sifive_e -kernel program.elf -nographic" | sed 's:qemu32:@QEMU32@:' | sed 's:qemu64:@QEMU64@:' > $@
	chmod +x $@

launch_simv-@MACHINE_NAME@: scripts/launch-simv
	cat $< | sed 's:ELF2HEX:@ELF2HEX@:' | sed 's:VERILOG:@MACHINE_VERILOG@:' | sed 's:MACHINE:@MACHINE_NAME@:' > $@
	chmod +x $@

EXTRA_DIST = scripts/launch
EXTRA_DIST += scripts/launch-simv
EXTRA_DIST += README.md

# Extra clean targets
clean-local:
	-rm -rf $(libdir)/launch_openocd-@MACHINE_NAME@.cfg @MACHINE_NAME@.dtb
	-rm -rf freedom-launch launch_openocd-@MACHINE_NAME@
	-rm -rf launch_qemu-@MACHINE_NAME@ @MACHINE_NAME@.mk
	-rm -rf launch_simv-@MACHINE_NAME@
