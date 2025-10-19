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


## Finding 2 - CVE-2025-6524: Unauthenticated File Storage Allowing Remote Dumping of Video Footage and Live Video Stream

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

## Finding 3 - CVE-2025-6525: Unauthorised Configuration Change

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

## Finding 4 - CVE-2025-6526: Exposed Root Password via Unauthenticated HTTP Server

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


## Finding 5 - CVE-2025-6527: Remotely Dump All Sensitive Video & Audio Recordings

**Description**: The 70mai Dashcam M300 has port 23 open with weak authentication such that an attacker connecting to the dashcam's network via default credentials, without needing device-pairing, can obtain a full list of video recordings and dump them out.

Although directory listing is disabled on the web server for the video recordings stored on SD card to prevent unauthorised personnel from downloading the videos:

![image](https://github.com/user-attachments/assets/2e6c8c06-393b-48d5-b943-96f04c1a1ce3)

There a a full bypass by obtaining the video recordings via telnet instead:

![image](https://github.com/user-attachments/assets/1a19d4d5-5bf3-4a89-9619-308ffd61ee9d)

Then downloading them via curl or http:

![image](https://github.com/user-attachments/assets/5f4e1df1-641c-4f6e-a14d-a7cb4661e610)


**Vulnerability Type**: Incorrect Access Control

**Vendor of Product**: 70mai

**Affected Product Code Base**: Dash Cam M300

**Affected Component**: Weak Telnet Authentication

**Attack Type**: Remote

**Impact Code execution**: True

**Impact Information Disclosure**: True

**Attack Vectors**: A remote attacker nearby connected to the dashcam's network can access the dashcam's telnet session as root user and fetch a full list of sensitive video recordings.

**Has vendor confirmed or acknowledged the vulnerability?**: No


## Finding 6 - CVE-2025-6528: Unauthenticated Live Video Stream

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



## Finding 7 - CVE-2025-6529: Remotely Upload Malicious Files and Execute Code

**Description**: The 70mai Dashcam M300 has port 23 open with weak authentication such that an attacker connecting to the dashcam's network via default credentials, without needing device-pairing, can upload arbitrary/malicious files or even replace firmware via editing the auto-run script(s). 

![image](https://github.com/user-attachments/assets/bf609930-4d38-4962-b794-4c24a892ca1d)


**Vulnerability Type**: Incorrect Access Control

**Vendor of Product**: 70mai

**Affected Product Code Base**: Dash Cam M300

**Affected Component**: OS Write Permissions

**Attack Type**: Remote

**Impact Code execution**: True

**Impact Information Disclosure**: True

**Attack Vectors**: A remote attacker nearby connected to the dashcam's network can write arbitrary code into the dashcam memory or SD, run malicious commands (RCE), or even replace the firmware with a malicious one.

**Has vendor confirmed or acknowledged the vulnerability?**: No


## Finding 8 - CVE-2025-6530: Remotely Crashing the Dashcam

**Description**: If telnet is accessed on the 70mai Dashcam M300, a default script (demo.sh) is run automatically. In order to interact with the telnet session, the user needs to break the running of the script:

![image](https://github.com/user-attachments/assets/1d04320b-0897-4f18-bcbb-939f51bc1e18)

If not, the dashcam would continue running the default script, which is unintended, and that causes a natural crash and the dashcam light turns from green to blinking blue, then red, effectively stuck at a disabled state until the battery power drains out and it reboots.

![image](https://github.com/user-attachments/assets/796778ac-1ebc-44f6-8a9f-e8de33781ce5)

This creates a DoS on the dashcam. Yes, this dashcam has a weak network stack.

**Vulnerability Type**: Incorrect Access Control

**Vendor of Product**: 70mai

**Affected Product Code Base**: Dash Cam M300

**Affected Component**: OS 

**Attack Type**: Remote

**Impact Code execution**: True

**Impact Information Disclosure**: True

**Attack Vectors**: A remote attacker nearby can connect to the dashcam's telnet session to crash the camera and disable it. 

**Has vendor confirmed or acknowledged the vulnerability?**: No


# 70mai Dashcam Omni X200

**Product**: Dashcam

**Version**: Omni X200

**Product URL**: https://www.70mai.com/global/omni/


## Finding 9 - CVE-2025-11942: Bypass Device Pairing of 70mai Dashcam Omni X200

**Description**: From the official 70mai mobile app, a user needs to perform authorization by clicking on the physical power button in order to connect to the dashcam’s network. However, by connecting to the dashcam’s network and directly accessing the API on port 80 and RTSP on port 554, an attacker can bypass the device authorization mechanism that requires a user to physically press on the power button during connection. Moreover, the http and rtsp services are not protected by any form of authentication.

<img width="700" height="443" alt="image" src="https://github.com/user-attachments/assets/31c422e7-c273-4f33-8c68-988e9daf6dbd" />


**Vulnerability Type**: Incorrect Access Control

**Vendor of Product**: 70mai

**Affected Product Code Base**: Dash Cam Omni X200

**Affected Component**: Authentication mechanism

**Attack Type**: Remote

**Impact Code execution**: False

**Impact Information Disclosure**: True

**Attack Vectors**: A remote attacker nearby can connect to the dashcam's services without needing to physically pair the device.

**Has vendor confirmed or acknowledged the vulnerability?**: No


## Finding 10 - CVE-2025-11943: Exposed Root Password via Unauthenticated HTTP Server

**Description**: The 70mai Dashcam Omni X200 has port 80 open without authentication such that an attacker connecting to the dashcam's network via default credentials can access all files on it. 

<img width="265" height="85" alt="image" src="https://github.com/user-attachments/assets/3abf5897-2b58-41ef-8b17-3232e67a1e4f" />
<br/>
<img width="265" height="47" alt="image" src="https://github.com/user-attachments/assets/9ed75584-d843-4020-90cc-c82e1b279d66" />
<br/>
<img width="265" height="214" alt="image" src="https://github.com/user-attachments/assets/4e202783-427d-4f5f-ac43-f51e2ddf1e34" />
<br/>
Even if the OS's password is changed, it would still be exposed by the http server.

**Vulnerability Type**: Incorrect Access Control

**Vendor of Product**: 70mai

**Affected Product Code Base**: Dash Cam Omni X200

**Affected Component**: Unauthenticated Web Server

**Attack Type**: Remote

**Impact Code execution**: False

**Impact Information Disclosure**: True

**Attack Vectors**: A remote attacker nearby connected to the dashcam's network can access all files on the web server without going through authentication or device pairing and can obtain the root password.

**Has vendor confirmed or acknowledged the vulnerability?**: No





## Disclosure timeline

31 Jan 2025 - Responsible disclosure to manufacturer

15 Feb 2025 - A follow-up email to manufacturer





