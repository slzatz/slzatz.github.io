# Info Display

The following scripts are important:

- `display_info_photos.py`: python 2.7 (right now).  Runs on a raspi.  Dependent on pygame.

The following run right now on an EC2 instance:
- `esp_tft_mqtt.py`: handles som of the common stuff like stock prices, listmanager stuff, news feeds, industry stuff
- `esp_tft_mqtt_photos.py`: lyrics and photos when music is running.  Gets artist and track from `sonos_track_info.py`
- `esp_tft_mqtt_sf2.py`: Shows total salesforce forecast plus top opportunities and top closed
- `esp_tft_mqtt_outlook.py`: shows my outlook schedule

