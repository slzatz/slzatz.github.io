## alexa_cli.py

This script can be run from anywhere.  It gets to the python scripts that talk to Sonos network via ngrok URLs.  So, for example,
you can see what is playing at any given moment in either NYC or CT by just typing "what"

Below are the keywords that can be entered at the command line that are mapped to Amazon Intents 
in flask_ask_sonos.py, which in turn calls flask_ask_zmq.py via zmq.

- louder,quieter->TurnTheVolume
- album->PlayAlbum 
- pause->AMAZON.PauseIntent 
- resume->AMAZON.ResumeIntent
- next->AMAZON.NextIntent
- shuffle->Shuffle
- radio->PlayStation
- what->WhatIsPlaying
- list or show->ListQueue
- clear->ClearQueue
- mix {a} and {b}->Mix
- add {a}->AddTrack
- recent->RecentTracks

everything else assumed to be the title of a track -> PlayTrack
