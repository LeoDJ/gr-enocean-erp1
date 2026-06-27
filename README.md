# GNU Radio Decoder for EnOcean Radio Protocol 1

This is a rudimentary decoder for EnOcean ERP1 made in GNU Radio.  

The 125 kbps ASK demodulation was a bit of a challenge and can probably be improved upon.  
The decoding of the demodulated bitstream is done in a python module block (including CRC check).

## Instructions
1. Setup input source in flow graph
1. Run flow graph
1. Set squelch slider so low that it lets everything pass and then a few dB higher (so only actual transmissions get through)
1. Set Attack and Gain sliders so the conditioned signal looks like in the screenshot below (the defaults might be ok, idk)
1. Play around with TED Gain and Loop BW, if the blue line in "Symbol Sync Debug" jumps around a lot or doesn't get a sync at all
1. Then activate the Decoding Checkbox (and optionally "Write Decoded Bits")
1. The decoding result get printed to the GNU Radio console

### Flow Graph
![](doc/enocean_erp1.svg)

### GUI
![](doc/gui_screenshot.png)

## Notes
- The "recording" folder needs to exist, otherwise it wont't run.
- It will always create two "dummy" recording files. I didn't find a way to inhibit the creation of files dynamically at runtime.

### Resources
- ERP1 Protocol Spec: https://www.enocean.com/wp-content/uploads/support/download/EnOceanRadioProtocol1.pdf
- EEP (data packet) definitions: https://tools.enocean-alliance.org/EEPViewer/#1
- STM300 transceiver module documentation: https://www.enocean.com/wp-content/uploads/downloads-produkte/en/products/enocean_modules/stm-300/user-manual-pdf/STM300x_UserManual_Dec2021.pdf
- Thermokon SR04 (sensor) datasheet: https://www.thermokon.de/direct/alterra-base/mimes/get/e76366f5494cb1001

### Example decoded data
Here is some example data from sensors using the `A5-02-05` EEP. The byte 03 contains the actual temperature value, while bytes 05-08 contain the transmitter ID.  
For more detailed information, look into the ERP1 spec / EEP definitions.

```
00 01 02 03 04 05 06 07 08 09 0A
--------------------------------
A5 00 00 56 08 05 12 43 70 01 CE
A5 00 00 56 08 05 12 43 70 00 CD
A5 00 00 64 08 05 09 70 3D 01 CD
A5 00 00 54 08 05 10 D8 8C 01 7B
A5 00 00 5F 08 05 03 07 9E 01 BA
A5 00 00 55 08 05 14 46 E7 00 48
A5 00 00 55 08 05 14 46 E7 01 49
A5 00 00 5B 08 05 09 A2 CC 01 85
```
(Abbreviated. Every sensor sends the data 3 times.)