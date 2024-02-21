# EcoForest-modbus-registers
EcoForest Heatpumps modbus registers (ESPHome format and others)

At first, the focus will be on geothermal version. The documentation referred below contains also the ecoAIR versions, so this repository can be extended easily.

The registers are described in heatpump-ecogeo.yaml (for EcoForest ecoGEO 1-6, 1-9, 3-12, 3-22 kW models).

See original source of information: https://www.docdroid.net/vecljSG/modbus-2021-en-v02-pdf#page=6

in your ESPHome node yaml, this example allows you to easily integrate if you have a local copy of heatpump-ecogeo.yaml:

```
substitutions:
  ## address of the Modbus slave device on the bus, default value is typically 17
  heatpump_modbus_address: "17"
  ## this exact name will be used in heatpump-ecogeo.yaml
  heatpump_uart_id: "modbus_uart"

uart:
  id: ${heatpump_uart_id}  
  tx_pin: GPIO17
  rx_pin: GPIO16
  baud_rate: 19200
  stop_bits: 2
  parity: NONE

packages:
  heatpump: !include heatpump-ecogeo.yaml
```

For this example, pin 16/17 are the UART pins used towards a MAX3485 board, to convert to RS485 (modbus RTU). Please connect the TX and RX of the ESP respectively to the RX and TX of that MAX3485 board.

Alternatively, it is possible to directly integrate from GitHub:
```
substitutions:
  ## address of the Modbus slave device on the bus, default value is typically 17
  heatpump_modbus_address: "17"
  ## this exact name will be used in heatpump-ecogeo.yaml
  heatpump_uart_id: "modbus_uart"

uart:
  id: modbus_uart
  tx_pin: GPIO17
  rx_pin: GPIO16
  baud_rate: 19200
  stop_bits: 2
  parity: NONE

packages:
  remote_package:
    url: https://github.com/bp-ouhaha/EcoForest-modbus-registers
    ref: main
    file: heatpump-ecogeo.yaml
```


Out of personal experience:
- avoid using UART0 (GPIO0 and GPIO1) - even with logger/baudrate: 0.
- I've tested it with ESP32-WROOM-32U, ESP8266 (Wemos D1), it works :-)


Other external references:
- https://github.com/graememk/esphome-ecoforest/blob/main/esphome-ecoair.yaml
- https://community.home-assistant.io/t/controlling-ecoforest-heatpumps-via-modbus/309968/49
- [in Dutch] https://gathering.tweakers.net/forum/list_messages/1654510
