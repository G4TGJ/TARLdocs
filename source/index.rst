.. TARL documentation master file, created by
   sphinx-quickstart on Mon Jul 13 12:05:42 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

G4TGJ AVR Radio Library
=======================

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   index

This library is a set of source files for AVR processors that provide functionality of use in amateur radio applications, although many of the files could also be used in non-radio systems.

These files have been tested on the ATTiny85 and ATMega328P.

When adding these files to your project in Atmel Studio you should add them as links rather than copying them over. This way you can easily update the library when I issue new versions. You should add as links the C files that you need so that they are compiled. You can also link to the header files but this does not bring them into the compilation. For the compiler to find them you need to add their path in Project Properties

Index
==================

* :ref:`genindex`

Drivers
=======

config.h
--------
All source files include ``config.h``. It is used to customise the library for specific hardware, e.g. IO ports, I2C address, frequency ranges.

``config.h`` is not part of the library but should be provided as part of your project.

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
.. doxygenfunction:: i2cWriteRegister
.. doxygenfunction:: i2cReadRegister

CAT
---

CAT control simulating the Yaesu FT450D. Requires the serial driver.

Files
^^^^^

.. describe:: cat.h
    
    Header file.

.. describe:: cat.c

    Implementation.

Functions
^^^^^^^^^

.. doxygenfunction:: catInit
.. doxygenfunction:: catControl

Display
-------

Display driver. Requires the LCD driver.

Files
^^^^^

.. describe:: display.h
    
    Header file.

.. describe:: display.c

    Implementation.

Functions
^^^^^^^^^

.. doxygenfunction:: displayInit
.. doxygenfunction:: displayText
.. doxygenfunction:: displayCursor
.. doxygenfunction:: displaySplitLine
.. doxygenfunction:: displayUpdate

.. doxygenenum:: eCursorState

LCD
-------

Low level LCD display driver. Normally you would use the display driver rather than the LCD driver.

Files
^^^^^

.. describe:: lcd.h
    
    Header file.

.. describe:: lcd.c

    Implementation. Requires either ``lcd_i2c.c`` or ``lcd_port.c``.

.. describe:: lcd_i2c.c

	Use this with LCD displays connected over I2C.

.. describe:: lcd_port.c

    Use with LCD displays directly connected to the IO ports on the AVR controller.

Functions
^^^^^^^^^

.. doxygenfunction:: lcdInit
.. doxygenfunction:: lcdBegin
.. doxygenfunction:: lcdClear
.. doxygenfunction:: lcdHome
.. doxygenfunction:: lcdDisplayOff
.. doxygenfunction:: lcdDisplayOn
.. doxygenfunction:: lcdBlinkOff
.. doxygenfunction:: lcdBlinkOn
.. doxygenfunction:: lcdCursorOff
.. doxygenfunction:: lcdCcursorOn
.. doxygenfunction:: lcdScrollDisplayLeft
.. doxygenfunction:: lcdScrollDisplayRight
.. doxygenfunction:: lcdScrollLeftToRight
.. doxygenfunction:: lcdScrollRightToLeft
.. doxygenfunction:: lcdAutoscrollOn
.. doxygenfunction:: lcdAutoscrollOff
.. doxygenfunction:: lcdSetCursor
.. doxygenfunction:: lcdPrint

EEPROM
-------

EEPROM driver.

Files
^^^^^

.. describe:: eeprom.h
    
    Header file.

.. describe:: eeprom.c

    Implementation.

Functions
^^^^^^^^^

.. doxygenfunction:: eepromInit
.. doxygenfunction:: eepromRead
.. doxygenfunction:: eepromWrite


Millis
-------

Timer providing a count in milliseconds and delays in milliseconds or microseconds.

Files
^^^^^

.. describe:: millis.h
    
    Header file.

.. describe:: millis.c

    Implementation.

Functions
^^^^^^^^^

.. doxygenfunction:: millisInit
.. doxygenfunction:: millis
.. doxygenfunction:: delay

.. doxygendefine:: delayMicroseconds

Morse
-------

Morse keyer. Supports Iambic A, Iambic B and Ultimatic.

Files
^^^^^

.. describe:: morse.h
    
    Header file.

.. describe:: morse.c

    Implementation.

Functions
^^^^^^^^^

.. doxygenfunction:: morseInit
.. doxygenfunction:: morseGetWpm
.. doxygenfunction:: morseSetWpm
.. doxygenfunction:: morseScanPaddles
.. doxygenfunction:: morseSetTuneMode
.. doxygenfunction:: morseInTuneMode
.. doxygenfunction:: morseGetKeyerMode
.. doxygenfunction:: morseSetKeyerMode

.. doxygenenum:: eMorseKeyerMode

Oscillator
----------

Oscillator driver. Allows the setting of individual output frequencies and enabling quadrature mode.
Currently only supports the Si5351A chip with 3 outputs. CLK0 and CLK1 use PLL A and CLK2 uses PLL B.

Files
^^^^^

.. describe:: osc.h
    
    Header file.

.. describe:: si5351a.c

    Implementation for the Si5351A.

Functions
^^^^^^^^^

.. doxygenfunction:: oscInit
.. doxygenfunction:: oscSetFrequency
.. doxygenfunction:: oscClockEnable
.. doxygenfunction:: oscSetXtalFrequency

Serial
----------

Serial driver for chips, such as the ATMega328, that have a USART. Interrupt driven with transmit and receive buffers.

Files
^^^^^

.. describe:: serial.h
    
    Header file.

.. describe:: serial.c

    Implementation.

Functions
^^^^^^^^^

.. doxygenfunction:: serialInit
.. doxygenfunction:: serialTransmit
.. doxygenfunction:: serialTXString
.. doxygenfunction:: serialReceive
