# IpCamsInHa
Things I have learned about IP Cams in Home Assistant environment. I like to use POE cams and each one has it's one special things wich sometimes are really driving me crazy.

## IPCAM

How to identify this kind of cams:

### In the web page that they are publishing:
```
Device ID:        IPCAM
Device Type:      C6F0SoZ3N0PlL2
Software Version: V30.1.22.16.3-20240508
Webware Version:  V3.0.7.6
PTZ Version:      V0.0
```
### In aliexpress definition sheet
```
Model Number     JN-409-E-POE
Brand Name       JIENUO
APP              Camhipro / Camhi
```
### RTSP construction:

[more info](https://camhi.pro/how-to-add-wifi-camera-to-ispy/)
- rtsp://user:password@ip:554/11
- rtsp://user:password@ip:554/12
 
### Snapshot:

- http://<ip>/tmpfs/auto.jpg

Strictly it's called thru javascript:
```
img2345.src = "/tmpfs/auto.jpg?" + (new Date()).getTime();
```
