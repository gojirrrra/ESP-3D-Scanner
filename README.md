# ESP-3D-Scanner
Proof of concept for an array of ESP32's used as a photogrammetry 3D-Scanner

This project is a riff on https://github.com/ldab/ESP32-CAM-Picture-Sharing
Thank you, ldab! (Is there a way to poke/mark users on github?) 

So far it works with 10 Units - hardwawre is described for 10

Little disclaimer: I'm a total noob. Also i fugured this is actually so easy, total beginners might give it a shot, so it might be a little over explained for some.

The code was slighly changed so:

- Units don't sleep
- Units upload to local ftp
- Image quality at max


How to:

Hardware: 

ESP-32s with OV2640 Cam (AI Thinker) 

FTDI-Programmer 

Power supply: 5V,  20A <- seems overkill but keeps room for ~20 units and overhead

Cables: Take care which cables you use from mains to psu and psu to connector. All elements like switches have to be rated for the amps you use

Connector: You need some high amp connector that doesnt go poof. I used a XT60.

Test code with one if it works this is wiring for 10 Units

Wall outlet ---220V, 10A--- PSU (Line one/ fist V-, first V+) --- 5V, 10A --- Connector --- (solder 10 dupont cables to each ground and power of the connector.) single dupont connecton --- ESP


Software:

As the original project this was done in PlatformIO for VisualStudio Code. 
(Its rather buggy, but has its benefits (intellisense)).

FTP Server: 
I set up an old macbook that was collecting dust. The installed OS (el Capitan) still has native support for FTP and was the quickest solution. 
Windows users might want to try fileZilla server or other free ftp server software
Linux... Well you're on linux, you'll figure something out, I guess...

Instructions:

Blynk:
-Download the BLYNK app to your smartphone
-Open a new project and add a Real Time Clock, a button and a slider.
-in your project settings setup 10 devices (All ESP32 Dev Board, connection type WiFi)
-add all devices to a tag // if you later add devices add them to the tag
-email yourself all auth tokens for the 10 decives

-Change the RTC to your locations time zone
-Change the Button output to V5 with mode push
-Change slider to V10 with range from 0 to 4 // this might change in later versions

Code:

-Set char auth[] for each token created in BLYNK
-Fill in your FTP Server and WiFi credentials
-Set the Filename (line 77) according to the folder structure of your ftp.
 It makes sense to name the file according to your unit number. Some modules might have connection issues or a smudge on the lens...

You're done.
Upload the code to the ESP, start FTP Server and Blynk, hit the button in the app. You should see a file at your set location and filename with a timestamp in its name.

TODO: 
-Energy optimisation. The ESP's are a little greedy right now

-Prepare the project for higher unit count:
  Trigger camera capture with physical button - then log 10 units into wifi and upload - wait - the next 10
  
-Find a better solution to dim the onboard flash LED on pin4. It works but its not really practical. The lowest level is still way too bright.



Special thanks to u/lost_electron! I think he saved my life.

