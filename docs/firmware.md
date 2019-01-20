# Firmware

The project was developed using Arduino 1.6.x, and works with the latest version of Arduino and librairies. The traditional Arduino `.ino` is not an option to be followed for any project with some respect for clean code. As such, the projects is split across several files and ditching the Arduino IDE for more advanced one (VSCode, Visual Micro, ...) is strongly recommended.

## Compiling

Install the required libraries through Arduino's Library Manager or with the following script. You can omit the version but just in case, those were the ones used to build the firmware.

```sh
arduino --install-library "AS1115:1.1.1"
arduino --install-library "E24:1.1.0"
arduino --install-library "MCP23017:1.0.1"
arduino --install-library "MIDI Library:4.3.1"
arduino --install-library "MIDIUSB:1.0.3"
```

Build the sketch with your IDE or by running `arduino --verify --board arduino:avr:micro src/MLS800.ino`.

## Interrupts

All of the ATmega32U4 external interrupts are used

| Interrupt 		| Use				|
|-------------------|-------------------|
| `INT0`, `INT1` 	| MIDI port			|
| `INT2`, `INT3` 	| USB Serial port	|
| `INT6` 			| User input [^1]	|

[^1]: At the time of conception, `INT6` was unsupported by the Arduino framework. That explains code setting it up.

## Main loop

To offer the best response time possible to MIDI commands while playing, the main `loop` is kept simple and as fast as possible by not using any delays. Every delay needed - for instance for the user interface to blink - are run conditionally and use `millis()`.

## Menu

The MLS800 menu is kinda designed around the [action design pattern](https://en.wikipedia.org/wiki/Command_pattern). Each menu entry describes its content, its relation and how to respond to user inputs. For instance, pressing the ++"Edit"++ key in some cases saves the changes and calls for a `back` action.  
That pattern keeps things tidy, easy to read and change while being memory efficient. It also nicely split the menu features implementation from its UI.

## EEPROM memory structure

The stock schematics use a 24LC256 which provides 256Kb of EEPROM memory. Currently, only the first half of that memory is used, and badly aligned. In reality, only a byte is needed to store a preset, but the original idea was to implement more complex patches that could respond to `Control Change` MIDI messages on top of changing patches with `Program Change`. That idea was never implemented, and 128 presets being more than enough for my use, the memory layout stayed that way.

Address 		| Name					| Size (bytes) 		|Notes
----------------|-----------------------|------------------:|---------
`0x00`			| Main configuration	| 17				|
    			| `seed`				| 5					| Used to detect invalid and/or blank configuration
				| `version`				| 7					| Version string
				| `rxChannel`			| 1					| MIDI Rx channel [`0`-`16`], `0` for Omni Mode
				| `txChannel`			| 1					| MIDI Tx channel [`0`-`16`], unused
				| `patchNumber`			| 1					| Active path number
				| `currentState`		| 1					| Current loops state
				| `displayDim`			| 1					| Display intensity [`0`-`15` ]
`0x11`			| Reserved				| 47				| 
`0x40`			| Patch `0`				| 129				| Yep, this is bad design
				| `data`				| 1					| Patch `0` loops state
				| `cc 0` 				| 1					| `Control Change 0` sub state
				| `...`					|					|
				| `cc 127`				| 1					| `Control Change 127` sub state
`...`
`0x403F`		| Patch `128`			| 129				| Last patch [^2]

[^2]: But still plenty of room left

## Loops state

A loop state is mapped to a single byte, where each bit corresponds to the same loop number state (bit `0` to loop `0` etc). Sending the current state to reflect on the display is straightforward, but reflecting that state on audio loops is another story. 

On the MCP23017, port A bits 0 to 7 turn loops 8 to 1 ON, while bits 0 to 7 turn loops 1 to 8 OFF on port B. Bits manipulation are fast and easy so priority has been given to PCB layout to keep traces parallel and avoid routing the audio signal from one side of the PCB to another.
