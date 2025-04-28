# meta-samsung-smartTV
Driver to control Samsung SmartTV from 2016 upwards using [meta v2](https://github.com/jac459/meta) and the original NEEO remote.

Feel free to support me with some [coffee](https://www.paypal.me/MarkusMas721).

### Device Compatibility
The driver in this repository uses a WebSocket connection and some http post requests to communicate with the TV. The compatibility of your TV is automatically checked during step 12 of the instructions below.

Further requirement: The IP addresses of your TV must be fixed!

## How to Install the Samsung SmartTV Driver
### a) Installation via meta-core Driver
0. Start meta-core on the NEEO remote or in the web-UI
1. Open the directory "Settings"
2. Browse to the submenu "Library"
3. From the library select the driver "Samsung smartTV (2016+)"
4. Click "Download this Driver"
5. Click "Activate Driver"
6. Restart meta using meta-core. Go to: "Global Settings" > "Restart meta" > "Restart meta now"!
7. Close/ Power off meta-core.
8. Open the NEEO App or the web-UI and go to: "Devices" > "Add a Device"
9. Search for "meta samsung"
10. Select the driver "Samsung .meta2 smartTV (2016)".
11. Click "NEXT"
12. When asked for the `<security code>`, enter the IP adress of your TV (For example: `192.168.178.1`) and click "Verify". A communication and compatability check is performed in the background. If successful, your device will be added to the list of compatible devices displayed on the next page.
13. It might happen that you are asked to refresh the next page.
14. Select the device you want to add to your NEEO and click "NEXT".
15. Follow the remaining instructions in the app to install your device to your NEEO system.

### b) Manual Installation for advanced Users
1. Download the driver file (for example: "samsung-smartTV.json") to the Subfolder "active" of your meta Installation.
2. Restart meta
3. Follow the Instructions above starting at Step 8.

## How to use the Driver on the NEEO Remote
### Device
For each TV added, one "device" will be added in the devices section of your NEEO system. The device device name is automatically retrieved from the settings of your TV during the installation (Step 12). You can rename your TV in the NEEO app at any time. For that open the NEEO App or the web-UI and go to "Devices" and click the tree dots next to the device.

### Using Recipes/ Driver Features
Due to being added as device type "TV" a recipe is also automatically created in the respective room. If you need, you can disable the automatically generated recipe. For that open the NEEO App or the web-UI and go to "Recipes" and flick the switch.

Using the actual features of the TV driver is done by adding shortcuts, directories or switches to the slide of a recipe.

#### Buttons "POWER ON" and "POWER OFF"
The buttons "POWER ON" and "POWER OFF" are turning the TV on and off. They also initialise and close the communication to the TV. So if you want to use shortcuts of the TV in custom recipes, you must add the "POWER ON" and "POWER OFF" for the TV to the (custom) recipe.

Note: The buttons "POWER ON" and "POWER OFF" are automatically added to the automatically generated recipe of the TV.

#### Buttons/ Shortcuts
- CURSOR UP/ DOWN/ LEFT/ RIGTH/ ENTER: Use the buttons to control the cursor (also mapped to the remote buttons)
- BACK: go back in a TV menu (also mapped to the remote button)
- MENU: opens the SmartHub (also mapped to the remote button short press MENU)
- GUIDE: opens the TV guide (also mapped to the remote button long press MENU)
- INFO: shows info on the TV screen (like on the original remote).
- VOLUME UP/ DOWN: Volume control (also mapped to the remote buttons)
- CHANNEL UP/ DOWN: Channel control (also mapped to the remote buttons)
- CHANNEL LIST/ PRE/ FAV: extended channel zapping features.
- PLAY/ PAUSE/ STOP/ FORWARD/ REVERSE/ RECORD/ LIVE: Playback control
- DIGIT 0-9: Enter digits to change channel
- FUNCTION RED/ GREEN/ YELLOW/ BLUE: function buttons
- SOURCE, INPUT TV/ DTV/ AV1-3/ HDMI 1-4: Input and Source control
- MODE PANORAMA/ DYNAMIC/ STANDARD/ MOVIE/ GAME/ CUSTOM: change picture settings
- _APP-Shortcuts_: A number of predefined app shortcuts is included in the driver.

More buttons or shortcuts can be added at a later stage if there is any need.

A great resource for app id's is [this page](https://tavicu.github.io/homebridge-samsung-tizen/extra/applications.html#list-with-ids).
And for buttons i found [this page](https://github.com/jaruba/ha-samsungtv-tizen/blob/master/Key_codes.md) being very helpfull.

#### Lables
- _none_

#### Switches
- MUTE: Toggle the mute status

#### Directory "Apps"
Shows a list of the available apps. Selecting an app will launch it. The list can be modified using the directory "settings".

This feature is only available on the older TVs without `"TokenAuthSupport": "true"` [Link](#token-authentication-dor-developers).

#### Directory "Settings"
Change the Settings of this Driver. The Settings are stored (persisted) per Device. The following Options are available:

###### Apps
Customize the app list (seperate directory) by hiding individual apps (for example bloat ware), rearrange the order of the apps or enable/ disable icons in the list (icon server).

Please note, that a local simple http server is created on your device running meta (at http://localhost:2016) for making the icons available on the remote. Disable the icon server in the settings if you have any issue with this.

###### Remote Button: Menu  
Customize the short and long press of the physical "MENU" button on the remote. Change wether short press or long press is assigned to open open the guide or the SmartHub.

### Token-Authentication (For Developers)
Using a browser you can have a look at `http://<tv-ip>:8001/api/v2/`.\
This request is performed during the setup of the TV (Step 12).

Until version 4 of this driver, only devices were supportet that did not contain `"TokenAuthSupport": "true"` in the response to the aforementioned request. With version 5, essential parts of the driver were rewritten to enable the authentication with token. This essentially involves the use of secure websocket (wss://) for newer devices, while websocket (ws://) is still used for older devices.

I would like to give credit to two repositories that have been very helpful in providing a resource with a working wss implementation for the development of version 5:
- https://github.com/jaruba/ha-samsungtv-tizen/
- https://github.com/tavicu/homebridge-samsung-tizen

#### Versions
##### Version 1
- First release
  
##### Version 2
- Bug fixing in registration process and first start/ initialization of driver

##### Version 3
- Revert back to the previous solution for the registration process as soluton of verison 2 caused new bugs.
- (currently only allows one device per driver)

##### Version 4
- Bug fixing in registration process and first start/ initialization of driver

##### Version 5
- added support for newer Samsung smartTVs (TVs with "TokenAuthSupport:true" and using Secure WebSocket wss://)
- added shortcuts for apps "Smart STB" and "Philips Hue Sync"
