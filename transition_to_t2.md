Steps to create a new EC2 instance (t2.small) so that my original t1.micro instance can be retired.  My understanding is that since t1 uses a paravirtualization volume and t2 supports only HVM that there isn't a simple migration path but even if there was, this was an opportunity to clean up some of the stuff that had accumulated over the years.

**Create a new public/private key pair**
Logging on to an EC2 instance requires a public/private key handshake.  

1. Created a new key/pair through the AWS Web interface
2. Downloaded the private key from and used `puttygen` to create the `.ppk` file that `putty` requires
3. Created a new `putty` entry named *AWS2*

**Initial logon as ubuntu user**

Logon as the default ubuntu user and then add user *username*:

    sudo adduser username
    ...

Add *username* to sudoers

    usermod -aG sudo username

To make it possible to log directly into the t2 AWS instance as *username* via putty (SSH), you need to take the public key sitting in the ubuntu user in `.ssh/authorized_keys` and create `username\.ssh` and copy the file there.  Need to use sudo to copy and then change the ownership to *username*.

    sudo chown username authorized_keys

Also changed the `username/.ssh` directory permissions to match the way the ubuntu user is configured:

    chmod 700 .ssh

Then tested that I could log in with putty as *username* and that worked.

**Install some required programs/services**

    sudo apt-get install unzip (to unzip solr and to zip directories to move them [e.g. solr sonos_companion])
    sudo apt-get install default-jre (needed by solr)

Install and configure postgresql:

    sudo apt-get install postgresql (note this installed version 9.5)

Need to make some changes in postgresql config files so that *username* can access from any address and tell postgresql to listen to all addresses:

    sudo -u postgres -i

    vim /etc/postgresql/9.5/main/pg_hba.conf
  
      #TYPE  DATABASE     USER     ADDRESS     METHOD (password method)
       host           all            username     0.0.0.0/0      md5 

    vim /etc/postgresql/9.5/main/postgresql.conf
    listen_addresses = '0.0.0.0'

Create a *username* user that postgresql recognizes:

    sudo -u postgres -i
    psql
    postgres=# create user username createuser password '*******';     (yes include quotes)
    CREATE ROLE

Then switched user to to *username* and did:

    sudo service postgresql stop
    sudo service postgresql start

And now can do everything as *username*.

Then used PG4 Admin on local Windows machine to create a new server called t2.small.

Then move databases from t1.micro to t2.small by downloading the pg_dump file to the local Windows machine and then upload it to the new t2 instance using WinsSCP:

    pg_dump -U username artist_images > artist-images.pgsql
    [use WINSCP to move to the new server]
    use psql username=# CREATE DATABASE artist_images;
    psql -U username artist_images < artist_images.pgsql

Then did the same for `listmanager_p`.

Move solr:

    wget http://www.gtlib.gatech.edu/pub/apache/lucene/solr/6.6.0/solr-6.6.0.zip
    unzip solr-X...

To move the `sonos-companion` directory and the `listmanager` directory (which I did not do initially) located in `solr-6.xxx/server/solr` to the same place on the new server:

    zip -r sonos sonos_companion
    Use winscp to move sonos.zip to local machine and then to new machine.
    unzip sonos.zip

**Install more software**

sudo apt-get install python3-pip
sudo apt-get install llibffi-dev, libssl-dev (for cryptography which needed to be upgraded but had already been installed)

git clone mylistmanager3 and sonos-companion

**Steps for test_scheduler** 

  1. `sudo -H pip3 install Flask --upgrade`
  2. `sudo -H pip3 install apscheduler --upgrade`
  3. `sudo -H pips install sendgrid --upgrade`
  4. `sudo -H pip3 install sqlalchemy --upgrade`
  5.  `git clone https://github.com/sixohsix/twitter.git`
  6.  `sudo -H pip3 install --upgrade psycopg2`
  7.  `sudo -H pip3 install -- upgrade paho-mqtt`

Need to go to cloudmailin.com and change ip address to new server.

**Steps to get esp_tft_mqtt, sf2, outlook and photos running (broadcasts info boxes that are displayed locally)** 

[Note that some of these are running under python2.7 and some under python3]

    sudo -H pip3 install --upgrade schedule
    sudo -H pip3 install --upgrade tabulate
    sudo apt-get install python3-pandas (for esp_tft_mqtt_sf2.py)
    sudo apt-get install python-pip (already have pip3 but need pip2)
    sudo -H pip2 install exchangelib (python 2.7 version  - for outlook)
    sudo -H pip2 install paho-mqtt (python 2.7 version - for _outlook)
    sudo -H pip2 install paho-mqtt (python 2.7 version - for _outlook)
    sudo -H pip2 install sqlalchemy (python 2.7 version - for _photos)
    sudo -H pip2 install psycopg2 (python 2.7 version - for _photos)
    sudo -H pip2 install --upgrade google-api-python-client (python 2.7 version for _photos)
    sudo -H pip2 install --upgrade cssselect (python 2.7 version for _photos)
    
*Note for sf2 need to enable the new IP to access SF.*

also ...

- need to change raspi pi `display_info_photos.py` `config.py` to new server IP
- need to change raspi pi `sonos_track_info.py` `config.py` to new server IP
- starting/stopping solr: `.../solrxxxx/bin/solr start|stop`
- starting/stopping mosquitto: `sudo /etc/init.d/mosquitto start|stop`

