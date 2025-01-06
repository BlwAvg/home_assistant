# ESP32-S3-Box-3
Home Assistant configuration for on-device micro wake word detection. This setup includes radar, temperature, humidity, and battery monitoring. IR support is excluded but can be added if needed. The configuration is based on multiple forum posts, with deviations from the [official configuration](https://github.com/esphome/wake-word-voice-assistants/blob/main/esp32-s3-box-3/esp32-s3-box-3.yaml) clearly marked. Search for "##" in the yaml file to locate changes.

Notes:
- The "Disable When Absent" feature mutes the mic and turns off the screen when presence is not detected.
- These additional sensors only exist on the [ESP32-S3-BOX-3-Sensor](https://www.espressif.com/en/news/ESP32-S3-BOX-3) dock and not on the ESP32-S3-Box-3 screen.

## Sources
- [Glyphs and Compile Fix](https://community.home-assistant.io/t/compiling-esp32-s3-box-3-fails-and-gives-failed-config-font-is-missing-791-glyphs/797536/28)
- [Radar Config](https://github.com/esphome/feature-requests/issues/2475#issuecomment-1879449021)
- [Humditiy and Temp Config](https://community.home-assistant.io/t/esp32-s3-box3/638287/106)
- [Battery Config](https://community.home-assistant.io/t/esp32-s3-box3/638287/110?page=6)

## HA Dashboard Reference

![Sensor](/esp32-s3-box-3/Sensors.png)

![Config and Diagnostics](/esp32-s3-box-3/Config&Diagnostics.png)

## ESPHome Configuration
Below is my ESP Home config on my device. This overrides specifics in the main YAML file pulled from github. I wanted to keep the source as close to the original as possible and just add features. Hopefully someone else finds this useful.

```
---
substitutions:
  name: voice-assistant                 ## Name your device whatever you want
  friendly_name: Voice Assistant        ## ^^^^^^^^^^
  micro_wake_word_model: hey_jarvis     ## Change to whatever option you prefer: https://github.com/esphome/micro-wake-word-models 

packages:                               ## This pulls this repo for the full configuration.
  esphome.voice-assistant:
    url: https://github.com/BlwAvg/home_assistant
    files: esp32-s3-box-3/esp32-s3-box-3.yaml
    ref: main
    refresh: 1s

esphome:
  name: ${name}
  name_add_mac_suffix: false            ## Change from the default which is "true"
  friendly_name: ${friendly_name}

esp32:
  board: esp32s3box
  framework:
    type: esp-idf
    version: 4.4.8                      ## Changing out the versions or this will not compile on the local device.
#    version: 5.3.0
#    #version: recommended
    platform_version: 5.4.0
#    platform_version: 6.8.1            ## Changing out the versions or this will not compile on the local device.

api:
  on_client_connected:
    - script.execute: draw_display
  on_client_disconnected:
    - script.execute: draw_display
  encryption:
    key: !secret va-api                 ## pulling keys from secrets.yaml

ota:
  - platform: esphome
    password: !secret va-ota-password   ## pulling keys from secrets.yaml

wifi:
  networks:                             ## You can also add domain: under this to include a domain.
    - ssid: !secret wifi_ssid
      password: !secret wifi_password
#      hidden: true                      ## Only needed if network is hiddend. I have this commented this out from this configfuration file because I assume my setup is more niche, just left it for reference.
  on_connect:
    - script.execute: draw_display
  on_disconnect:
    - script.execute: draw_display

#mdns:                                  ## I have this commented this out from this configfuration file because I assume my setup is more niche, just left it for reference.
#  disabled: false
```
