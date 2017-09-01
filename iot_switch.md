This project used:
- Digital Loggers IoT Relay (AC/DC Control Relay) version 2 with one always on outlet
- Adafruit Feather HUZZAH ESP8266
- Adafruit OLED Feather Wing (SSD1306)
- AWS IoT Button
- MQTT broker running on an EC2 instance (t2.small)
- Python script running on Feather is `iot_switch.py`
- AWS Lambda function is `switch.py` which uses `paho-mqtt`

A few other points:
- MQTT topic is *switch*
- The Lambda event that is triggered by AWS IoT button includes the clickType, which can be SINGLE, DOUBLE or LONG
- The
- The`iot_switch` script interprets a single click as *on* and double-click as *off*
- The Lambda function sends some additional info besides the type of click to the Feather to be displayed on the Feather Wing OLED:
  - A 
