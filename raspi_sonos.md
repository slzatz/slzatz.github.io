# Raspi Sonos Stuff

1. ct black ethernet cable
2. flask_ask_sonos.py - interacts with alexa and alexa_cil
3. flask_ask_zmq.py - flask_ask_sonos and alexa_cli depend on flask_ask_zmq to interact with sonos.
4. esp_check_mqtt.py - now running on AWS
5. sonos_track_info.py replaces sonos_scrobble3
6. ./ngrok start flask ssh
7. Below as of March 26, 2017
8. Forwarding https://3e343355.ngrok.io -> localhost:5000
9. Forwarding tcp://0.tcp.ngrok.io:15557 -> localhost:22

Notes: mqtt should have been installed by sudo apt-get ...
