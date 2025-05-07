# Home Assistant Solar Info Panel
Display House Solar Information on a TFT or OLED display

## Purpose[/B]
The purpose of the **Home Assistant Solar Info Panel** (SIP) is to display in regular intervals, the house power data on a TFT or OLED display.
[LIST]
[*][B]Solar - [/B]production (W) [PowerFromGrid]
[*][B]House [/B]- power usage (W) [PowerToHouse]
[*][B]Grid [/B]- supply or load [PowerTo/PowerFromGrid]
[*][B]Battery - [/B]charging level (%) [BatteryChargeState] & state charge or discharge (W) [PowerToBattery/PowerFromBattery]
[*][B]Date [/B]& [B]Time [/B]shown on the top of each page
[I][SIZE=1](unit)[Home Assistant Entity][/SIZE][/I]
[/LIST]
The initial solution uses the device Sunton ESP32 2.8" TFT w320xh240 (Cheap Yellow Display CYD).
The CYD has 3 pages, which can be selected via touch:
[LIST]
[*]Main - Solar Info Panel with the 4 textboxes Solar Production [SOLAR], House Usage [HOUSE], Grid Import & Export [GRID], Battery Charging State & Usage [BATTERY].
[*]Battery - Charging state & usage (positive battery is charged, negative battery is discharged).
[*]Settings - Set display brightness (0-100) or level (0-255) of the CYD backlight RGB LED white.
[/LIST]
In addition to the CYD, an ESP32 with OLED display is used to mainly display the battery charge state direct at the battery.
This display has 4 pages Solar > House > Grid > Battery selected via red push-button.
The hardware is DIY MORE ESP32 0.96" OLED, w128xh64. The top area w128xh16 has yellow text (default) and the area below blue text.

The solution
[LIST]
[*]can handle many clients and the data is immediate published.
[*]is custom made and developed for personal use only.
[*]source is not shared but can provide more detailed solution info on request.
[/LIST]
[B]Devices[/B]
[ATTACH type="full" alt="1745393235802.png"]163577[/ATTACH]

[B]Concept[/B]
There are 3 components interacting with each other being Home Assistant (HA), Node-RED (NR) and B4R.
[LIST]
[*]B4R to connect to the WiFi network and sent HTTP GET request to the HA NR RESTful server.
[*]NR RESTful server to get the relevant HA entities data, create HTTP response content CSV string and sent the HTTP response to B4R.
[*]B4R to parse the HTTP response CSV string and updates the display. The display is controlled using the B4R library URL [URL='https://www.b4x.com/android/forum/threads/rlovyangfxex-esp32.166606/']rLovyanGFXEx[/URL].
[/LIST]
[U]HTTP Response Format[/U]
CSV string with HA entities data:
powerfromsolar,powerfromgrid,powertogrid,powertohouse,powertobattery,powerfrombattery,batterychargestate,powerdatestamp,powertimestamp
[CODE]
774,0,506,268,0,0,100,20250422,1207
[/CODE]

[B]Hardware[/B]
[LIST]
[*]Sunton ESP32 2.8" TFT, w320xh240, touch, driver ILI9341.
[*]DIY MORE ESP32 0.96" OLED, w128xh64, no touch, driver SSD1306.
[/LIST]
[B]Software[/B]
[LIST]
[*][URL='https://www.b4x.com/b4r.html']B4R[/URL] 4.00 (64 bit) for the ESP32 with external library [URL='https://www.b4x.com/android/forum/threads/rlovyangfxex-esp32.166606/']rLovyanGFXEx[/URL] 1.0 (requires the ESP32 library 3.1.3 and LovyanGFX 1.2.0).
[*][URL='https://www.home-assistant.io']Home Assistant[/URL] 2025.x with [URL='https://nodered.org']Node-RED[/URL] Add-on 19.x.
[/LIST]
[B]To-Do[/B]
[LIST]
[*]Built proper cases for the devices (3-D printing).
[*]HTTP response byte array instead CSV string.
[/LIST]
[B]Node-RED Flow[/B]
HTTP RESTful server listening to GET requests http://ip-ha:1880/endpoint/solarinfo?data=all.
Receiving the request, the current state of the HA entities are obtained and the date/time stamp is set.
HTTP response is a CSV string: 774,0,506,268,0,0,100,20250422,1207
[ATTACH type="full" alt="1745321965940.png"]163568[/ATTACH]
Note: There are more flows to built the power data.
