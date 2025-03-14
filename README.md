# 70mai Dashcam 1S

Product: https://www.70mai.com/cam1s

## Finding: Bypass Device Pairing of 70mai Dashcam 1S

From the official 70mai mobile app, a user needs to perform authorization by clicking on the physical power button in order to connect to the dashcam’s network. However, by connecting to the dashcam’s network and directly accessing the API on port 80 and RTSP on port 554, an attacker can bypass the device authorization mechanism that requires a user to physically press on the power button during connection.

This is the intended flow:

https://github.com/user-attachments/assets/f917d4c3-891a-4205-8fad-1e1b5d62463e

This is the bypass - once we get a list of video file names, we can proceed with downloading them:


https://github.com/user-attachments/assets/eb0532f9-96f5-40cc-b0e5-2dd4c186148d


**Disclosure timeline**

31 Jan 2025 - Responsible disclosure to manufacturer

15 Feb 2025 - A follow-up email to manufacturer





