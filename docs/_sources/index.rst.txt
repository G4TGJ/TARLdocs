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

This library is a set of source files for AVR processors that provide functionality of use in amateur radio applications, although many of the files could also be used in
non-radio systems.

These files have been tested on the ATTiny85, ATtiny817 and ATMega328P. They should work with most tinyAVR 1-series, mega and xmega devices.

When adding these files to your project in Atmel Studio you should add them as links rather than copying them over. This way you can easily update the library when I issue
new versions. You should add as links the C files that you need so that they are compiled. You can also link to the header files but this does not bring them into the compilation.
For the compiler to find them you need to add ``../../../TARL`` and ``..`` to ``Project Properties/Toolchain/C Compiler/Directories``.

The source code is available at `TARL <https://github.com/G4TGJ/TARL>`_. A good way to learn how to use the library is to look at tHREE projects which use the library: `Serial817 <https://github.com/G4TGJ/Serial817>`_,
`TATC <https://github.com/G4TGJ/TATC>`_ and
`FreqGen5351 <https://github.com/G4TGJ/FreqGen5351>`_. You can, of course, use these projects as the basis for your project - feel free to clone or fork. Just remember that if you distribute
your program in binary form you must also make the source code available under the GPL. I would welcome pull requests with any modifications you may make.

Index
==================

* :ref:`genindex`

Drivers
=======

config.h
--------

All source files include ``config.h``. It is used to customise the library for specific hardware, e.g. IO ports, I2C address, frequency ranges.

``config.h`` is not part of the library but should be provided as part of your project. For each driver I have listed the defintions required with example values - these will
obviously have to match your requirements and hardware.

main.c and main.h
-----------------

