# ESP-3D-Scanner
### Proof of concept for an array of ESP32's used as a photogrammetry 3D-Scanner
Little disclaimer: I'm a total noob. But I figured this is actually so easy, other total beginners might give it a shot, thus it might be a little over explained for some.

This project is a riff on https://github.com/ldab/ESP32-CAM-Picture-Sharing
Thank you, ldab! (Is there a way to poke/mark users on github?) All prais belongs to you. Your license is included here. 

So far the scanner works with 10 Units - hardware is described for 10. 

The original code was slighly changed so:

- Units don't sleep and are ready to take a picture
- enable  / dim flash led
- Image quality set to max
- image view in BLYNK currently disabled



## How to:

### Hardware: 

10x ESP-32s with OV2640 Cam (AI Thinker) 

1x FTDI-Programmer 

1x Power supply: 5V,  20A <- seems overkill but keeps room for ~20 units and overhead

1x Cardboard or similar: 60cm by >20cm 

<img src="ESP%203D-Scanner/pics/IMG_2611%202.jpg" width="600">

Arrange all units on the cardboard. 

<img src="ESP%203D-Scanner/pics/IMG_2612.jpg" width="800">


# !!! Be aware! If you use wrong cables you might burn down your house! 5v seems harmless but the amps arent! Go and ask an adult like me ;)!!!

## Cables: Take care which cables you use from mains to psu and psu to connector. All elements like switches have to be rated for the amps you use

## Connector: You need some high amp connector that doesnt go poof. I used a XT60.

<img src="ESP%203D-Scanner/pics/IMG_2613.jpg" width="800">

20+ dupont cables female to female preferably in red and black

Test code with one. If it works, this is wiring for 10 Units:

Wall outlet ---220V, 10A--- PSU (connector line one/ fist V-, first V+) --- 5V, 10A --- Connector --- (solder 10 dupont cables to each ground and power of the connector.) single dupont connecton --- ESP


<img src="ESP%203D-Scanner/pics/IMG_2614.jpg" width="800">



### Software:

Likethe original project this was done in PlatformIO for VisualStudio Code. Check out ldab's git for instructions. You might have to update python to its latest version if you get stuck. 
(Its rather buggy, but has its benefits (intellisense)).

FTP Server: 
I set up an old macbook that was collecting dust. The installed OS (el Capitan) still has native support for FTP and was the quickest solution. 
Windows users might want to try fileZilla server or other free ftp server software
Linux... Well you're on linux, you'll figure something out, I guess...

Photogrammetry Software: 
My software of choise is reality capture. It comes at 370$ for indie devs but is free for students.
There is also a ton of free other sofware - google will help you out.

## Instructions:

### Blynk:


- Download the BLYNK app to your smartphone

You can either clone the setup via this qr code 

<img src="ESP%203D-Scanner/pics/3d_SCANNER%20BLYNK.PNG" width="200">

or for better understanding:

- Open a new project and add a Real Time Clock, a button and a slider.
- in your project settings setup 10 devices (All ESP32 Dev Board, connection type WiFi)
- add all devices to a tag // if you later add devices add them to the tag
- email yourself all auth tokens for the 10 decives

- Change the RTC to your locations time zone
- Change the Button output to V5 with mode push with target device = tag
- Change slider to V10 with range from 0 to 4 // this might change in later versions

- (Optional) set up an image view on V0

### Code:

```
char auth[] = ""; //1
//char auth[] = ""; //2
//char auth[] = ""; //3
//char auth[] = ""; //4
//char auth[] = ""; //5
//char auth[] = ""; //6
//char auth[] = ""; //7
//char auth[] = ""; //8
//char auth[] = ""; //9
//char auth[] = ""; //10

// Your WiFi credentials.
char ssid[] = "";
char pass[] = "";

// FTP Server credentials
char ftp_server[] = "";
char ftp_user[]   = "";
char ftp_pass[]   = "";

// Camera buffer, URL and picture name
camera_fb_t *fb = NULL;
String pic_name = "Desktop/SCAN/Module1 -"; // example for folder corresponding on ftp server with Module name / change accordingly
```

- Set char auth[] for each token created in BLYNK
- Fill in your FTP Server and WiFi credentials
- Set the Filename (line 77) according to the folder structure of your ftp.

 It makes sense to name the file according to your unit number. Some modules might have connection issues or a smudge on the lens...
 
- Change filename and auth when uploading code according to unit

- Upload the code to the ESP

## Testing

- Power the system

- Start FTP Server 

- Bend the Cardbord slightly, so each camera is ponted at a diggerent angle towards you (or to object you want to scan)

- Start Blynk and hit the button in the app. 

You should see files appearing at your set location with module number and timestamp in its name. Wait till the file size stops increasing. Check the pictures for artifacts and errors.

- Shove the files in you photogrammetry software and see what happens.

## TODO: 

- Energy optimisation. The ESP's are a little greedy right now.

- Prepare the project for higher unit count:
  Trigger camera capture with physical button - then log 10 units into wifi - upload - logout - the next 10
  
- Find a better solution to dim the onboard flash LED on pin4. It works but it is not really practical. The lowest level is still way too bright.



Special thanks to u/lost_electron! I think he saved my life. See "!!!Be aware..."

