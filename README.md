# ESPHome-Support-on-Seeed-Studio-XIAO-ESP32C3

## Introduction

This wiki will walkthrough step-by-step on how to connect [Seeed Studio XIAO ESP32C3](https://www.seeedstudio.com/Seeed-XIAO-ESP32C3-p-5431.html) with ESPHome running on Home Assistant and send the sensor data/ control devices after connecting Grove modules to XIAO ESP32C3. So, let's get started!

## What is ESPHome?

[ESPHome](https://esphome.io/) is a tool which aims to make managing your ESP boards as simple as possible. It reads in a YAML configuration file and creates custom firmware which it installs on your ESP device. Devices or sensors added in ESPHome’s configuration will automatically show up in Home Assistant’s UI.

## Install Home Assistant

Make sure you already have Home Assistant up and running. You can follow [this wiki](https://wiki.seeedstudio.com/ODYSSEY-X86-Home-Assistant) for a step-by-step guide on installing Home Assistant on an ODYSSEY-X86 SBC or this [link](https://www.mbreviews.com/how-to-home-assistant-seeed-mini-router/) for a detailed instruction use Home Assistant with a Seeed Mini Router.

## Install ESPHome on Home Assistant

ESPHome is available as a **Home Assistant Add-On** and can simply be installed via the add-on store.

- **Step 1.** To quickly setup ESPHome on Home Asssistant, click the below button

- **Step 2.** Once you see the following pop-up, click **OPEN LINK**

- **Step 3.** Click **INSTALL**

- **Step 4.** Enable all the options and click **START**

- **Step 5.** Click **OPEN WEB UI** or **ESPHOME from the side-panel**

You will see the following window if ESPHome is successfully loaded

## Add Seeed Studio XIAO ESP32C3 to ESPHome

- **Step 1.** Click **+ NEW DEVICE**

- **Step 2.** Click CONTINUE

- **Step 3.** Enter a **Name** for the device and enter WiFi credentials such as **Network name** and **Password**. Then click **NEXT**

- **Step 4.** Select **ESP32-C3** and click

- **Step 5.** Click **SKIP** because we will configure this board manually

- **Step 6.** Click **EDIT** under the newly created board

- **Step 7.** This will open a **YAML** file and this file will be used to set all the board configurations. Edit the content under **esp32** as follows




**Note:** Here we are using the latest version of [Arduino core](https://github.com/espressif/arduino-esp32/releases) for ESP32 and [ESP32 support for PlatformIO](https://github.com/platformio/platform-espressif32/releases)

- **Step 8.** Click **SAVE** and then click **INSTALL**

- **Step 9.** Connect one end of a USB Type-C cable to Seeed Studio XIAO ESP32C3 and the other end to one of the USB ports on the reRouter CM4 1432

``` 
esphome:
  name: xiao-esp32c3
  platformio_options:
   board_build.flash_mode: dio

esp32:
  board: seeed_xiao_esp32c3
  variant: esp32c3
  framework:
    type: arduino
    platform_version: 5.4.0

# Enable logging
logger:
 hardware_uart: UART0

# Enable Home Assistant API
api:

ota:

wifi:
  ssid: "WiFi_SSID"
  password: "Your Password"

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "Xiao-Esp32C3 Fallback Hotspot"
    password: "Your Password"
```

- **Step 10.** Click **Plug into the computer running ESPHome Dashboard**


- **Step 11.** Select the connected port. It is likely to be ```/dev/ttyACM1 because /dev/ttyACM0``` is connected to the reRouter CM4 1432

Now it will download all the necessary board packages and flash the ESPHome firmware into the XIAO ESP32C3. If the flashing is successful, you will see the following output

- **Step 12.** The above window displays the real-time logs from the connected board. Close it by clicking **STOP**

- **Step 13.** If you see the board status as **ONLINE**, that means the board is successful connected to WiFi

Now you can disconnect the XIAO ESP32C3 from the reRouter CM4 1432 and just power it via a USB cable. This is because from now on, if you want to flash firmware to the XIAO ESP32C3, you can simply do it OTA without connecting to the X86 board via a USB cable.

- **Step 14.** Click the **three dots** and click **Install**

- **Step 15.** Select **Wirelessly** and it will push the changes to the board wirelessly

- **Step 16.** Go to **Settings** and select **Devices & Services**

- **Step 17.** You will see **ESPHome** as a discovered integration. Click **CONFIGURE**

- **Step 18.** Click **SUBMIT**

Here it will ask for the encryption key you have in your configuration for xiao-esp32c3

- **Step 19.** Go back to **xiao-esp32c3.yaml**, copy the encryption key, paste inside the above dialog box and click **SUBMIT**

- **Step 20.** Click **FINISH**

# Grove Modules with ESPHome and Home Assistant

Now we will connect Grove modules to Seeed Studio XIAO ESP32C3 so that we can display sensor data or control the devices using Home Assistant!

## Connect Grove Modules to XIAO ESP32C3

In order to use Grove modules with Seeed Studio XIAO ESP32C3, we will use a [Seeed Studio Expansion Base for XIAO](https://www.seeedstudio.com/Seeeduino-XIAO-Expansion-board-p-4746.html) and connect XIAO ESP32C3 on it.

After that, the Grove connectors on the board can be used to connect Grove modules

## Pin Definitions

You need to follow the table below to use the appropriate internal pin numbers when connecting the Grove modules to the Grove connectors on Grove Shield for Seeed Studio XIAO.

| First Header  | Second Header |
| ------------- | ------------- |
| 2  | 	D0  |
| 3  | 	D1  |
| 4  | 	D2  |
| 5  | 	D3  |
| 6  | 	D4  |
| 7  | 	D5  |
| 21 | 	D6  |
| 20 | 	D7  |
| 8  | 	D8  |
| 9  | 	D9  |
| 10 |  D10 |
| 6  |  SDA |
| 7  |  SCL |


For example, if you want to connect a Grove module to D0 port, you need to define the pin on ESPHome as 2

## Grove Compatibility List with ESPHome

Currently the following Grove modules are supported by ESPHome

Check [here](https://esphome.io/components/sensor/index.html#see-also)

Now we will select 6 Grove modules from the above table and explain how they can be connected with ESPHome and Home Assistant.

## Grove - Temperature and Humidity Sensor (BME680)

### Setup Configuration

- **Step 1.** Connect Grove - [Temperature, Humidity, Pressure and Gas Sensor (BME680)](https://www.seeedstudio.com/Grove-Temperature-Humidity-Pressure-and-Gas-Sensor-for-Arduino-BME680.html) to one of the I2C connectors on the Seeed Studio Expansion Base for XIAO

- **Step 2.** Inside the **xiao-esp32c3.yaml** file that we created before, change the file and push it OTA to XIAO ESP32C3

```
esphome:
  name: xiao-esp32c3
  platformio_options:
   board_build.flash_mode: dio

esp32:
  board: seeed_xiao_esp32c3
  variant: esp32c3
  framework:
    type: arduino
    platform_version: 5.4.0

# Enable logging
logger:
 hardware_uart: UART0

# Enable Home Assistant API
api:

ota:

wifi:
  ssid: "UMASS fried chicken"
  password: "Zacharyloveschicken"

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "Xiao-Esp32C3 Fallback Hotspot"
    password: "MoLTqZUvHwWI"

captive_portal:

i2c:
  sda: GPIO6
  scl: GPIO7

sensor:
  - platform: bme680
    temperature:
      name: "BME680 Temperature"
      oversampling: 16x
    pressure:
      name: "BME680 Pressure"
    humidity:
      name: "BME680 Humidity"
    gas_resistance:
      name: "BME680 Gas Resistance"
    address: 0x76
    update_interval: 60s
```

You can learn more about the [BME680 component](https://esphome.io/components/sensor/bme680) here. It allows you to use BME280, BME680, BMP085, BMP280, AHT10, AHT20 and AHT21 based sensors. Here we add the I²C Bus component because AHT20 communicates using I2C protocol.


### Visualize on Dashboard

- **Step 1.** On the Overview page of Home Assistant, click the 3 dots and click **Edit Dashboard**

- **Step 2.** Click **+ ADD CARD**

- **Step 3.** Select **By ENTITY**, type **temperature** and select the **check box** next to **Temperature**

- **Step 4.** Repeat the same for **Humidity**, **Gas Resitance** and **Pressure**

- **Step 5.** Click **CONTINUE**

- **Step 6.** Click **ADD TO DASHBOARD**

Now your Home Assistant dashboard will look like below

- **Step 7.** You can also visualize sensor data as gauges. Click **Gauge** under **BY CARD**

- **Step 8.** Select **Temperature** from the drop-down menu

- **Step 9.** Click **SAVE**

- **Step 10.** Repeat the same for **Humidity**, **Gas Resitance** and **Pressure**

- Now your dashboard will look like below
























































