Zephyr Micropython module
#########################

Overview
********

This project adds `Micropython <micropython_github_>`_ as a submodule to zephyr, and exposes it
as it using zephyr's shell and filesystem. The Micropython instance is run in a shell thread,
and input/output is passed directly between the shell backend using zephyr shell bypass feature.

This approach differs from the existing `zephyr port <micropython_zephyr_port_github_>`_ as
we are adding Micropython as an addition to a zephyr application, rather than building a
Micropython application using zephyr.

The overhead of littlefs + minimal micropython is about 120K ROM and 20K RAM, this is without
optimizing readline and stdio which duplicate a lot of the logic of zephyr's shell module.

Setup
*****

* Follow the official `Getting Started <zephyr_rtos_getting_started_>`_
  guide.

* Set up a new zephyr workspace using the following commands

  ::

    west init -m https://github.com/bjarki-andreasen/zephyr-micropython
    west update

* Build and flash the sample application. If you want to be able to execute a
  python script from the filesystem, set up a filesystem partition for your
  board and enable CONFIG_FILE_SYSTEM. For examples see
  zephyr-micropython/samples/micropython/shell/boards/

  Overlays and configs for native_sim/native and nrf54l15dk/nrf54l15/cpuapp
  have been added for quick testing.

  ::

    west build -p -b <board> zephyr-micropython/samples/micropython/shell/
    west flash

Usage
*****

Open a terminal emulator or whichever method you use to access the zephyr shell.
To start a normal REPL, use the **python** command.

::

  uart:~$ python
  MicroPython aceafd9e98 on 2025-12-14; nrf54l15dk with nrf54l15/cpuapp
  >>>

Press CTRL+D to exit, or import sys and call sys.exit().

::

  uart:~$ python
  MicroPython aceafd9e98 on 2025-12-14; nrf54l15dk with nrf54l15/cpuapp
  >>> import sys
  >>> sys.exit()
  uart:~$

To run a python script from the filesystem, use the **python** command and pass
a path to the script as the only argument. A simple hello world script can be written to a
file using zephyr's **fs** shell commands.

::

    uart:~$ fs write /storage/test.py 70 72 69 6e 74 28 22 68 65 6c 6c 6f 20 77 6f 72 6c 64 22 29 d a
    uart:~$ fs cat /storage/test.py
    print("hello world")
    uart:~$ python /storage/test.py
    hello world
    uart:~$

.. _micropython_github:
   https://github.com/micropython/micropython

.. _micropython_zephyr_port_github:
   https://github.com/micropython/micropython/tree/master/ports/zephyr

.. _zephyr_rtos_getting_started:
   https://docs.zephyrproject.org/4.3.0/develop/getting_started/index.html
