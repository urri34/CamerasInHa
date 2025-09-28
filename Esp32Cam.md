## ESP32-cam

### Software

If adding an esp32 camera, it has no rtsp stream and needs to be added in special way using http. (Assuming standard esp32 cam config).

### Config

First lets install "ESPHome Device Builder" Add-on for Home Assistant. [Official guide](https://esphome.io/guides/getting_started_hassio/) Then simply create an empty device selecting esp2 type. Once done, it will appear in the webpage, In my case I want to configure the cams with the following options:

- Hostname different for each cam, this example is cam2
- Static ip address in order to keep some order in my LAN as I have different segments for each type of device
- A fallback hotspot so in case the esp32-cam is not able to connect to my wifi it will generate a hotspot and I can connect to that in order to repair the cam config. The only presence of a esp32-cam* hostspot will let me know something is not properly working.
- Web server with basic info in port 80
- Streaming realtime server in port 8080
- Snapshot server in port 8081
- Sensor for wifi signal strenght
- Sensor for uptime
- Button to start the embeded light.

Select edit and paste something like the following:

```
esphome:
  name: esp32-cam2
  friendly_name: ESP32-CAM2

esp32:
  board: esp32dev
  framework:
    type: arduino

# Enable logging
logger:

# Enable Home Assistant API
api:
  encryption:
    key: "F75g5jG//E+BZ+m+BRpoD/rbX4bdu3aPzqpmvs6r807="

ota:
  - platform: esphome
    password: "88d248a4ad16c1358825fd22d38bbd37"

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

  # Optional manual IP
  manual_ip:
    static_ip: 192.168.1.8
    gateway: 192.168.1.1
    subnet: 255.255.255.0

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "esp32-cam2"
    password: "esp32-cam2"

web_server:
  port: 80

captive_portal:

esp32_camera:
  external_clock:
    pin: GPIO0
    frequency: 20MHz
  i2c_pins:
    sda: GPIO26
    scl: GPIO27
  data_pins: [GPIO5, GPIO18, GPIO19, GPIO21, GPIO36, GPIO39, GPIO34, GPIO35]
  vsync_pin: GPIO25
  href_pin: GPIO23
  pixel_clock_pin: GPIO22
  power_down_pin: GPIO32
  vertical_flip: False
  horizontal_mirror: False

  # Image settings
  name: ESP CAM
  resolution: 800X600

sensor:
  - platform: wifi_signal
    name: "WiFi Signal Sensor"
    update_interval: 2s
  - platform: uptime
    name: Uptime Sensor

esp32_camera_web_server:
  - port: 8080
    mode: stream
  - port: 8081
    mode: snapshot

# Flashlight
output:
  - platform: gpio
    pin: GPIO4
    id: gpio_4

## GPIO_4 is the flash light pin
light:
  - platform: binary
    output: gpio_4
    name: flashlight
```
If you dont feel comfortable inventing the part about
```
api:
  encryption:
    key: "F75g5jG//E+BZ+m+BRpoD/rbX4bdu3aPzqpmvs6r807="

ota:
  - platform: esphome
    password: "88d248a4ad16c1358825fd22d38bbd37"
```
You can copy it from the original device created.

On the top right of the webpage a link called "Secrets" should appear. It's used to store there the SSID and the password of the wifi instead of storing them in the main config files. You should edit this and be sure that the informations stored is the right one.
```
# Your Wi-Fi SSID and password
wifi_ssid: "MyRealWifiSSI"
wifi_password: "MyUltraSecretPassword"
```
Config done! 

### ESP32-CAM FW install

Now click on the three dots in the device, and select install. It's a must for the first time to select the manual download option. It's going to start with the compilation of the firmware for your esp32-cam device (it can take up to 10 minuts depending on yur hardware, but you can see it working in real time). Once it finish we are going to download it in factory format. 

Firmware done! Lets connect your esp32-cam to your computer with a data USB cable, WITH A DATA USB CABLE! You have to be sure that the USB cable is able to send data not only power. Now, lets open [](https://web.esphome.io/) . We are going to click on the "Connect button", the select the port and 

![My HA ESP32-Cam config](https://github.com/urri34/IpCamsInHa/blob/main/ESP32CamHAConfig.jpg.PNG).
