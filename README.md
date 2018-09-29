# Freedom-Launcher
A launcher script for SiFive's Freedom platforms

    ./freedom-launch [--help] --elf INPUT.ELF --machine MACHINE-TARGET --fidelity [functional cycle-accurate all]

    ./freedom-launch --elf INPUT.ELF --machine MACHINE-TARGET --functional [qemu]

    ./freedom-launch --elf INPUT.ELF --machine MACHINE-TARGET --cycle-accurate [arty]

The fullpath is require for INPUT.ELF file. The initail MACHINE-TARGET support for functional qemu is sifive-hifive1.

## Building `freedom-launch` from a release

The best way to build `freedom-launch` is from the [latest release's
tarball](https://github.com/sifive/freedom-launcher/releases/download/v0.0.2/freedom-launcher-0.0.2.tar.gz),
as these releases have been tested.  `freedom-launch` uses the standard GNU
build flow:

    tar -xvzpf freedom-launcher-0.0.2.tar.gz
    cd freedom-launcher-0.0.2
    ./configure \
           --with-machine-name="MACHINE-TARGET" \
           --with-machine-dts="MACHINE-TARGET.dts"
    make

Building from the tarballs requires freedom-devicetree-tools package and the machine name and dts file.

## Building `freedom-launch` from git

The latest source for `freedom-launcher` can be found on [SiFive's
GitHub](https://github.com/sifive/freedom-launcher).  While the master branch is
always meant to be stable, there are no guarantees.  To build from
git sources you must regenerate the build scripts from their sources and
then follow the standard build flow

    git clone git://github.com/sifive/freedom-launcher.git
    cd freedom-launcher
    autoreconf -i
    ./configure \
           --with-machine-name="MACHINE-TARGET" \
           --with-machine-dts="MACHINE-TARGET.dts"
    make

