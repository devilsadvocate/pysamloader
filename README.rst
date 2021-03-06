
.. image:: https://img.shields.io/pypi/v/pysamloader.svg?logo=pypi
    :target: https://pypi.org/project/pysamloader

.. image:: https://img.shields.io/pypi/pyversions/pysamloader.svg?logo=pypi
    :target: https://pypi.org/project/pysamloader

.. image:: https://img.shields.io/travis/ebs-universe/pysamloader.svg?logo=travis
    :target: https://travis-ci.org/ebs-universe/pysamloader

.. image:: https://img.shields.io/coveralls/github/ebs-universe/pysamloader.svg?logo=coveralls
    :target: https://coveralls.io/github/ebs-universe/pysamloader

.. image:: https://img.shields.io/requires/github/ebs-universe/pysamloader.svg
    :target: https://requires.io/github/ebs-universe/pysamloader/requirements

.. image:: https://img.shields.io/pypi/l/pysamloader.svg
    :target: https://www.gnu.org/licenses/gpl-3.0.en.html



.. inclusion-marker-do-not-remove

.. raw:: latex

       \vspace*{\fill}

Introduction
------------

``pysamloader`` is a python library for writing flash on Atmel's ARM chips
via SAM-BA. Originally written years ago when Atmel's standard tools were
unavailable or unusable on Linux, it has been adapted to serve narrower but 
specific use cases.

Specifically, ``pysamloader`` is intended to be :

    - Simple, particularly for an end-user
    - Easily installable across platforms
    - Usable from within larger python applications or scripts

Currently, ``pysamloader`` seems to be reasonably stable on a tiny set of 
supported devices.

If you happen to use ``pysamloader``, or wish to use it, let me know along
with any feedback you might have to ensure the tool is stable, reliable, and
sufficiently versatile. Device support is easy enough to add, and I will do
so as the need (and more importantly, the ability to test on other devices)
presents itself. Pull requests are also welcome.

See the ``pysamloader/devices`` folder for included device support modules. 
Currently supported devices are :

.. documentedlist::
    :listobject: pysamloader.pysamloader.supported_devices
    :header: "Device" "Interface"

.. raw:: latex

    \clearpage
    \tableofcontents
    \clearpage


Features
--------

``pysamloader`` currently supports the following actions :

    - Write device flash
    - Optionally verify flash after writing
    - Optionally set the GPNVM bits to boot from flash after writing
    - Read and parse ChipID
    - Read Unique Identifier from Embedded Flash
    - Read Flash Descriptor

.. raw:: latex

       \vspace*{\fill}

Requirements & Installation
---------------------------

.. attention::
    You might want to take a look at the
    `pysamloader-gui <https://github.com/chintal/pysamloader-gui>`_ package
    as well. It provides a simple GUI. If you are using the binary packages,
    it can be installed and used independent of this.


``pysamloader`` should work on any platform which supports ``python``. It is 
best tested on Linux followed by on Windows (10 and 7).

``pysamloader`` supports both Python 2 (2.7.x) and Python 3 (>3.5). Python 2 
support is likely to be removed in the near future.

In general, ``pysamloader`` is expected to be pip-installed. It can be safely 
installed into a virtualenv. As long as you have a functioning python 
installation of sufficient version, installing ``pysamloader`` would be simply :

.. code-block:: console

    $ pip install pysamloader

If you require pre-built binaries, they are available for 64-bit Linux and 
Windows. However, be aware that these binaries are not thoroughly tested, 
and your mileage may vary based on your specific operating system and machine 
architecture. You will also have to manually copy the included ``devices`` 
folder to the correct location. (See below)

If you wish to develop, modify the sources, or otherwise get the latest 
version, it can be installed from a clone of the git repository (or from a 
source package) as follows :

.. code-block:: console

    $ git clone https://github.com/chintal/pysamloader.git
    $ cd pysamloader
    $ pip install -e .

The ``pysamloader/devices`` folder contains the included device support 
modules, each of which is a python file with a single class of the same name, 
containing device specific information about one device. This folder can be 
copied into a separate location where you can safely add, remove, or modify 
device configuration as needed. This step is generally optional, but will be 
required if you are using the binary packages. The location is that provided 
by ``user_config_dir`` of the python ``appdirs`` package, specifically : 

    - Linux : ``~/.config/pysamloader``
    - Windows : ``C:\Users\<username>\AppData\Roaming\Quazar Technologies\pysamloader``

The current ``pysamloader`` windows .msi installer will create this folder and
populate it as a part of the install process. 


.. raw:: latex

       \vspace*{\fill}


Usage
-----

The primary entry point for use of ``pysamloader`` is as a console script.

For those in a hurry, the following is a quick example of how to use the
script to burn ``app.bin`` to an ``ATSAM3U4E`` whose UART SAM-BA interface
is accessible on ``\dev\ttyUSB1``:

.. code-block:: console

    $ pysamloader --device ATSAM3U4E --port \dev\ttyUSB1 -g app.bin


Script usage and arguments are listed here. This help listing can also be
obtained on the command line with ``pysamloader --help``.

.. argparse::
    :module: pysamloader.cli
    :func: _get_parser
    :prog: pysamloader
    :nodefault:


.. raw:: latex

       \vspace*{\fill}


Links & Other Information
-------------------------

Known Issues
............

 - Writing to multiple flash planes is not currently supported. This
   application will always write to the first flash plane and will start at
   the beginning.
 - The use of xmodem send file to write flash doesn't seem to work. Flash is
   instead written using SAM-BA ``write_word`` commands, which is about 20
   times slower.

Future Directions
.................

 - Add support for ``libftdi``/``libd2xx``/``libusb`` based backend for cases
   where the device disables ``ftdi_sio`` for its normal operation.
 - Add hooks for device auto-detection. Do not even bother to probe blindly
   for SAM-BA - that is too dangerous. Instead rely on apriori knowledge of
   signatures of device configuration, including VID, PID, Manufacturer,
   Product, Serial Number, and USB endpoint descriptors.

Links
.....

The latest version of the documentation, including installation, usage, and
API/developer notes can be found at
`ReadTheDocs <https://pysamloader.readthedocs.io/en/latest/index.html>`_.

The latest version of the sources can be found at
`GitHub <https://github.com/ebs-universe/pysamloader>`_. Please use GitHub's features
to report bugs, request features, or submit pull/merge requests.

The principle author for ``pysamloader`` is Chintalagiri Shashank. The author can
be contacted if necessary via the information on the
`author's github profile <https://github.com/chintal>`_ . See the AUTHORS file
for a full list of collaborators and/or contributing authors, if any.

``pysamloader`` is distributed under the terms of the
`GPLv3 license <https://www.gnu.org/licenses/gpl-3.0-standalone.html>`_ .
A copy of the text of the license is included along with the sources.
