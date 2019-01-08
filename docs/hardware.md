# Hardware

The MLS800 is built around two surface mounted PCB : a motherboard that holds the main unit and rear connectors, and a daugher board that contains elements the user interact with during use.  

Those two boards fit in a custom designed stainless case with self-clinching fasteners and digitial printing for nice, profesionnal-like finish.

## The motherboard

The MLS800 is based on the [Arduino Micro](https://store.arduino.cc/usa/arduino-micro) design. As such, it is powered by an [ATmega32U4](https://www.microchip.com/wwwproducts/en/ATmega32U4). The ATmega32U4 does not need an additional IC to interface with a USB port and provides 2 hardware serial ports out of the box. The MIDI protocol being a serial communication protocol, this choice made perfect sense.  

Connected to that processor are several ICs. A [MCP23017](https://www.microchip.com/wwwproducts/en/MCP23017) port expander drives two [ULN2803ADW](https://www.ti.com/store/ti/en/p/product?p=ULN2803ADW) NPN arrays. Each pair of NPN transistor drive one [V23079E1201B301](https://www.te.com/usa-en/product-6-1393788-8.html) bistable relays. This design allows to keep things simple on every level. PCB traces are kept parrallel and changes sides as few times as possible, while there is no complex bit manipulations to change loop states : port A (bit 0 to 7) of the MCP23017 turn the corresponding loop ON, while the other turn it off. The use of bistable relays allows to reduce power consumption, as power is only needed while changing their state. Finally, an [24LC256](https://www.microchip.com/wwwproducts/en/en010828) EEPROM is used for preset storage.

The power is supplied by 2 [LM340MP-5.0/NOPB](https://www.ti.com/store/ti/en/p/product/?p=LM340MP-5.0/NOPB) each capable of delivering 1.5A@5VDC.

![Main components iFixit style](assets/hardware-motherboard-ifixit.jpg)

<ul>
	<li style="color:red">
		<span style="color:black">The main ATmega32U4 processor</span>
	</li>
	<li style="color:orange">
		<span style="color:black">MCP23017 port expander</span>
	</li>
	<li style="color:green">
		<span style="color:black">24LC256 EEPROM</span>
	</li>
	<li style="color:yellow">
		<span style="color:black">2 ULN2803ADW NPN arrays</span>
	</li>
	<li style="color:lightblue">
		<span style="color:black">2 LM340 power supplier</span>
	</li>
</ul>

## The input board

The input board is simply a daugher board that connects directly to the motherboard. It contains a [LTC-5723HR](http://optoelectronics.liteon.com/upload/download/DS-30-96-124/C5723HR.pdf) 4 digits 7 segments display and 10 high quality illuminated [Apem MEC switches](https://www.apem.com/int/29-mec-switches) switches (5GSH93582 combined with 1ES096 caps).

![Daughterboard](assets/hardware-daughterboard.jpg)

The display and 8 of those are controlled by an [AS1115-BSST](https://ams.com/as1115), while the two remaining switches are controlled directly by the main processor. The AS1115 being capable of driving up to 8 7 segments, a neat trick was to use the 8 LEDs of the loop switches as if there were a fifth digit. Saving ATmega32U4 pins and reducting components at the same time !

## The case

