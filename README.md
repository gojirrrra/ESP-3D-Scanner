# ESP-3D-Scanner
### Proof of concept for an array of ESP32's used as a photogrammetry 3D-Scanner

This project is a riff on https://github.com/ldab/ESP32-CAM-Picture-Sharing
Thank you, ldab! (Is there a way to poke/mark users on github?) All prais belongs to you. Your licens should be included here. 

So far it works with 10 Units - hardwawre is described for 10 - 20 more are ordered. 

Little disclaimer: I'm a total noob. But I fugured this is actually so easy, total beginners might give it a shot, so it might be a little over explained for some.

The code was slighly changed so:

- Units don't sleep
- exable / dim flash led
- Image quality max



## How to:

### Hardware: 

ESP-32s with OV2640 Cam (AI Thinker) 

FTDI-Programmer 

Power supply: 5V,  20A <- seems overkill but keeps room for ~20 units and overhead
![Here 5V, 40A](


20+ dupont cables female to female preferably in red and black

!!! Be aware! If you use wrong cables you might burn down your house! 5v seems harmless but the amps arent! Go and ask an adult like me ;)!!!

Cables: Take care which cables you use from mains to psu and psu to connector. All elements like switches have to be rated for the amps you use

Connector: You need some high amp connector that doesnt go poof. I used a XT60.

Test code with one. If it works, this is wiring for 10 Units:

Wall outlet ---220V, 10A--- PSU (connector line one/ fist V-, first V+) --- 5V, 10A --- Connector --- (solder 10 dupont cables to each ground and power of the connector.) single dupont connecton --- ESP


### Software:

As the original project this was done in PlatformIO for VisualStudio Code. Check out ldab's git for instructions. You might have to update pathon to its latest version if you get stuck. 
(Its rather buggy, but has its benefits (intellisense)).

FTP Server: 
I set up an old macbook that was collecting dust. The installed OS (el Capitan) still has native support for FTP and was the quickest solution. 
Windows users might want to try fileZilla server or other free ftp server software
Linux... Well you're on linux, you'll figure something out, I guess...

## Instructions:

### Blynk:

You can clone the setup via this qr code 
<img scr="https://github.com/Schaggo/ESP-3D-Scanner/blob/master/ESP%203D-Scanner/pics/3d_SCANNER%20BLYNK.PNG" width="400">
![BLYNK CLONE](https://github.com/Schaggo/ESP-3D-Scanner/blob/master/ESP%203D-Scanner/pics/3d_SCANNER%20BLYNK.PNG)
- Download the BLYNK app to your smartphone
- Open a new project and add a Real Time Clock, a button and a slider.
- in your project settings setup 10 devices (All ESP32 Dev Board, connection type WiFi)
- add all devices to a tag // if you later add devices add them to the tag
- email yourself all auth tokens for the 10 decives

- Change the RTC to your locations time zone
- Change the Button output to V5 with mode push with target device = tag
- Change slider to V10 with range from 0 to 4 // this might change in later versions

### Code:

- Set char auth[] for each token created in BLYNK
- Fill in your FTP Server and WiFi credentials
- Set the Filename (line 77) according to the folder structure of your ftp.

 It makes sense to name the file according to your unit number. Some modules might have connection issues or a smudge on the lens...
 
- Change filename and auth when uploading code according to unit

You're done.
Upload the code to the ESP, start FTP Server and Blynk, hit the button in the app. You should see a file at your set location with a timestamp in its name. Wait till the file size stops increasing. Check the picture for artifacts and errors.

## TODO: 

- Energy optimisation. The ESP's are a little greedy right now.

- Prepare the project for higher unit count:
  Trigger camera capture with physical button - then log 10 units into wifi and upload - wait - the next 10
  
- Find a better solution to dim the onboard flash LED on pin4. It works but it is not really practical. The lowest level is still way too bright.



Special thanks to u/lost_electron! I think he saved my life. See "!!!Be aware..."

