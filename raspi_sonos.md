# Raspi Sonos Stuff

*Note that in CT this is the raspi with the black ethernet cable*

These scripts currently run on a local raspberry pi. They are all python 2.7 scripts.

1. `flask_ask_sonos.py`
    1. This script translates spoken alexa commands ("Ask Sonos" ...) and `alexa_cli.py` command line requests into sonos actions
    2. Uses the flask extension `flask-ask`, which was written specifically for python-based Alexa apps.
    3. Uses zmq to communicate with `flask_ask_zmq.py`, which is the script that directly interacts with Sonos
    4. Also requires ngrok http 5000 to be running on the raspberry pi and the url will look like 1234.ngrok.io/sonos
    5. There is a lambda python program that just directs the alexa interaction to the right raspberry pi based on the location
2. `flask_ask_zmq.py`
    1. Script receives task dictionaries via json from `flask_ask_sonos.py`.  
    2. Also carries out the tasks created by `alexa_cli.py`
3. `sonos_track_info.py`
    1. Broadcasts track information via mqtt
4. `./ngrok start flask ssh`
5. `esp_check_mqtt.py`
    1. This script subscribes to mqtt messages issued from esp8266 running the script `sonos_remote.py` using the topic `sonos/{location}` and sent to an mqtt broker running on ec2 instance 
    2.  Supports commands like changing the volume and pausing the music

Notes: mqtt should have been installed by sudo apt-get ...
