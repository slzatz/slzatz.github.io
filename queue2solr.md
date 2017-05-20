# queue2solr.py

This script is used to move tracks from the Sonos queue into the solr database that is running on EC2 instance.
The solr database is determined by the `solr_uri` in `config.py` - right now *ec2*
This works without having to play the tracks
The script asks whether this is an album to know whether to assign track numbers
from the queue positions or just default to track 1
Note that if the solr document already exists, the add operation will replace it. 
However, if you change the id at all by changing the album name or title it won't delete
the older versions.

The upload format for SolrClient (python3 solr client program) is jsonifying a list of dictionaries:

    [{'id':'After_The_Gold_Rush_Birds', 'artist':'Neil Young', 'title':'Birds', 'album':'After the Gold Rush',
    'uri':'x-sonos-http:amz%3atr%3a44ce93d2-4105-416a-a905-51fe0f38ed9a.mp4?sid=26&flags=8224&sn=2'...}{...
