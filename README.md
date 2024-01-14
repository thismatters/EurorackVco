# Voltage Controlled Oscillator

A Voltage Contolled Oscillator (VCO) is the backbone module for subtractive synthesis. It generates an audio signal which can then be shaped by myriad filters and amplifiers. Without a VCO you don't have a synth.

The circuit here is based on the [Yusynth VCO](https://yusynth.net/Modular/EN/VCO/index.html) with minor tweaks to make it more Eurorack friendly. The changes I've made are thus:
* Selected resistors appropriate for +/-12V rails,
* Added polarity protection so that a backward power header doesn't fry anything (twin series schotty diodes: BAT54SL),
* Selected a modern dual NPN diode package for the exponential converter,
* Full surface mount design,
* Toggle switch on front face for sync hardness.

The Yusynth page is a treasure trove of information about this circuit that I am too amateur to claim as my own; please read that outstanding page before using this circuit. The page provides several other implementations of the circuit that you may find suitable.

The KiCAD project here uses the library/footprints [found in my companion repo](https://github.com/thismatters/EurorackKiCAD).

## Width

12hp on a standard 3U rack.

## Inputs

This VCO features dual inputs for 1V/oct, Linear FM, and Exponential FM, Sync, and PWM. A toggle switch on the front face allows the player to set sync hardness. Two potentiometers allow the setting of frequency offset: coarse and fine. Two potentiometers are devoted to level for each of the frequency modulators (linear and exponential). Another pair of potentiometers gives control over (inherent) pulse width and PWM input level.

## Outputs

Four wave shapes are simultaneously available:
* Sine
* Triangle
* Saw (falling)
* Square

## Fiddly bits

Three 100kOhm resistors (R3, R4, R5) should be selected to within 0.1% tolerance (of each other) for good input tracking.

Likewise, two diodes (D5 and D6) ought to be selected which have a closely matched bias voltage in order to ensure a good sine wave symmetry. Testing SMD diodes for bias voltage is no small feat, so I didn't really bother doing this on my test circuits and first printed boards and they seemed fine. I hasten to bet that manufacturing processes have improved to the extent that manual matching is no longer relevant; but that could also just be a justification for laziness!

The schematic shows an OPA2134, however the original circuit calls for an OPA2137. The OPA2134 is a higher quality chip for sound applications, but it is also more expensive and was out of stock when I went to orde; the OPA2137 is a suitable alternative.

The schematic calls for a `thermally_coupled_transistor_pair`; this is actually two components arranged carefully together. First a dual NPN transistor array (BCM847), then a thermco resistor which is nestled directly atop the transistor array package and the couple are blanketed in thermal grease so that the two components stay close to the same temperature.

## Adjustment

You'll need an oscilloscope, a CV keyboard, and ideally a tuner.

### Waveshape

1. Set the frequency potentiometers to their middle positions, set the FM and PWM level knobs to minimum (counterclockwise), and the PW knob to the middle position.
1. Connect the sawtooth output to the oscilloscope.
1. Power on the circuit.
1. Adjust RV10 until the sawtooth fills the full width of the trace (no horizontal line at top or bottom).
1. Adjust RV9 until the sawtooth is centered about 0V. (You may have to revisit RV10 to get this perfect)
1. Connect the triangle output to the oscilloscope.
1. Adjust RV9 some more to get the triangle shape just right.
1. Adjust RV11 until the triangle is centered around 0V. (You may have to revisit RV10 to get this perfect)
1. Connect the sine output to the oscilloscope.
1. The sine wave ought to look about right, but you may adjust RV9, RV11 (and possibly RV10) to hone this.


### Volt/Octave Tracking

1. Connect the CV source to the 1V/oct input, set the CV to 0V. If you're using a keyboard for this, hit C0.
1. Connect your tuner to the sine output.
1. Set CV to 0V, adjust the frequency knobs (coarse, then fine) until the tuner shows A1 (55Hz), then set the CV to 1V (press C1).
1. If the tuner shows A2 (110Hz) then you're all set, move on to the next step. Otherwise, if the pitch is sharp (>110Hz) adjust RV8 so that the pitch goes sharper (if flat go flatter)! Now go back to the prior step.
1. Repeat the prior _two_ steps this time jumping an additional octave. Once you've worked through all the octaves you have available then you're done!

| Tone | Frequency |
|------|-----------|
| A1   |   55 Hz   |
| A2   |   110 Hz  |
| A3   |   220 Hz  |
| A4   |   440 Hz  |
| A5   |   880 Hz  |
| A6   |   1760 Hz |
| A7   |   3520 Hz |
| A8   |   7040 Hz |

### Tuning

1. Disconnect all inputs from circuit.
1. Connect the tuner to the sine output.
1. Set the coarse frequency knob to the fully counterclockwise position (0), set the fine frequency knob to the middle position (0).
1. Adjust RV6 until you get a frequency of 16.2Hz.
1. Connect a CV keyboard to the 1v/oct input, then press A3 (220Hz).
1. Adjust RV6 until the frequency is 220Hz.


## Vendors

There are part numbers in the [BOM](vco.csv) for many of the parts (not for basic passives though) at the following vendors:

* [Mouser](https://www.mouser.com): Needs no introduction. Get your ICs from here (or [digikey](https://www.digikey.com)).
* [Tayda Electronics](https://www.taydaelectronics.com/): Good supplier for passive components; audio jacks, and potentiometers. Their audio jacks are slightly smaller than the thonkiconn from thonk.
* [Thonk](https://www.thonk.co.uk/): The only place I've found tempco resistors for sale! You're looking for the 1k resistor, their store doesn't support part number searches.
* [Love My Switches](https://lovemyswitches.com/): Has [really good knobs](https://lovemyswitches.com/anodized-aluminum-knob-the-lo-fi-1-4-smooth-shaft-12-5mm-od/) to go on those potentiometers!
* [OSHPark](https://oshpark.com/): Fast and (relatively) cheap PCB manufacturer. [The V1 circuit](https://oshpark.com/shared_projects/N1VjtYdp) works well, but you'll have to file down the toggle switch legs to fit the tiny through holes on the board.


## Changelog

- v1: first draft
- v2: Added polarity protection, bigger holes for toggle switch footprint, reorganized board for easier trace routing.
