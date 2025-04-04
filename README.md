# **CIRCUTOR CVM-BD Power Meter / Analyzer ESPHome Integration**

The CVM-BD Power Meter is a programable measuring instrument
supports RS485 Modbus Protocol communication.

## **Requirements**

- ESP32 device with 320KB RAM, 4MB Flash and RS485 Adapter
- Recommended devices, with RS485 on board:
  - [T-CAN485](https://lilygo.cc/products/t-can485)
  - [KAmodESP32 POW RS485](https://wiki.kamamilabs.com/index.php?title=KAmodESP32_POW_RS485)
  - any other ESP32 device with RS485 interface
- [ESPhome](https://github.com/esphome/esphome/releases)
- [HomeAssistant](https://www.home-assistant.io/)
- esphome should installed on your computer or HASS instance (ESPHome Builder)

## **ESP configuration**

Example configuration in `lilygo-jk0.yaml`

<details><summary>lilygo-jk0.yaml - click here to expand</summary>

```yaml
substitutions:
  modbus_contr_addr: "10"
  modbus_contr_id: cvm

esphome:
  name: esp32-s3
  friendly_name: ESP32-S3 ESP Circutor CVM-BD-RED
  platformio_options:
    build_flags: -DBOARD_HAS_PSRAM
    board_build.arduino.memory_type: qio_opi
    board_build.f_flash: 80000000L
    board_build.flash_mode: qio 

esp32:
  board: esp32-s3-devkitc-1
  framework:
    type: arduino

# Enable logging
logger:
    level: DEBUG
    # level: VERY_VERBOSE


# Enable Home Assistant API
api:
  encryption:
    key: "<your_generated_key>"

ota:
  - platform: esphome
    password: "<your_ota_password>"

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password
  power_save_mode: none

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "esp32-S3  Fallback Hotspot"
    password: "<your_hotspot_password>"

captive_portal:

time:
  - platform: homeassistant
    id: homeassistant_time
 
uart:
  id: mod_uart
  tx_pin: GPIO43
  rx_pin: GPIO44
  baud_rate: 19200
  stop_bits: 1
  rx_buffer_size: 512
  debug:
#    direction: BOTH
#    dummy_receiver: false
#    after:
#      delimiter: "\n"
#    sequence:
#      - lambda: UARTDebug::log_string(direction, bytes);
#      - lambda: UARTDebug::log_hex(direction, bytes);

modbus:
  flow_control_pin: GPIO9
  id: modbus1
  uart_id: mod_uart

modbus_controller:
  - id: ${modbus_contr_id}
    address: ${modbus_contr_addr}
    modbus_id: modbus1
    setup_priority: -10
    update_interval: 1s
    command_throttle: 75ms

sensor: !include ./include/cvm-bd.yaml
text_sensor: !include ./include/cvm-bd-t.yaml
```
</details>

## **Known issues**

## **TO DO**

## **Licence**

|[![](images/ASF_Logo.svg)](https://www.apache.org/licenses/LICENSE-2.0)|
|-|
| Apache License, Version 2.0 |

## **References**

* <https://gitlab.stn.pl/picoides-monitor/power_meter/circutor_cvm-bd>
 
