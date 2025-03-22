# 70mai 

## Dashcam 1S

Product: https://www.70mai.com/cam1s

Version: Dash Cam 1S

## Finding 1 - CVE-2025-30112: Bypass Device Pairing of 70mai Dashcam 1S

From the official 70mai mobile app, a user needs to perform authorization by clicking on the physical power button in order to connect to the dashcam’s network. However, by connecting to the dashcam’s network and directly accessing the API on port 80 and RTSP on port 554, an attacker can bypass the device authorization mechanism that requires a user to physically press on the power button during connection.

This is the intended flow:

https://github.com/user-attachments/assets/f917d4c3-891a-4205-8fad-1e1b5d62463e

This is the bypass:


https://github.com/user-attachments/assets/eb0532f9-96f5-40cc-b0e5-2dd4c186148d


## Finding 2: Unauthenticated File Storage Allowing Remote Dumping of Video Footage and Live Video Stream

Once connected to the dashcam's network, all video recordings can be dumped via the following path without any http-level authentication:

```
http://192.72.1.1/SD/Normal/$FILE_NAME
```
The RTSP feed can also be accessed directly at port 554:

```
rtsp://192.72.1.1/liveRTSP/av4
```

## Finding 3: Unauthorised Configuration Change

Once connected to the dashcam's network, an attacker can make unauthorised configuration changes to the dashcam and even sabotage the car battery to drain it by disabling the battery protection settings:
```
curl -s "http://192.72.1.1/cgi-bin/Config.cgi?action=set&property=Camera.Menu.<REDACTED>
```

![image](https://github.com/user-attachments/assets/73a5ca50-1b68-4d96-86c7-1af22874f837)


**Disclosure timeline**

31 Jan 2025 - Responsible disclosure to manufacturer

15 Feb 2025 - A follow-up email to manufacturer





