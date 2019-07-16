# Irrigation-with-display
Garden irrigation based on Sonoff 4ch, with built-in Nextion display

![Irrigation](/images/irrigation.jpg "Sonoff 4ch with Nextion display")
s
## Getting Started

### Architecture

Using Sonoff 4ch Pro to control four 24V AC valves (Gardena). The control of the solenoid valves is simple - open the valve by connecting 24V to it, it will close automatically when disconnected. I am using the original Gardena 24V AC adapter - one of its wires wire is connected to the (1-4) Sonoff 4ch Pro C (common) connectors. The NO (Normally Open) connectors (1-4) are then connected to the solenoids. The second AC adapter wire is then connected to the second contact of all 4 solenoids.  

The Nextion display is connected via 4 wires: GND, +5V, RX, and TX. So to control the display is extremely easy - just connect these 4 wires to the corresponding ports in the Sonoff 4CH. GND, TX and RX is easy. But you need to find the 5V contact. This can be done in 2 ways - either power the whole Sonoff 4ch Pro by 5V (which is what I have done). Or power it by mains and find a 5V contact on the circuit board.

### Design

The principle is very simple: I used Nextion Editor to create a layout for the display screen. There is a static image background with all graphics that do not change (including text, icons, frames, or color background). On top of the background, I created text objects for all the values that change. When the design is finished, you can upload the layout to the display from the NextionEditor using the serial connection (via USB FTDI board).

The program  is then very simple - it just sets the text value for the individual text objects. For example, for the inside temperature it does this:
```
it.set_component_text_printf("time1","%2d:%02d",id(remaining_time1)/60,id(remaining_time1)%60);
```

ESPHome can read Home Assistant sensors through API, so the ESPHome sensor "rain_today" is linked to Home Assistant sensor sensor.rain_today this way:
```
sensor:
  - platform: homeassistant
    id: rain_today
    entity_id: sensor.rain_today
 ```

It is using the [2.4 inch Nextion display](https://www.banggood.com/Nextion-NX3224T024-2_4-Inch-Man-machine-Interface-HMI-Screen-Kernel-In-English-p-1105052.html?utm_campaign=19381694_november&utm_content=2635&p=@K220219381694201802&cur_warehouse=CN).

### Source files

- **display_weather.yaml** - this is the main file - the actual ESPHome configuration
- **display.HMI** - this is the actual design of the display I use - the file can be edited via the NextionEditor (can be [downloaded](https://nextion.itead.cc/resources/download/nextion-editor/) from ITEAD pages)
- **weather.yaml** - Home Assistant configuration of the weather sensors I use. You can obviously use any sensors or any other weather service you like. Additionally, I have 3 temperature sensors - one of them is Z-Wave temperature sensor (internal). The other sensors are created through ESPHome. I also have luminosity sensor and rain sensor (but I do not show these, they are used in various automations). It is really up to you what information you find useful to show.
- **images (folder)** - I have included the source images for the Nextion display editor. These are not essential for the project, but if you want to create own design, you can start from there.
- **3D model front.stl, 3D model back.stl** - 3D model of the box the display is mounted in (Wemos D1 mini fits in the back).

## Author

* **Václav Chaloupka** - *Initial work* - [bruxy70](https://github.com/bruxy70)
