## RP2040 Debug Tester
### Licence and design:
-	RP2040 Debug Tester adapted by Elliot Alfirevich http://ell-tech.com/ 
-	Based on Adafruit RP2040 Tester Brains https://github.com/adafruit/RP2040-Based-Tester-Brains-PCB by Adafruit (ladyada & evaherrada) 
-	Licenced under Creative Commons Attribution-ShareAlike 3.0 Unported
-	4-layer impedance controlled circuit board, JLC04161H-7628 to ensure appropriate trace impedance for USB.

### Added features:
1.	USB-Serial converter CP2102N added to snoop serial UART bus, and allow direct serial programming of devices on test using the changeover DPDT switches.
2.	USB Hub with device power reset buttons and USB device current measurement at JP7 using Nordic PPK2 or similar. 
3.	Extra power supply functions added in the FPC 18-pin and external device 0.1in header as JP6.
4.	Additionally JP5 provides connection point for a Nordic PPK2 or similar for measuring load current of external prog-cable devices. “TEST” pin provides opportunity for custom voltages/measured currents. 
5.	Nordic PPK2 can be permanently mounted via the mounting holes and standoffs.

### Notes/changes to Adafruit design:
1.	Dedicated 3.3V regulator provided, in lieu of using the in-built RP2040 buck-boost converter. 
2.	Diode included on 3.3V supply to RP2040 VSYS to prevent back-feeding of ~5V onto the 3.3V bus from plugging in the RP2040 USB port.
3.	USB-C PD controller STUSB4500LQTR ensures enough VBUS current is provided by the host. Previously used 5.1k CC resistor method – but strictly compliant USB hosts would limit supply to 500mA which was not enough.
4.	RTS/DTR logic transistors added for ESP boot/reset to be generated from the CP2102N.
5.	High side switch (Option 1) or separate regulator (Option 2) required to power CP2102N because the CP2102N defaults to pull-up on USB data lines, causing the USB hub to disable the port at start-up (doh!)
6.	USB Hub USB2514B added to enable one cable to the host computer. 
   1.	Port 1 – CP2102N USB-Serial converter
   2.	Port 2 – USB port inc. 1.5A charging
   3.	Port 3 – USB port inc. 500mA power measurement via jumper JP7
   4.	Port 4 – RP2040 via test points.
7.	USB Hub controller strapped to report Port 1 as “non-removable” and enable USB charging support on Port 2.
8.	USB Hub controller requires an external crystal and 1M resistor. Check as newer QFN packages don’t require this resistor.
9.	SD card changed due to part availability. Note new part changes card detect from NO to NC which requires change in Brains library code.
10.	TFT connection not tested. Directly copied from original design.

### Original Adafruit Design
Forked from https://github.com/adafruit/RP2040-Based-Tester-Brains-PCB

This board is what we use internally at Adafruit to program and test boards with microcontrollers on them. 
   * An [RP2040 Pico](https://www.adafruit.com/product/5525) (or perhaps PicoW?) is used to run the test software
   * [Use a 16x2 LCD (ideally RGB backlight)](https://www.adafruit.com/product/398) for display output with color feedback such as green for pass, red for fail. RGB backlight is WS2811-controlled. Don't forget to set the contrast pot on first usage.
   * Serial1 hardware UART has RX/TX pin switch to flip directions
   * 1.9" TFT connector is placed but hasn't been tested or used yet
   * Piezo buzzer can be used for audible feedback alerts
   * STEMMA QT I2C port
   * Big reset button
   * Firmware files can be stored on MicroSD card which is slotted in. 
   * Connect the right hand side, either the 0.1" header or 18-pin EYESPI-like FPC connector to the device-under-test jig which can be different for each product. 
   * [USB Host testing is done with bitbang PIO support in TinyUSB](https://github.com/adafruit/Adafruit_TinyUSB_Arduino), USB 5V power can be turned on / off 
   * Use Philhower RP2040 core for programming
   * The RP2040 brains can perform ESP32 programming, RP2040-via-USB, AVR ICSP/UPDI, or SWD DAP. [See the TestBed library for some examples.](https://github.com/adafruit/Adafruit_TestBed)

### License

Originally designed by Limor Fried/Ladyada for Adafruit Industries. Adafruit invests time and resources providing this open source design, please support Adafruit and open-source hardware by purchasing products from [Adafruit](https://www.adafruit.com)!

Creative Commons Attribution/Share-Alike, all text above must be included in any redistribution. 
See license.txt for additional details.
