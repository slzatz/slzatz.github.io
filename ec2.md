# Overview and instructions for EC2 Amazon AWS cloud-based server

*Note that there is a `who_am_i.txt` file*

- `test_scheduler.py` is a `flask`-based app that also depends on `apscheduler` and has a few roles with listmanager including email (using sendgrid) and twitter alarms, and incoming emails to cloudmailin that are turned into http posts and allow the user to receive an email alarm and reply to it and modify the entry which is pretty cool (although not really used at the moment)
- solr 6.1.0 indexes both my **Amazon Music** database for Sonos and my **listmanager** database (which is nice but right now isn't being used for much)
- The `postgresql`-based listmanager_p database is the database that the local sqlite versions of listmanager synch with 
- mosquitto server that  esp_tft_mqtt.py posts messages to that are picked up by `display_info.py`
- `esp_tft_mqtt` (python3)
- `esp_tft_mqtt_sf2` (python3)
- `esp_tft_mqtt_outlook.py` (python2.7)

When you restart, you need to:

1. run `screen`
2. create a `test_scheduler` tab and `cd mylistmanager3` and `python3 test_scheduler.py`
3. create tab and `cd ~/solr-6.1.0/bin` and `./solr start` and `sudo /etc/init.d/mosquitto start`
4. create a `esp_tft_mqtt tab` and `cd sonos-companion` and `python3 esp_tft_mqtt.py`
5. create a `_sf tab` and `cd sonos-companion` and `python3 esp_tft_mqtt_sf2.py`
6. create `a_photos` tab and `python esp_tft_mqtt_photos.py`
7. create `a _outlook` tab and `cd sonos-companion` and `python esp_tft_mqtti_outlook.py`

*Note that EC2 postgresql being used for images and listmanager restarts automatically*

If the solr database stops working and you see:
    cannot open '/home/slzatz/solr-6.1.0/server/logs/solr.log' for reading: No such file or directory 
it likely means that you've somehow run out of memory and need to reboot the EC2 cloud-based server    
    
