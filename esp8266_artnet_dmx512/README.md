# Overview

This sketch implements a WiFi module that uses the Art-Net protocol over a wireless connection to control wired stage lighting fixtures/lamps that are DMX512 compatible. It listens to incoming Art-Net packets and forwards a single universe to DMX512. It features over-the-air (OTA) configuration of the wifi network that it connects to, configuration of the universe that is forwarded, and monitoring of the incoming packets and the frame rate.

Upon switching on, the LED turns yellow to indicate that setup is done. After that the LED turns red to indicate that it is not connected to WiFi. It will try connect as client to the previously used WiFi network; if that succeeds, the LED turns green and setup is ready. If that fails, the LED remains red and the node creates a wireless access point (AP) with the name Artnet. You can connect with laptop or smartphone to that network to configure the WIFi client settings and provide the password of the network to which it should connect. After that it resets.

Wherever there is activity on the web interface (configuration, monitoring), the LED turns blue. During web interface activity, the DMX512 output is silenced. A smooth web interface and smooth DMX signalling don't go together.

See http://robertoostenveld.nl/art-net-to-dmx512-with-esp8266/ for more details and photos, and https://robertoostenveld.nl/timing-and-jitter-in-dmx512-signals/ for a detailled look at the timing of the DMX signals.

# Components
  - Wemos D1 mini
  - MAX485 module, e.g. http://ebay.to/2iuKQlr
  - common cathode RGB LED
  - 2x 220 Ohm resistors
  - 1x 100 Ohm resistor
  - 3 or 5 pin female XLR connector or cable
  - USB A plug for power input

# Wiring scheme
 - connect 5V and GND from the USB A plug to the Vcc and GND of the MAX485 module
 - connect 5V and GND from the Vcc and GND of the MAX485 module to the 5V and GND of the Wemos D1 mini
 - connect pin A of the MAX485 module to XLR 3
 - connect pin B of the MAX485 module to XLR 2
 - connect GND                        to XLR 1
 - connect   TX1/D4 of the Wemos D1 mini to the DI (data in) pin of the MAX485 module
 - connect GPIO0/D3 of the Wemos D1 mini to the DE (data enable) of the MAX485 module
 - connect GPIO4/D2 of the Wemos D1 mini to the RE (receive enable) of the MAX485 module
 - connect GPIO5/D1 of the Wemos D1 mini to the Ro (receive output) of the MAX485 module
 - connect the blue    leg of the LED over the 100 Ohm resistor to GPIO16/D0
 - connect the green   leg of the LED over the 220 Ohm resistor to GPIO14/D5
 - connect the red     leg of the LED over the 220 Ohm resistor to GPIO12/D6
 - connect the cathode leg of the LEDto GND

# XLR conections:
<img src="https://mediarealm.com.au/wp-content/uploads/2013/04/XLR-Wiring-Standard-3-pin.jpg"><br>
<img src="https://mediarealm.com.au/wp-content/uploads/2013/04/XLR-Wiring-Standard-5-pin.jpg">

# LED conections
<img src="http://www.nkcelectronics.com/assets/images/rgb_5mm_cathode.jpg">

# SPIFFS for static files

Please note that you should not only write the firmware to the ESP8266 module, but also the static content for the web interface. The html, css and javascript files located in the data directory should be written to the SPIFS filesystem on the ESP8266. See for example http://esp8266.github.io/Arduino/versions/2.0.0/doc/filesystem.html and https://www.instructables.com/id/Using-ESP8266-SPIFFS for instructions.
You will get a "file not found" error if the firmware cannot access the data files.

