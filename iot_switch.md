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
- The IoT generated event also includes the battery state and the serial number and I added a text message
- The Lambda function posts a json formatted string to the MQTT broker running on EC2
- The`iot_switch` script interprets a single click as *on* and double-click as *off* and sets digital pin 15 of the ESP8266 either high or low based on the click type
- The information displayed on the Feather Wing OLED is the text message, the click type and the battery state
- At the moment I am using Lambda environment variables to set the topic and for the optional message
- The current application for all this is to turn on the electric kettle from upstairs when we wake up so the water is hot by the time we come downstairs (yes, that will no doubt contribute to world peace)

The AWS lambda function is very simple:

    import paho.mqtt.publish as mqtt_publish
    import json
    import os
    from config import aws_mqtt_uri
    
    topic = os.environ['topic']
    
    try:
        msg = os.environ['message']
    except Exception:
        msg = ''
        
    def lambda_handler(event, context):
        event.update({'message':msg'})
        mqtt_publish.single(topic, json.dumps(event), hostname=aws_mqtt_uri)
        
        
The Python script running on the esp8266:  [iot_switch.py](https://github.com/slzatz/esp8266/blob/master/iot_switch.py)
