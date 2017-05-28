# Raspi Sonos Stuff

*Note that in CT this is the raspi with the black ethernet cable*

The scripts that are needed include:

1. `flask_ask_sonos.py` - interacts with spoken alexa commands ("Ask Sonos" ...) and `alexa_cli.py`
2. `flask_ask_zmq.py` - `flask_ask_sonos.py` and `alexa_cli.py` depend on this script to interact with sonos.
3. `sonos_track_info.py` broadcasts track information
4. `./ngrok start flask ssh`

Notes: mqtt should have been installed by sudo apt-get ...
