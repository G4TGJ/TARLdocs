.. TARL documentation master file, created by
   sphinx-quickstart on Mon Jul 13 12:05:42 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

G4TGJ AVR Radio Library
=======================

This library is a set of source files for AVR processors that provide functionality of use in amateur radio applications, although many of the files could also be used in non-radio systems.

These files have been tested on the ATTiny85 and ATMega328P.

When adding these files to your project in Atmel Studio you should add them as links rather than copying them over. This way you can easily update the library when I issue new versions. You should add as links the C files that you need so that they are compiled. You can also link to the header files but this does not bring them into the compilation. For the compiler to find them you need to add their path in Project Properties

.. toctree::
   :maxdepth: 2
   :caption: Contents:



Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

I2C
---

Implements the Philips |I2C| (Inter Integrated Circuit) bus. Some manufacturers call it TWI (Two Wire Interface), presumably to avoid trademark infringement.

Currently only the master side is implemented.

.. |I2C| replace:: I\ :sup:`2`\ C

Files
^^^^^

.. describe:: i2c.h
    
    Header file.

.. describe:: i2c.c

    Implementation for devices with native 2-wire serial support (such as the ATMega328).

.. describe:: USI_TWI_Master.c
.. describe:: USI_TWI_Master.h

    Implementation for devices that use the Universal Serial Interface (USI) to provide I2C (such as the ATTiny85).

Functions
^^^^^^^^^

.. doxygenfunction:: i2cInit
.. doxygenfunction:: i2cSendRegister
.. doxygenfunction:: i2cReadRegister

