# Overview and instructions for EC2 box

Note that there is a who_am_i.txt file

1. Running test_scheduler.py which is a flask-based app that also depends on apscheduler and  which has a f    ew roles with listmanager including email (using sendgrid) and twitter alarms, and incoming emails to cloud    mailin that are turned into http posts and allow the user to receive an email alarm and reply to it and mod    ify the entry which is pretty cool
2. solr 6.1.0 which contains both the Amazon Music Database for Sonos and the solr listmanager data    base (which is nice but right now isn't being used for much)
3. postgresql listmanager_p database that is the server version of listmanager and is the datab    ase that the local sqlite versions of listmanager synch with
4. mosquitto server that  esp_tft_mqtt.py posts messages to that are picked up by display_info.py
5. esp_tft_mqtt(python3)
6. esp_tft_mqtt_sf (python3)
7. esp_tft_mqtt_outlook.py (python2.7)

When you restart, you need to:

1. run screen
2. create a test_scheduler tab and do cd mylistmanager3 and then python3 test_scheduler.py
3. create another tab and cd to ~/solr-6.1.0/bin and do ./solr start and then do sudo /etc/init.d/mosquitto start
5. postgresql being used for images and listmanager restarts automatically
6. create a esp_tft_mqtt tab and cd sonos-companion and python3 esp_tft_mqtt.py
7. create a _sf tab and cd sonos-companion and python3 esp_tft_mqtt_sf2.py
9. create a_photos tab and python esp_tft_mqtt_photos.py
10. create a _outlook tab and cd sonos-companion and python esp_tft_mqtti_outlook.py
~
~
~
~
"who_am_i.txt" 18L, 1590C                                                                    1,1           All
0$ bash  1$ solr/mosquitto  2$ esp_tft_mqtt  3-$ _sf  4*$ bash                                20:26 27/May/2017

