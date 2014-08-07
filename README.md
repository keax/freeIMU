Fork of FreeIMU for embedded framework
======================================

This fork allows you to play with a MPU6050 on :
* TI stellaris LM4F120XL.
* STM32F0 Discovery (STM32F051)

Prerequisite
------------

see prerequisites of embedded at https://github.com/r-3-t/embedded

Build
-----

git submodule update --init --recursive

cmake <freeIMU_for_embedded_path> -Dmcu=lm4f120xl

make

Run
---

openocd

arm-none-eabi-gdb freeimu_serial

> load

> continue

Tests
-----

Download and install processing (see http://www.varesano.net/projects/hardware/FreeIMU and https://www.processing.org)

./processing/processing processing/FreeIMU_cube/FreeIMU_cube.pde

Run

freeIMU
=======

The bzr export of the FreeIMU 4.0, originally designed by Fabio Varesano.

Please note (and especially for Mac OSX): You will need to download and place PyPy (pypy.org) version 1.9 (or possibly higher) in the correct directories, as github doesn't seem to want to import all the binaries correctly.
You will also need wxPython 2.9 (for the Mac OSX client) or higher, and python 2.7.3 or higher (again, the Mac OSX client)

