# Alexa-Assistant-HomeAutomation
#smart gujarat hackathon project
We made solution of this problem that is software based platform or service be created by the means of which we can control home appliances by giving voice commands. We  integrated both of the popular platforms called Amazon Alexa and Google Home. Special attention  in our solution was to integrate with our existing system for which we can build a raspberry pi  to cloud servers. That  listen to alexa/google home commands coming from the cloud/internet and forward it to our home automation devices via HTTP requests ( GET method) or socket connections and also get data from our home automation devices and push it to the cloud back.


Hardware Requirements:
1.	Raspberry Pi 3b + (with 16 GB sd-card)
2.	External Speaker with 3.5mm AUX cable
3.	Sound Card 
4.	Relay module
5.	LED/ AC Bulb

          ---------------> Solution  <---------------

Step 1:  Install Raspbian on  Raspberry Pi
Link:  https://thepi.io/how-to-install-raspbian-on-the-raspberry-pi/

Step 2:  Create Amazon Developer Account and Configure Alexa Voice Services on Raspberry Pi

Step 3: For installation of Alexa Voice Service in Raspberry Pi
		1.Register on Amazon Developer
		2.Device Registration
    •	First select alexa voice service
    •	Then Click on Product & give some product name, ID. 
    •	select “Device with Alexa built-in
    •	The product Category will be Wireless speaker & select other options as shown in the pictures.
    •	Now  set Security profile. Select Create new profile & give some profile name & description.
    •	Now click Others devices and give a client name and click generate ID.
    •	Download button to download the JSON File. This file is very important!
		3. Setting up Speaker & Mic for Alexa on Raspberry Pi 3B+
    •	nano /home/pi/.asoundrc
    •	After editor opens, copy the following text and paste there. Do not forget to replace the card no and device no with your card and device.
    
    pcm.!default { 
    type asym 
    capture.pcm "mic" 
    playback.pcm "speaker"
    }
    
    pcm.mic {
    type plug 
    slave { 
    pcm "hw:1,0" 
    }
    }
    pcm.speaker 
    {
    type plug 
    slave { 
    pcm "hw:0,0"
    }
    }
    
    •	Now for audio configuration run the code: alsamixer
		4. For Alexa SDK installation
    •	To install Amazon Alexa on Raspberry Pi 3B+, first go to File Manager and create a Folder named “alexa” inside your home directory which typically “/home/pi/”.
    •	Now copy the previous downloaded JSON file inside alexa folder.
    •	Now open that folder and press “F4” to open terminal on present directory.
    •	To install AVS SDK run the following commands one by one.
    1.	sudo apt-get update
    2.	sudo apt-get upgrade
    3.	wget https://raw.githubusercontent.com/alexa/avs-device-sdk/master/tools/Install/setup.sh
    4.	wget https://raw.githubusercontent.com/alexa/avs-device-sdk/master/tools/Install/genConfig.sh
    5.	wget https://raw.githubusercontent.com/alexa/avs-device-sdk/master/tools/Install/pi.sh
    6 .	sudo bash setup.sh config.json [-s 1234]
    	Now you will be asked to agree the terms and condition inside terminal. Just Type “AGREE” there.
    •	Now run command :sudo bash startsample.sh and authorize alexa.
    •	Now made alexa.sh file on desktop and write following content
      	#!/bin/bash
    	cd "/home/pi/alexa/build/SampleApp/src"
    	./SampleApp "/home/pi/alexa/build/Integration/AlexaClientSDKConfig.json" "/home/pi/alexa/third-party/alexa-rpi/models" DEBUG9
    	Alexa is installed in a pi
Step4: For installation of google assistant in raspberry pi.

1.	Create project on google action console.

2.	After creating the project, above page will open. Then scroll down and select device registration.

3.	After registering in next window download OAuth credentials. Keep this in home directory & do not rename the file.

4.	Enable google assistant api from clicking on enable api in google api console.

5.	To install google assistant run the following commands one by one.
•	sudo apt-get install python3-dev python3-venv
•	python3 -m venv env
•	env/bin/python -m pip install --upgrade pip setuptools wheel
•	source env/bin/activate
•	sudo apt-get install portaudio19-dev libffi-dev libssl-dev libmpg123-dev
•	python -m pip install --upgrade google-assistant-library
•	python -m pip install --upgrade google-assistant-sdk[samples]
•	python -m pip install --upgrade google-auth-oauthlib[tool]
•	For authorization of google assistant run following command
•	google-oauthlib-tool --scope https://www.googleapis.com/auth/assistant-sdk-prototype \
•	      --scope https://www.googleapis.com/auth/gcm \
•	      --save --headless --client-secrets client_secret_XXXXXX.json

Step5:create a home automation system
•	Register an account with PubNub
•	Register an account with IFTTT
•	Installing PubNub Python SDK on raspberry pi:
	Sudo pip install pubnub==3.9.0
•	then, go to PubNub and sign in with your credentials.
•	Setting up ifttt for alexa/google assistant
	Login to IFTTT with your credentials or Sign Up if you don’t have an account on it.
	Now goto On My Applets, Click on New Applet
	Click on +this
	Search Amazon Alexa/google assistant and click on it, sign in with your amazon developer account details.
	Choose the trigger, Say a specific phrase
	Provide “turn on the light” as the phrase, click on Create Trigger.
	Click on +that
	Then Search for Webhooks, click on it and Select Make a Web Request
	Webhooks provides the REST WEB Request, Use the Publish and Subscribe keys obtained from the PubNub and modify the following URL.
	http://pubsub.pubnub.com//publish/pub_key/sub_key/0/alexaTrigger/0/{"requester":"Alexa","trigger":"light","status":1}
	Paste the modified URL to the URL text box.
	Set the Method as GET, Content as application/json and click on save.
	Follow the same steps to create the trigger for “turn off the light” and modify the URL to the following. 
	http://pubsub.pubnub.com//publish/pub_key/sub_key/0/alexaTrigger/0/{"requester":"Alexa","trigger":"light","status":0}
	Now raspberry pi is ready to use home automation.

    
    
    