Your program requires a ``main.c`` (although you can name it anything you like) which contains ``main()`` which should call the init function for each driver you are using and then
it should start a continuous loop. This will continually call into those drivers that require to do regular processing (e.g. ``morseScanPaddles()`` in the morse keyer. You obviously
have to include your own application code here too.

Some drivers include ``main.h`` which defines the functions that the driver calls to feed back into the main program e.g. the morse keyer will call ``keyDown()`` whenever it is 
sending a dot or a dash.


io.c and io.h
-------------

Many of the drivers require an I/O driver e.g. to read the state of a switch. The functions should be declared in ``io.h`` and implemented in ``io.c``. Again, for each driver I have
listed the required functions.

CAT
---

CAT control simulating the Yaesu FT450D. Requires the serial driver.

Requires the main application to implement a number of functions for setting VFO frequency, RIT etc.

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

.. code-block:: c
	:caption: Example config.h definitions for the CAT driver

	// Time between scans of the CAT interface
	#define CAT_CHARACTER_DELAY 10

Required functions to provide in ``main.c`` and ``main.h``:

.. c:function:: void     setVFOFrequency( uint8_t vfo, uint32_t freq )

	Sets a VFO frequency.

	:param vfo: VFO number
	:param freq: Frequency

.. c:function:: uint32_t getVFOFreq( uint8_t vfo )

	Returns a VFO's frequency.

	:param vfo: VFO number

.. c:function:: uint32_t getCurrentVFOFreq()

	Returns the current VFO's frequency.

.. c:function:: uint32_t getOtherVFOFreq()

	Returns the other VFO's frequency.
	e.g. if the current VFO is A, then return B's frequency.

.. c:function:: int16_t  getCurrentVFOOffset()

	Returns the current VFO's offset.

.. c:function:: bool     getCurrentVFORIT()

	Returns the current VFO's RIT setting.

.. c:function:: bool     getCurrentVFOXIT()

	Returns the current VFO's XIT setting.

.. c:function:: int16_t  getOtherVFOOffset()

	Returns the other VFO's offset.

.. c:function:: bool     getOtherVFORIT()

	Returns the other VFO's RIT setting.

.. c:function:: bool     getOtherVFOXIT()

	Returns the other VFO's XIT setting.

.. c:function:: uint8_t  getCurrentVFO()

	Returns the current VFO.

.. c:function:: void     setCurrentVFO( uint8_t vfo )

	Sets the current VFO.

	:param vfo: VFO number

.. c:function:: void     setCurrentVFORIT( bool bRIT )

	Set the current VFO in or out of RIT.

	:param bRIT: true if going into RIT

.. c:function:: bool     getVFOSplit()

	Return true if in split VFO mode.

.. c:function:: void     setVFOSplit( bool bSplit )

	Sets split VFO mode.

	:param bSplit: true if setting split

.. c:function:: bool     getTransmitting()

	Return true if currently transmitting.

.. c:function:: void     vfoSwap()

	Swap the two VFOs.

.. c:function:: void     vfoEqual()

	Set the VFOs to be equal.

.. c:function:: void     setCurrentVFOOffset( int16_t rit )

	Set the current VFO's offset.

	:param rit: RIT value to set

.. c:function:: void     setCWReverse( bool bCWReverse )

	Sets CW reverse mode i.e. swaps sidebands.

	:param bCWReverse: true if setting CW reverse


Display
-------

Display driver. Requires the LCD driver.

If you are short of flash space then defining DISPLAY_DISABLE_SCROLLING will save some space. Obviously this disables
scrolling and ``displaySplitLine()`` is not available.

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

.. doxygenenum:: eCursorState

.. code-block:: c
	:caption: Example config.h definitions for the display driver

	// Define to disable scrolling
	#define DISPLAY_DISABLE_SCROLLING

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


I2C
---

Implements the Philips |I2C| (Inter Integrated Circuit) bus. Some manufacturers call it TWI (Two Wire Interface), presumably to avoid trademark infringement.

Currently only the master side is implemented. Multi-master buses are not supported.

.. |I2C| replace:: I\ :sup:`2`\ C

Files
^^^^^

.. describe:: i2c.h
    
    Header file.

.. describe:: i2c.c

    Implementation.
	
	Pulls in the required source file for the device. Supports ATMega, tinyAVR 1-series and devices that use the Universal Serial Interface (USI) to provide I2C (such as the ATTiny85).

Functions
^^^^^^^^^

.. doxygenfunction:: i2cInit
.. doxygenfunction:: i2cWriteRegister
.. doxygenfunction:: i2cReadRegister

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

.. code-block:: c
	:caption: Example config.h definitions for the LCD I2C driver

	// Address of the LCD display
	#define LCD_I2C_ADDRESS 0x27

.. code-block:: c
	:caption: Example config.h definitions for the LCD port driver

	// LCD Port definitions
	#define LCD_ENABLE_PORT PORTB
	#define LCD_ENABLE_DDR  DDRB
	#define LCD_ENABLE_PIN  PB3
	#define LCD_RS_PORT     PORTB
	#define LCD_RS_DDR      DDRB
	#define LCD_RS_PIN      PB4
	#define LCD_DATA_PORT_0 PORTD
	#define LCD_DATA_DDR_0  DDRD
	#define LCD_DATA_PIN_0  PD7
	#define LCD_DATA_PORT_1  PORTD
	#define LCD_DATA_DDR_1  DDRD
	#define LCD_DATA_PIN_1  PD4
	#define LCD_DATA_PORT_2  PORTD
	#define LCD_DATA_DDR_2  DDRD
	#define LCD_DATA_PIN_2  PD3
	#define LCD_DATA_PORT_3  PORTD
	#define LCD_DATA_DDR_3  DDRD
	#define LCD_DATA_PIN_3  PD2

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

.. code-block:: c
	:caption: Example io.c definitions for the morse keyer

	void ioInit()
	{
		// Initialise morse output
		MORSE_OUTPUT_DDR_REG |= (1<<MORSE_OUTPUT_PIN);

		// Paddle dot and dash pins as inputs with pull-ups
		MORSE_PADDLE_DOT_PORT_REG |= (1<<MORSE_PADDLE_DOT_PIN);
		MORSE_PADDLE_DASH_PIN_REG |= (1<<MORSE_PADDLE_DASH_PIN);
	}

	// Functions to read morse paddle inputs
	bool ioReadDotPaddle()
	{
		return !(MORSE_PADDLE_DOT_PIN_REG & (1<<MORSE_PADDLE_DOT_PIN));
	}

	bool ioReadDashPaddle()
	{
		return !(MORSE_PADDLE_DASH_PIN_REG & (1<<MORSE_PADDLE_DASH_PIN));
	}

Required functions to provide in ``main.c`` and ``main.h``:

.. c:function:: void displayMorse(char *text)

	Sends the characters being sent by the keyer to the application. Can be displayed on a screen, for example.

	:param text: Pointer to characters to display

.. c:function:: void keyDown(bool bDown)

	Sends the key state to the application so that the keyer can key the transmitter.

	:param ``bDown``: true if the key is down, false if it is up


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

.. code-block:: c
	:caption: Example config.h definitions for the oscillator driver

	// Oscillator chip definitions
	// I2C address
	#define SI5351A_I2C_ADDRESS 0x60

	// Transmit and receive clocks. Receive uses 2 clocks for quadrature.
	#define NUM_CLOCKS 3
	#define RX_CLOCK_A 0
	#define RX_CLOCK_B 1
	#define TX_CLOCK   2

	// The minimum and maximum crystal frequencies in the setting menu
	// Have to allow for adjusting above or below actual valid crystal range
	#define MIN_XTAL_FREQUENCY 24000000
	#define MAX_XTAL_FREQUENCY 28000000

	// The si5351a default crystal frequency and load capacitance
	#define DEFAULT_XTAL_FREQ	27000000
	#define SI_XTAL_LOAD_CAP SI_XTAL_LOAD_10PF

Pushbutton
----------

Pushbutton driver. Debounces the button detecting short and long presses.

Files
^^^^^

.. describe:: pushbutton.h
    
    Header file.

.. describe:: pushbutton.c

    Implementation.

Functions
^^^^^^^^^

.. doxygenfunction:: debouncePushbutton

.. code-block:: c
	:caption: Example use of the pushbutton driver

	// Debounce times in ms
	#define DEBOUNCE_TIME 200
	#define LONG_PRESS_TIME 1000

	// Keep track of pushbutton debounce state
	static struct sDebounceState debounceState;

	// Read and debounce the pushbutton
	bool shortPress, longPress;
	debouncePushbutton( readButton(), &shortPress, &longPress, DEBOUNCE_TIME, LONG_PRESS_TIME, &debounceState );

.. code-block:: c
	:caption: Example io.c definitions for the pushbutton driver

	bool readButton()
	{
		return !(BUTTON_PIN_REG & (1<<BUTTON_PIN));
	}

	// Configure all the I/O we need
	void ioInit()
	{
		BUTTON_PORT_REG |= (1<<BUTTON_PIN);
	}

Rotary
----------

Rotary driver. Handles a rotary control with pushbutton. Decodes the quadrature signal for rotation and debounces the button
detecting short and long presses.

Requires the pushbutton driver.

Files
^^^^^

.. describe:: rotary.h
    
    Header file.

.. describe:: rotary.c

    Implementation.

Functions
^^^^^^^^^

.. doxygenfunction:: readRotary

.. code-block:: c
	:caption: Example config.h definitions for the rotary driver

	// Time for debouncing the rotary pushbutton (ms)
	#define ROTARY_BUTTON_DEBOUNCE_TIME 100

	// Time for the rotary pushbutton press to be a long press (ms)
	#define ROTARY_LONG_PRESS_TIME 250

.. code-block:: c
	:caption: Example io.c definitions for the rotary driver

	void ioReadRotary( bool *pbA, bool *pbB, bool *pbSw )
	{
		*pbA =  !(ROTARY_ENCODER_A_PIN_REG & (1<<ROTARY_ENCODER_A_PIN));
		*pbB = !(ROTARY_ENCODER_B_PIN_REG & (1<<ROTARY_ENCODER_B_PIN));
		*pbSw = !(ROTARY_ENCODER_SW_PIN_REG & (1<<ROTARY_ENCODER_SW_PIN));
	}

	// Configure all the I/O we need
	void ioInit()
	{
		ROTARY_ENCODER_A_PORT_REG |= (1<<ROTARY_ENCODER_A_PIN);
		ROTARY_ENCODER_B_PORT_REG |= (1<<ROTARY_ENCODER_B_PIN);
		ROTARY_ENCODER_SW_PORT_REG |= (1<<ROTARY_ENCODER_SW_PIN);
	}

Serial
----------

Serial driver for chips, such as the ATMega328 or ATtiny817, that have a USART. Interrupt driven with transmit and receive buffers.

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

.. code-block:: c
	:caption: Example config.h definitions for the serial driver

	// // Serial port buffer lengths
	// Lengths should be a power of 2 for efficiency
	#define SERIAL_RX_BUF_LEN 32
	#define SERIAL_TX_BUF_LEN 64

