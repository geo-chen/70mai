# 70mai Dashcam 1S

**Product**: Dashcam

**Version**: 1S

**Product URL**: https://www.70mai.com/cam1s

## Finding 1 - CVE-2025-30112: Bypass Device Pairing of 70mai Dashcam 1S

From the official 70mai mobile app, a user needs to perform authorization by clicking on the physical power button in order to connect to the dashcam’s network. However, by connecting to the dashcam’s network and directly accessing the API on port 80 and RTSP on port 554, an attacker can bypass the device authorization mechanism that requires a user to physically press on the power button during connection.

This is the intended flow:

https://github.com/user-attachments/assets/f917d4c3-891a-4205-8fad-1e1b5d62463e

This is the bypass:


https://github.com/user-attachments/assets/eb0532f9-96f5-40cc-b0e5-2dd4c186148d


## Finding 2: Unauthenticated File Storage Allowing Remote Dumping of Video Footage and Live Video Stream

**Description**: Once connected to the network of 70mai Dashcam 1S, all video recordings can be dumped via http://192.72.1.1/SD/Normal/$FILE_NAME without any http-level authentication:

```
http://192.72.1.1/SD/Normal/$FILE_NAME
```
The RTSP feed can also be accessed directly at port 554 - rtsp://192.72.1.1/liveRTSP/av4:

```
rtsp://192.72.1.1/liveRTSP/av4
```

**Vulnerability Type**: Incorrect Access Control

**Vendor of Product**: 70mai

**Affected Product Code Base**: Dash Cam 1S

**Affected Component**: Unauthenticated Video Services

**Attack Type**: Remote

**Impact Code execution**: False

**Impact Information Disclosure**: True

**Attack Vectors**: A remote attacker nearby can connect to the dashcam to view livestream or dump recorded sensitive media files.

**Has vendor confirmed or acknowledged the vulnerability?**: No

## Finding 3: Unauthorised Configuration Change

**Description**: Once connected to the network of 70mai Dashcam 1S, an attacker can make unauthorised configuration changes to the dashcam and even sabotage the car battery to drain it by disabling the battery protection settings:
```
curl -s "http://192.72.1.1/cgi-bin/Config.cgi?action=set&property=Camera.Menu.<REDACTED>
```

![image](https://github.com/user-attachments/assets/73a5ca50-1b68-4d96-86c7-1af22874f837)

**Vulnerability Type**: Incorrect Access Control

**Vendor of Product**: 70mai

**Affected Product Code Base**: Dash Cam 1S

**Affected Component**: Unauthenticated Configuration Management

**Attack Type**: Remote

**Impact Code execution**: False

**Impact Information Disclosure**: True

**Attack Vectors**: A remote attacker nearby connected to the dashcam can make unauthorised changes to the dashcam's configurations without alerting the dashcam owner or pressing any physical pairing button on the dashcam.

**Has vendor confirmed or acknowledged the vulnerability?**: No



# 70mai Dashcam M300

**Product**: Dashcam

**Version**: M300

**Product URL**: https://www.70mai.com/m300

## Finding 4: Exposed Root Password via Unauthenticated HTTP Server

**Description**: The 70mai Dashcam M300 has port 80 open without authentication such that an attacker connecting to the dashcam's network via default credentials, without needing device-pairing, can access all files on it. 

![image](https://github.com/user-attachments/assets/a1990e6c-a081-4df9-85d4-fd613d2e978f)


From the web server, we obtain the root password hash and derive that it's using an empty password.

![image](https://github.com/user-attachments/assets/ecfb1270-8716-4248-a064-0150e4c504ad)


**Vulnerability Type**: Incorrect Access Control

**Vendor of Product**: 70mai

**Affected Product Code Base**: Dash Cam M300

**Affected Component**: Unauthenticated Web Server

**Attack Type**: Remote

**Impact Code execution**: False

**Impact Information Disclosure**: True

**Attack Vectors**: A remote attacker nearby connected to the dashcam's network can access all files on the web server without going through authentication or device pairing and can obtain the root password.

**Has vendor confirmed or acknowledged the vulnerability?**: No


## Finding 5: Remotely Dump All Sensitive Video & Audio Recordings

**Description**: The 70mai Dashcam M300 has port 23 open with weak authentication such that an attacker connecting to the dashcam's network via default credentials, without needing device-pairing, can obtain a full list of video recordings and dump them out.

Although directory listing is disabled on the web server for the video recordings stored on SD card:

![image](https://github.com/user-attachments/assets/2e6c8c06-393b-48d5-b943-96f04c1a1ce3)

We can obtain a full list of video recordings via telnet:

![image](https://github.com/user-attachments/assets/1a19d4d5-5bf3-4a89-9619-308ffd61ee9d)

Then downloading them via curl or http. 

**Vulnerability Type**: Incorrect Access Control

**Vendor of Product**: 70mai

**Affected Product Code Base**: Dash Cam M300

**Affected Component**: Weak Telnet Authentication

**Attack Type**: Remote

**Impact Code execution**: True

**Impact Information Disclosure**: True

**Attack Vectors**: A remote attacker nearby connected to the dashcam's network can access the dashcam's telnet session as root user and fetch a full list of sensitive video recordings.

**Has vendor confirmed or acknowledged the vulnerability?**: No


## Finding 6: Unauthenticated Live Video Stream

**Description**: Once connected to the network of 70mai Dashcam M300, an attacker can remotely access the live stream of the dashcam without authentication using the rtsp port:

```
rtsp://192.168.0.1:554/livestream/12
```

**Vulnerability Type**: Incorrect Access Control

**Vendor of Product**: 70mai

**Affected Product Code Base**: Dash Cam M300

**Affected Component**: Unauthenticated Video Services

**Attack Type**: Remote

**Impact Code execution**: False

**Impact Information Disclosure**: True

**Attack Vectors**: A remote attacker nearby can connect to the dashcam to view livestream without the dashcam owner's knowledge (no voice guidance or sounds triggered).

**Has vendor confirmed or acknowledged the vulnerability?**: No


![image](https://github.com/user-attachments/assets/66e2b7f0-4222-417c-bdd0-09723d106842)



## Finding 7: Remotely Upload Malicious Files and Execute Code

**Description**: The 70mai Dashcam M300 has port 23 open with weak authentication such that an attacker connecting to the dashcam's network via default credentials, without needing device-pairing, can upload arbitrary/malicious files or even replace firmware via editing the auto-run script(s). 

![Uploading image.png…]()


**Vulnerability Type**: Incorrect Access Control

**Vendor of Product**: 70mai

**Affected Product Code Base**: Dash Cam M300

**Affected Component**: OS Write Permissions

**Attack Type**: Remote

**Impact Code execution**: True

**Impact Information Disclosure**: True

**Attack Vectors**: A remote attacker nearby connected to the dashcam's network can write arbitrary code into the dashcam memory or SD, run malicious commands (RCE), or even replace the firmware with a malicious one.

**Has vendor confirmed or acknowledged the vulnerability?**: No




## Disclosure timeline

31 Jan 2025 - Responsible disclosure to manufacturer

15 Feb 2025 - A follow-up email to manufacturer





