# xBlinds

The xBlinds project started because I wanted a (cheap) way to motorize my vertical blinds and existing commercial solutions are heavily priced and not getting the best reviews, so a wintery night I started sketching the UI in Notepad.

This project is two-fold; the bin and guide published here and the STL for the casing on Thingiverse https://www.thingiverse.com/thing:4792584.

# esphome-blinds [![Made for ESPHome](https://img.shields.io/badge/Made_for-ESPHome-black?logo=esphome)](https://esphome.io)

This [ESPHome](https://esphome.io) package allows control of blinds using a stepper motor.

TLDR; Add this to your ESPHome device configuration:

```yaml
substitutions:
  esp_pin_a: D1
  esp_pin_b: D2
  esp_pin_c: D3
  esp_pin_d: D4
  stepper_max_speed: '250'
  stepper_endstop: '650'

packages:
  blinds: github://RGeneral/esphome-blinds/esphome-blinds.yaml@main

esp8266:
  board: d1_mini
  restore_from_flash: true
```

## Hardware installation

Follow this video guide by The Hook Up but skip their software installation:

[![Motorize and Automate your Blinds for $10! (WiFi)](https://img.youtube.com/vi/1O_1gUFumQM/0.jpg)](https://www.youtube.com/watch?v=1O_1gUFumQM)

Wiring diagram (from https://github.com/thehookup/Motorized_MQTT_Blinds):

![Schematic](https://github.com/tronikos/esphome-blinds/assets/9987465/b3f5dcb2-1fac-4226-9320-2a7b9b56e135)

*Don't forget to cut the center trace on the stepper motor as shown in the youtube video.*

## Software installation

1. Setup **ESPHome**, if you don't have it already, by following [Getting Started with ESPHome and Home Assistant](https://esphome.io/guides/getting_started_hassio.html).
2. In the **ESPHome Dashboard** select **New device**, **Continue**, give a name: e.g. Kitchen blinds, **Next**, select device type based on the ESP chip used e.g. ESP8266.
3. In the **Configuration created!** page select **Skip** to skip installation for now until we make a few changes.
4. Select **Edit** on the created configuration e.g. kitchen-blinds.yaml.
5. Skip this step if you used an `esp32`. Change `esp8266` section to:

```yaml
esp8266:
  board: d1_mini
  restore_from_flash: true
```

6. Add the following (either at the beginning or the end of the file):

```yaml
substitutions:
  esp_pin_a: D1
  esp_pin_b: D2
  esp_pin_c: D3
  esp_pin_d: D4
  stepper_max_speed: '250'
  stepper_endstop: '650'

packages:
  blinds: github://RGeneral/esphome-blinds/esphome-blinds.yaml@main
```

7. Change the values in the `substitutions` section based on your setting, e.g. if you have used different pins. The `stepper_endstop: '650'` for my blinds corresponds to the blinds being open, i.e. parallel to the ground `-`. 
8. Your configuration should now look something like the following:

```yaml
substitutions:
  esp_pin_a: D1
  esp_pin_b: D2
  esp_pin_c: D3
  esp_pin_d: D4
  stepper_max_speed: '250'
  stepper_endstop: '650'

packages:
  blinds: github://RGeneral/esphome-blinds/esphome-blinds.yaml@main

esphome:
  name: kitchen-blinds
  friendly_name: Kitchen blinds

esp8266:
  board: d1_mini
  restore_from_flash: true

# Enable logging
logger:

# Enable Home Assistant API
api:
  encryption:
    key: "L8408egzTATPCBT1nzvFpqj4YlVERRO31+GyB/yjf4E="

ota:
  - platform: esphome
    password: "d44ed9df293facf65e288062d5c7a5e7"

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "Kitchen-Blinds Fallback Hotspot"
    password: "8cSGOshkb2Rw"

captive_portal:
    
```

9.  Select **Save** and then **Install**.
10. Only for the first install select **Plug into this computer**. For subsequent updates/installs you can install **Wirelessly**.
11. Select **Download project** to save a bin file.
12. Select **Open ESPHome Web**, **Connect**, **Install downloaded project**.
13. In the **Install your existing ESPHome project** page select **Choose File**, select the previously downloaded bin file, and select **Install**.
14. Home Assistant should auto-discover your new device.
### Toogle button

Version 0.5 adds support for a tactile button to toggle open/close directly on the xBlinds unit. It will require printing a new lid (available on Thingiverse) for the unit and it's completely optional.


### Thingiverse

If you want to 3D print this project, go to Thingiverse to download the STLs here: https://www.thingiverse.com/thing:4792584

Here's the xblinds unit assembled: (You can safely desolder the LED's on the ULN2003 stepper driver if you should want to).

![3D-print](https://github.com/kp-bit/xblinds/raw/main/images/xblinds-open.jpg)

And this is what it looks like mounted on the wall:

![Mounted](https://github.com/kp-bit/xblinds/raw/main/images/xblinds-mounted.jpg)


### Bill of Materials

#### Steppers and drivers:
ANGEEK 5 pcs. 5V 28BYJ-48 ULN2003 Stepper Motor with Drive Module Board

* Amazon affiliate link: https://amzn.to/4bvPhqd


#### D1 mini:
AZDelivery D1 Mini NodeMcu with ESP8266-12F WLAN Module

> Note!
> Different manufacturers have different dimensions.

* Amazon affiliate link: https://amzn.to/49CFDQR
* AZDelivery: https://www.az-delivery.de/products/d1-mini

#### Tactile Button:
Tactile Push Button Switch 6x6x4.3 (6x6x5 should be fine as well)<br/>
10 kOhm pull-down resistor

* Amazon affiliate link: https://amzn.to/49eNN1K

#### Power supply:
Any 5V PSU will do, I've calculated ~2A per unit to be on the safe side.<br/>
**Do not use USB to power the D1 and stepper, the 5V pin will not deliver sufficient current to drive the stepper**


### Wiring diagram

![Wiring](https://github.com/kp-bit/xblinds/raw/main/images/diagram.jpg)

