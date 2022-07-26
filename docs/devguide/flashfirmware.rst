.. _flashfirmware:

===========================
Build and Flash MicroPython
===========================

Dependencies
------------

- `CMake <https://cmake.org/>`_
- `Arm GCC <https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads>`_
- `git <https://git-scm.com/>`_
- `ninja <https://ninja-build.org/>`_
- `python <https://www.python.org/downloads/>`_
- `srecord <http://srecord.sourceforge.net/>`_
- `yotta <http://docs.yottabuild.org//>`_

Build MicroPython
-----------------

The `yotta <http://docs.yottabuild.org>`_ tool is used to build MicroPython,
but before that takes place additional files have to be generated by the
Makefile in preparation for the build, and additional data is added to the
hex file after.

Clone the repository and change directory to it::

  $ git clone https://github.com/bbcmicrobit/micropython

  $ cd micropython

Configure yotta to use the micro:bit target::

  yotta target bbc-microbit-classic-gcc-nosd@https://github.com/lancaster-university/yotta-target-bbc-microbit-classic-gcc-nosd

Run yotta update to fetch remote assets::

  yotta up

Start the build using the makefile::

  make all

The resulting ``firmware.hex`` can be found in the ``build/``
directory which can then be copied to the micro:bit.

The Makefile does some extra preprocessing of the source, which is needed only
if you add new interned strings to ``qstrdefsport.h``. The Makefile also puts
the resulting firmware at build/firmware.hex, and includes some convenience
targets.

Preparing MicroPython with a Python program
-------------------------------------------

Using ``tools/makecombinedhex.py`` you can combine the MicroPython firmware
with a Python script and produce a hex file ready for uploading to the
micro:bit.::

  ./makecombinedhex.py <firmware.hex> <script.py> [-o <combined.hex>]

The script will output to ``stdout`` if no output option (``-o``) is provided.

Using ``tools/hexlify.py`` you can turn a Python script into Intel HEX format
to be concatenated at the end of the MicroPython firmware.hex.  A simple header
is added to the script.::

  ./hexlifyscript.py <script.py>

It also accepts data on standard input.

Flashing the micro:bit
----------------------

The micro:bit mounts itself as a USB mass storage device named ``MICROBIT``.
When it detects that a .hex file has been copied to the USB drive, it will
flash it, and start running the program.
