# Home Assistant Solar Info Panel
Display House Solar Information on a TFT or OLED display

## Purpose
The purpose of the **Home Assistant Solar Info Panel** (SIP) is to display in regular intervals, the house power data on a TFT or OLED display.

* **Solar** - production (W) [PowerFromGrid]
* **House** - power usage (W) [PowerToHouse]
* **Grid** - supply or load [PowerTo/PowerFromGrid]
* **Battery** - charging level (%) [BatteryChargeState] & state charge or discharge (W) [PowerToBattery/PowerFromBattery]
* **Date** & **Time** shown on the top of each page
__(unit)[Home Assistant Entity]__

The initial solution uses the device Sunton ESP32 2.8" TFT w320xh240 (Cheap Yellow Display CYD).
The CYD has 3 pages, which can be selected via touch:

* Main - Solar Info Panel with the 4 textboxes Solar Production [SOLAR], House Usage [HOUSE], Grid Import & Export [GRID], Battery Charging State & Usage [BATTERY].
* Battery - Charging state & usage (positive battery is charged, negative battery is discharged).
* Settings - Set display brightness (0-100) or level (0-255) of the CYD backlight RGB LED white.

In addition to the CYD, an ESP32 with OLED display is used to mainly display the battery charge state direct at the battery.
This display has 4 pages Solar > House > Grid > Battery selected via red push-button.
The hardware is DIY MORE ESP32 0.96" OLED, w128xh64. The top area w128xh16 has yellow text (default) and the area below blue text.

The solution
* can handle many clients and the data is immediate published.
* is custom made and developed for personal use only.
* source is not shared but can provide more detailed solution info on request.

## Devices
![Image](https://github.com/user-attachments/assets/3ef22ba2-3378-48da-8b4a-3801a4f8d3b5)

## Concept
There are 3 components interacting with each other being Home Assistant (HA), Node-RED (NR) and B4R.
* B4R to connect to the WiFi network and sent HTTP GET request to the HA NR RESTful server.
* NR RESTful server to get the relevant HA entities data, create HTTP response content CSV string and sent the HTTP response to B4R.
* B4R to parse the HTTP response CSV string and updates the display. The display is controlled using the B4R library [rLovyanGFXEx][https://www.b4x.com/android/forum/threads/rlovyangfxex-esp32.166606).

### Data Flow
![Image](https://github.com/user-attachments/assets/fedc6629-1dcd-4c60-862e-e7030e4d203a)

### HTTP Response Format
CSV string with HA entities data:
powerfromsolar,powerfromgrid,powertogrid,powertohouse,powertobattery,powerfrombattery,batterychargestate,powerdatestamp,powertimestamp
```
774,0,506,268,0,0,100,20250422,1207
```

## Node-RED Flow
HTTP RESTful server listening to GET requests http://ip-ha:1880/endpoint/solarinfo?data=all.
Receiving the request, the current state of the HA entities are obtained and the date/time stamp is set.
HTTP response is a CSV string: 774,0,506,268,0,0,100,20250422,1207
![Image](https://github.com/user-attachments/assets/8ac26211-7aad-463e-9fb5-4df9be426e6d)
Note: There are more flows to built the power data.

## Hardware
* Sunton ESP32 2.8" TFT, w320xh240, touch, driver ILI9341.
* DIY MORE ESP32 0.96" OLED, w128xh64, no touch, driver SSD1306.

## Software
* [B4R](https://www.b4x.com/b4r.html) 4.00 (64 bit) for the ESP32 with external library [rLovyanGFXEx](https://www.b4x.com/android/forum/threads/rlovyangfxex-esp32.166606) 1.0 (requires the ESP32 library 3.1.3 and LovyanGFX 1.2.0).
* [Home Assistant](https://www.home-assistant.io) 2025.x with Node-RED Add-on 19.x.

## Source
The source is in archive solarinfopanel-restful-NNN.zip.

## License
This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version. This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS for A PARTICULAR PURPOSE. See the GNU General Public License for more details. You should have received a copy of the GNU General Public License along with the application. If not, see [GNU Licenses](https://www.gnu.org/licenses/).


