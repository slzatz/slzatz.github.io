# Overview and instructions for EC2 Amazon AWS cloud-based server

*Note that there is a `who_am_i.txt` file*

- `test_scheduler.py` is a `flask`-based app that also depends on `apscheduler` and has a few roles with listmanager including email (using sendgrid) and twitter alarms, and incoming emails to cloudmailin that are turned into http posts and allow the user to receive an email alarm and reply to it and modify the entry which is pretty cool (although not really used at the moment)
- solr 6.1.0 indexes both my **Amazon Music** database for Sonos and my **listmanager** database (which is nice but right now isn't being used for much)
- The `postgresql`-based listmanager_p database is the database that the local sqlite versions of listmanager synch with 
- mosquitto server that  esp_tft_mqtt.py posts messages to that are picked up by `display_info.py`
- `esp_tft_mqtt` (python 3.x)
- `esp_tft_mqtt_sf2` (python 3.x)
- `esp_tft_mqtt_outlook.py` (python 2.7)
- `esp_tft_mqtt_photos.py` (python 2.7) - displays photos and lyrics of tracks that sonos is playing.  Relies on `sonos_track_info.py` running on a local raspi to post arist and track via which is also running on the EC2

When you restart, you need to:

1. run `screen`
2. create a `test_scheduler` tab and `cd mylistmanager3` and `python3 test_scheduler.py`
3. create tab and `cd ~/solr-6.1.0/bin` and `./solr start` and `sudo /etc/init.d/mosquitto start`
4. create a `esp_tft_mqtt tab` and `cd sonos-companion` and `python3 esp_tft_mqtt.py`
5. create a `_sf tab` and `cd sonos-companion` and `python3 esp_tft_mqtt_sf2.py`
6. create `a_photos` tab and `cd sonos-companion` and `python esp_tft_mqtt_photos.py`
7. create `a _outlook` tab and `cd sonos-companion` and `python esp_tft_mqtt_outlook.py`

I automated screen in the following way:

`# file infoscreenrc`
`# invoke with: screen -c infoscreenrc`

    shell -${SHELL}
    caption always "%?%{ Wk}%-Lw%?%{Rk}%n*%f %t%?(%u)%?%?%{Wk}%+Lw%? %{Rk}%=%c %{rk}%d/%M/%Y"
    defscrollback 1024
    startup_message off
    hardstatus on
    hardstatus alwayslastline

    chdir mylistmanager3
    screen -t test_scheduler
    stuff "python3 test_scheduler.py"
    chdir ../sonos-companion
    screen -t esp_tft_mqtt
    stuff "python3 esp_tft_mqtt.py"
    screen -t _sf
    stuff "python3 esp_tft_mqtt_sf2.py"
    screen -t _photos
    stuff "python esp_tft_mqtt_photos.py"
    screen -t _outlook
    stuff "python esp_tft_mqtt_outlook.py"
    chdir ../solr-6.6.0/bin
    screen -t bash
    stuff "./solr start && echo you need to sudo /etc/init.d/mosquitto start"

*Note that postgresql is being used for images (artist_images.db); postgresql restarts automatically*

If the solr database stops working and you see:

    cannot open '/home/slzatz/solr-6.1.0/server/logs/solr.log' for reading: No such file or directory 
    
it likely means that you've somehow run out of memory and need to reboot the EC2 cloud-based server 

Also note that to upgrade solr it seems to involve just wget'ing the latest version, unzipping it and then moving the `sonos-companion` directory and the `listmanager` directory (which I did not do initially) located in `solr-6.xxx/server/solr` to the same place in the new solr version's directory structure.  I did not modify `solr.xml` in the same directory but some chance that I should have and will check.
    
