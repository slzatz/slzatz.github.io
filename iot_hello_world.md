The equivalent if "Hello World!" in IoT might be making an LED turn on and off using some device be it a button, a phone app or something else.

So to create the IoT "Hello World!" I used an Adafruit ESP32 Feather and the OLED Featherwing.

The script running on the ESP32 is a simple mqtt script.

I am using the @loboris port of MicroPython for the ESP32.

The script that runs on the ESP32 is: 

[iot_generic_esp32.py](https://github.com/slzatz/esp8266/blob/35f7870e3fc59c022074014b50480e0a1f887c94/iot_generic_esp32.py)

It is necessary to put pull-ups on the ESP32 IC2 pins since the Adafruit ESP32 Feather doesn't have them/

The Adafruit drive supports the hardware I2C although I think that has been corrected by more recent @loboris builds (although that is not tested).

The Adafruit driver is at:

[Adafruit SSD1306 driver](https://github.com/adafruit/micropython-adafruit-ssd1306)

I renamed that Adafruit SSD1306 driver to `ssd1306_i2c.py` and used ampy to put in on the board. 

I used pullups (10k) on BAT because it was eaier to reach to SCL and SDA although probably should actually be connected to 3V3. 


