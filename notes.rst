*************
Notes on CVA6
*************


Prerequisite
============

.. code::

    > verilator --version
    Verilator 4.200 2021-03-12 rev v4.200


.. code::

    export RISCV=/opt/riscv


clone, build and install git@github.com:riscv/riscv-tools.git
(outside of CVA6 repo)


Hello world
===========

Elaboration with verilator
^^^^^^^^^^^^^^^^^^^^^^^^^^
.. code::

    make verilate

Build software
^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code::

    sabbari@sabbari-VirtualBox [~/work/cva6] ± master ?:2 ✗                                                                                                                                  [12:23:41]
    > mkdir mysoft
    mkdir: created directory 'mysoft'
    sabbari@sabbari-VirtualBox [~/work/cva6] ± master ?:2 ✗                                                                                                                                  [12:24:04]
    > cd mysoft
    /home/sabbari/work/cva6/mysoft
    sabbari@sabbari-VirtualBox [~/.../cva6/mysoft] ± master ?:2 ✗                                                                                                                            [12:24:08]
    > echo '
    \  #include <stdio.h>
    \
    \  int main(int argc, char const *argv[]) {
    \      printf("Hello CVA6!\\n");
    \      return 0;
    \  }' > hello.c
    sabbari@sabbari-VirtualBox [~/.../cva6/mysoft] ± master ?:3 ✗                                                                                                                            [12:24:11]
    > riscv64-unknown-elf-gcc hello.c -o hello.elf
    sabbari@sabbari-VirtualBox [~/.../cva6/mysoft] ± master ?:3 ✗                                                                                                                            [12:24:22]
    > ls
    hello.c  hello.elf


Simulation
^^^^^^^^^^
.. code::

    sabbari@sabbari-VirtualBox [~/work/cva6] ± master ?:3 ✗
    > work-ver/Variane_testharness $RISCV/riscv64-unknown-elf/bin/pk mysoft/hello.elf
    This emulator compiled with JTAG Remote Bitbang client. To enable, use +jtag_rbb_enable=1.
    Listening on port 39917
    bbl loader
    Hello CVA6!\n/opt/riscv/riscv64-unknown-elf/bin/pk completed after 2633204 cycles
    CPU time used: 267321.67 ms
    Wall clock time passed: 267662.70 ms

Dhrystone benchmark
===================

Spike
^^^^^
.. code::

    sabbari@sabbari-VirtualBox [~/work/cva6] ± master U:2 ?:4 ✗                                                                                                                              [15:06:33]
    255 > spike /opt/riscv/target/share/riscv-tests/benchmarks/dhrystone.riscv
    Microseconds for one run through Dhrystone: 392
    Dhrystones per Second:                      2550
    mcycle = 196024
    minstret = 196029

Ariane
^^^^^^
.. code::

    sabbari@sabbari-VirtualBox [~/work/cva6] ± master U:2 ?:4 ✗                                                                                                                              [15:06:50]
    > work-ver/Variane_testharness /opt/riscv/target/share/riscv-tests/benchmarks/dhrystone.riscv
    This emulator compiled with JTAG Remote Bitbang client. To enable, use +jtag_rbb_enable=1.
    Listening on port 37763
    Microseconds for one run through Dhrystone: 522
    Dhrystones per Second:                      1915
    mcycle = 261116
    minstret = 196029
    /opt/riscv/target/share/riscv-tests/benchmarks/dhrystone.riscv completed after 435434 cycles
    CPU time used: 48697.94 ms
    Wall clock time passed: 49024.01 ms

Ariane with waves
^^^^^^^^^^^^^^^^^

.. code::
    make verilate DEBUG=1
    work-ver/Variane_testharness --vcd=dhrystone.vcd /opt/riscv/target/share/riscv-tests/benchmarks/dhrystone.riscv
