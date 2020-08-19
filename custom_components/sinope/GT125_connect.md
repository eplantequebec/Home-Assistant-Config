# Home Assistant Sinopé Custom Component with direct connection to your GT125

## Supported Devices

Same as Neviweb custom_component.

## Prerequisite

- CRC8-0.0.5 module from PyPI should be installed by HA on first run automatically. In hassbian look in directory /srv/homeassistant/lib/python3.5/site-packages to find out if it is there. (crc8-0.0.5.dist-info)
- For the devices setup you still need to install crc8 to directory /usr/local/lib/python3.5/dist-packages with the command 
  'sudo pip3 install crc8'
- For python3.7, you will need to make sure that crc8 lib is installed in /srv/homeassistant/lib/python3.7/site-packages/.
  crc8.py and dir crc8-0.0.5.dist-info/*
- For devices setup in python3.7 you'll need to install crc8 lib with the command:
  sudo python3.7 -m pip install crc8
  
## Installation
The config folder is different depending if you are on Hassbian or Hass.io:
 - Hassbian: /home/homeassistant/.homeassistant/
 - Hass.io: /config/
### Via HACS:
- install hacs
- install sinope-GT125 via hacs store. It will be installed under «config»/custom_components/sinope

### Manually:
Create a directory named sinope under «config»/custom_components in your HA setup.

Copy the files in the sinope directory to your «config»/custom_components/sinope directory.

Once ready you will need to add entry for sinope in your configuration.yaml like this:

```yaml
# Example configuration.yaml entry
sinope:
  server: '<Ip adress of your GT125>'
  id: '<ID written on the back of your GT125>'
  api_key: '<Api_key received on first manual connection with the GT125>'
  dk_key: '<your Dark sky key>'
  my_city: '<the nearest city>' #needed to get sunrise and sunset hours for your location
  scan_interval: 120 #you can go down to 60 if you want depending on how many devices you have to update. default set to 180
```
## First run
To setup this custom_component, login to your Rpi via SSH and cd to the directory where you have copied the file. You don't need to edit the file device.py anymore but you will need to have the following data handy:
- IP adress of the GT125,
- GT125 device ID, written on the back of the device,
- Port number to connect to the GT125. should be 4550 (default),
- the required library crc8.py should be installed automatically. if not use this command: sudo pip3 install crc8. For python3.7,  use command: sudo python3.7 -m pip install crc8

Execute the command: 'sudo python3 device.py'. (for Python3.7, the command is 'sudo python3.7 device.py') This is required to get the Api_Key and the deviceID for each Sinopé devices connected to your GT125. For Hass.io you already run as root so you don't need sudo. On first run, device.py will ask you to enter the IP address of the GT125, the API ID written on the back of your GT125 and the port number (default 4550). It will send a ping request to the GT125 and will ask you to push de "WEB" button on the GT125. 
This will give you the Api Key. Then, device.py will create file 'config/.storage/sinope_devices.json' and write two line in it to store the above data.

I've put lots of comment in the code so I think you will understand.

Main difference with Neviweb is that with the GT125 we don't have command to request all data and info 
from one device at once. We need to issue on data read request for each info or data we want. 
ex:
- open a connection
- login to the GT125
- send data read request for room temperature
- send data read request for setpoint temperature
- send data read request for mode (manual, auto, off, away)
- send data read request for heat level
- etc
- close connection and start over for next device.

This is the same for data write request but in that case we normally send one data like changing temperature or mode 
to one device. One exception is when we sent request to change mode to auto. We need to send correct time prior to send write request for auto mode.

For the data report request it is possible to send data to all device at once by using a specific deviceID = FFFFFFFF. 
It is used to send time, date, sunset and sunrise hour, outside temperature, set all device to away mode, etc, broadcasted to all device.

## Devices discovery
Look like the GT125 use a different deviceID then Neviweb. Once you have your Api_key written in sinope_devices.json, you will need to run it to request deviceID for each devices on your network one by one. Device.py will ask for device name, type and connected watt load (for light and switch) for each device discovered. Program will loop to discover each device one by one. When finished, type "q" to leave the program. for each device discovery,the program will wait for you to push on both button of your device to receive the deviceID of that device. All devices id and data will be written in file sinope_devices.json and that file will be moved to config/.storage/sinope_devices.json to insure that any new update won't overwrite it. Once you have all your devices, edit sinope_devices.json and change the name, type and wattage (for light devices), if needed, for each devices. For device type you can get them at the top of each file climate.py, light.py and switch.py. You can also get them by typing "h" when prompted for type in device.py. Light connected watt load is not measured by the light devices but instead written in Neviweb on setup of light devices. We need to write it to devices.json (kind of Neviweb equivalent) to finish the devices setup.

```yaml
["IP", "Api Key", "Api ID", "PORT"] <- do not erase this line
["id", "name", "type", "watt"] <- do not erase or edit this line
["00470100", " ", " ", " "] <- once discovered by device.py, add devices info between the " "
["2e320100", "Office heating", "10", " "] <- thermostat ex.
["5a2c0100", "Office light", "102", "60"] <- light ex.
["6a560100", "Outside timer", "120", " "] <- power switch ex.
["00470100", "Dimmer TV Room", "112", "110"] <- Dimmer ex.
```

Each time you will add a new device to your GT125 you will need to do that setup

## Pypi module
As requested by HA, all API specific code has to be part of a third party library hosted on PyPi. I will soon add modules to Pypi that will include all specific code for direct connection to GT125 or to neviweb. 

- PI_Sinope, this module is for Sinope component for the GT125 direct connection
- PI_Neviweb, this module is for Neviweb component to work with Neviweb.

I will first upload to testPyPi and once stable, I'll switch them to PyPi. Then these module will be loaded automatically by HA at startup.

## TO DO
- Leave socket open to listen for events from devices state changes and answers from our data request. For now I open, send request, get result then close socket.
- Detect events from light dimer and switch so we can receive state changes from the GT125 without polling the devices (faster).
- Send time, date, sunset, sunrise once a day to each devices. Need to find out how to do that once a day at specific time.
- Send outside temperature to thermostat once per hour to have it displayed on the second display line.
- Improve logging and debug.

Test it and let me know. Any help is welcome. There is still work to do to use it in HA.
