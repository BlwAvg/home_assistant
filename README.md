# ESP32-S3-Box-3 
Home Assistant configuration for microwake word dectection (wake word on device). This includes enabling the radar, tempurature, humidity, and battery. I have no use for the IR, but if someone else needs it feel free to add it.

Below is my ESP Home config on my device. This overrides specifics in the main YAML file. I wanted to keep the source as close to the original as possible and just add features.

```
---
substitutions:
  name: voice-assistant                 ## Name your device whatever you want
  friendly_name: Voice Assistant        ## ^^^^^^^^^^
  micro_wake_word_model: hey_jarvis     ### Change to whateveroptions are available: https://github.com/esphome/micro-wake-word-models 

packages:                               ### This pulls this repo for the full configuration.
  esphome.voice-assistant:
    url: https://github.com/BlwAvg/home_assistant
    files: esp32-s3-box-3.yaml
    ref: main
    refresh: 1s

esphome:
  name: ${name}
  name_add_mac_suffix: false            ### Change from the default which is "true"
  friendly_name: ${friendly_name}

esp32:
  board: esp32s3box
  framework:
    type: esp-idf
    version: 4.4.8                      ### Changing out the versions or this will not compile on the local device.
#    version: 5.3.0
#    #version: recommended
    platform_version: 5.4.0
#    platform_version: 6.8.1            ### Changing out the versions or this will not compile on the local device.

api:
  on_client_connected:
    - script.execute: draw_display
  on_client_disconnected:
    - script.execute: draw_display
  encryption:
    key: !secret va-api                 ### pulling keys from secrets.yaml

ota:
  - platform: esphome
    password: !secret va-ota-password   ### pulling keys from secrets.yaml

wifi:
  networks:                            ### You can also add domain: under this to include a domain.
    - ssid: !secret wifi_ssid
      password: !secret wifi_password
      hidden: true
  on_connect:
    - script.execute: draw_display
  on_disconnect:
    - script.execute: draw_display

#mdns:                                  ### I have this disabled, I dont use mdns. I use ICMP. I have this commented this out from this configfuration file because I assume my setup is more niche, just left it for reference.
#  disabled: false
```
