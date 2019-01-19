# User Manual

## Presentation

[![MLS800](assets/product-front-2.gif)](assets/large/product-front-2.png)

The MLS800 is a very easy to use MIDI controlled Loop Switcher with 8 independant audio loops. It adapts itself to your gear and setup, not the other way around!

--8<-- "_partials/features.md"

Great, now what ?  
The whole idea of a loop switcher is to enable or disable audio loops. Each loop consist of a send and a return signal, with something processing the signal in between.

```flow
in=>start: Previous loop
out=>end: Next loop
lc=>condition: Loop active ?
l=>inputoutput: External unit
lr=>inputoutput: Loop return

in->lc(yes,bottom)->l
lc(no)->lr
l(right)->lr
lr(right)->out
```

By chaining several loops, the MLS800 act as a router that directs your audio signal to up to 8 external processing units. Leaving you with 2^8 possibilies to route your signal !

## Know your gear

### Front panel

<style>
	circle.bubble-circle {
		fill: #EF5350;
		r: 30;
	}

	text.bubble-text {
		stroke: white;
		fill: white;
		text-anchor: middle;
		dominant-baseline: central;
		font-size: 1.5em;
	}

	polyline.bubble-line {
		fill: none;
		marker-end: url(#arrow);
		stroke: #EF5350;
		stroke-width: 4;
	}

	marker.bubble-line {
		fill: #EF5350;
	}
</style>

<svg xmlns="http://www.w3.org/2000/svg" xmlns:svg="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 1269 448" width="100%">
	<title>MLS800 front</title>
	<defs>
		<circle id="bubble-back" class="bubble-circle" />
		<text id="bubble-text" stroke="white" text-anchor="middle" dominant-baseline="central" fill="white" />
		<!-- arrow -->
		<marker id="arrow" class="bubble-line" markerWidth="10" markerHeight="10" refX="7" refY="3" orient="auto" markerUnits="strokeWidth">
			<path d="M0,0 L0,6 L9,3 z" />
		</marker>
	</defs>
	<g>
		<image href="../assets/manual-front.gif" id="mls800-front-background" y="100" width="100%" />
		<!-- Menu -->
		<polyline id="menu-line" class="bubble-line" points="30,400 58,258"/>
		<g transform="translate(30, 400)">
			<use href="#bubble-back" />
			<text class="bubble-text">1</text>
		</g>
		<!-- Edit -->
		<polyline class="bubble-line" points="180,400 158,258"/>
		<g transform="translate(180, 400)">
			<use href="#bubble-back" />
			<text class="bubble-text">2</text>
		</g>
		<!-- Main display -->
		<polyline class="bubble-line" points="370,40 340,170"/>
		<g transform="translate(370, 40)">
			<use href="#bubble-back" />
			<text class="bubble-text">3</text>
		</g>
		<!-- Editing indicator -->
		<polyline class="bubble-line" points="420,400 290,265"/>
		<g transform="translate(420, 400)">
			<use href="#bubble-back" />
			<text class="bubble-text">4</text>
		</g>
		<!-- Loop 1 / Up  -->
		<polyline class="bubble-line" points="600,40 650,190"/>
		<g transform="translate(600, 40)">
			<use href="#bubble-back" />
			<text class="bubble-text">5</text>
		</g>
		<!-- Loop 2 / Down  -->
		<polyline class="bubble-line" points="810,40 770,190"/>
		<g transform="translate(810, 40)">
			<use href="#bubble-back" />
			<text class="bubble-text">6</text>
		</g>
		<!-- Loop 3 to 8  -->
		<polyline class="bubble-line" points="950,400 1010,258"/>
		<g transform="translate(950, 400)">
			<use href="#bubble-back" />
			<text class="bubble-text">7</text>
		</g>
	</g>
</svg>

1. ++"Menu"++ / Power indicator
2. ++"Edit"++ / Mode indicator
3. Main display
4. Editing indicator
5. Loop ++1++ key / ++arrow-up++
6. Loop ++2++ key / ++arrow-down++
7. Loop ++3++ to ++8++ keys

### Back panel

![MLS800 back](assets/manual-back.gif)

1. Power supply input: 2mm ID, 5.5mm OD, positively centered. 9 to 12 VDC @ 1A minimum. Basically, any supply fitting an Arduino Uno will do.
2. Reset switch
3. USB port. Provides a Serial port and 1x MIDI In, 1x MIDI out ports. 
4. MIDI Input
5. MIDI Thru
6. Main audio input
7. Loops `1` to `8` send signal
8. Loops `1` to `8` return signal
9. Main audio output

!!! danger
	Always use the recommended power supply with the MLS800.  
	Do not exceed the maximum recommended voltage of 12VDC.  
	The USB port does *not* provide power to the unit.


## First use

On power on, the MLS800 applies the last active preset and displays the firmware version for a brief moment. The unit then enters into the `Playing` mode.

## Modes

The MLS800 acts around 3 modes :

```flow
playing=>operation: Playing
learning=>operation: Learning
editing=>operation: Editing

playing(right)->learning(right)->editing
```

* `Playing` : Normal use. Display the active preset.
* `Learning` : Waiting for a `Program Change` MIDI command. The main display and ++"Edit"++ are blinking.
* `Editing` : Editing a preset. The main display editing indicator and ++"Edit"++ are on.

You pass from one mode to another by pressing ++"Edit"++.  

### Playing

The MLS800 displays the active preset and listens for an incoming MIDI `Program Change` message. If received, the corresponding preset number will be applied.

!!! note
	You can manually change the active preset using ++arrow-up++ or ++arrow-down++.

### Learning

The MLS800 main display and ++"Edit"++ are blinking, while waiting for an incoming `Program Change` message. Upon receiving one, the current preset state will remain unchanged, and the unit will switch to `Editing` mode for the requested preset number.

!!! tip "Copying a preset"
	You can use the fact that the current state will remain unchanged to copy a preset to a new location.

!!! tip "Editing the current preset"
	Simply press ++"Edit"++ a second time while in `Learning` mode to edit the current preset.

### Editing

The editing indicator and ++"Edit"++ are on. Use keys ++1++ to ++8++ to change to the corresponding loop state. Validate your changes by pressing ++"Edit"++, or cancel them by pressing ++"Menu"++.

## Configuration

You can enter the configuration menu by pressing ++"Menu"++ while in `Playing` mode.

| Button 						| Navigation					| Edition			|
|-------------------------------|------------------------------:|------------------:|
|`Menu` 						| Exit (sub-)menu 				| Cancel edit 		|
|`Edit` 						| Enter (sub-)menu / Edit value | Save value 		|
|:arrow_up_small: (`Up`) 		| Navigate up 					| Increase value 	|
|:arrow_down_small: (`Down`) 	| Navigate down 				| Decrease value 	|

Editing a value works like in `Editing` mode. The editing indicator is on, ++"Edit"++ will save the value while ++"Menu"++ will cancel the changes.

| Menu 		| Description																			|
|-----------|---------------------------------------------------------------------------------------|
| `menu`	| Main menu. Press ++"Menu"++ to return to `Playing` mode.								|
| `midi`	| Configures MIDI.																		|
| --> `r`	| Configures MIDI Rx channel from `0` to `16`. `0` listen to all channels (Omni mode). 	|
| `dim`		| Configures the main display intensity, from `0` to `15`.								|
| `clr`		| Factory reset.																		|
| --> `yes`	| Proceeds with factory reset. After a few seconds, Clear done (`clrd`) is displayed.	|
| --> `no`	| Cancels the factory reset.															|

!!! warning
	Factory reset will erase the MLS800 configuration ==and all presets data==!

## Firmware update

Being open source, there are several ways you can get this done. Simply pick the one that suits you!

??? example "Flashing the firmware, the simple way"
	1. Download the [latest release](https://github.com/blemasle/mls800-firmware/releases/latest) [![Latest release](https://img.shields.io/github/release/blemasle/mls800-firmware.svg?maxAge=3600)](https://github.com/blemasle/mls800-firmware/releases/latest) and uncompress its content.
	2. Connect the MLS800 to your computer using a USB-B cable.
	3. Run `update.bat` on Windows, `update.sh` on Linux

??? example "I'm a command-line guy !"
	1. Download the [latest release](https://github.com/blemasle/mls800-firmware/releases/latest) [![Latest release](https://img.shields.io/github/release/blemasle/mls800-firmware.svg?maxAge=3600)](https://github.com/blemasle/mls800-firmware/releases/latest) and uncompress its content.
	2. Connect the MLS800 to your computer using a USB-B cable.
	3. Run `avrdude -C .avrdude.conf -c arduino -p m32u4 -b 57600 -D -P COM4 -U flash:w:MLS800.hex:i -vv`, replacing `COM4` by the actual MLS800 COM port on your machine

??? example "I'm a developer !"
	1. Clone @blemasle/mls800-firmware
	2. [Compile](software.md#compile) with the IDE of your choice (but seriously, ditch that Arduino IDE) using the `Arduino Micro` board
	2. Upload with your IDE or the command line above

--8<-- "_partials/uml.md"
