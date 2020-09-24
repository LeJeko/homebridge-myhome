<p align="center">
  <a href="https://github.com/homebridge/homebridge"><img src="https://raw.githubusercontent.com/homebridge/branding/master/logos/homebridge-color-round-stylized.png" height="140"></a>
</p>

<span align="center">

# homebridge-myhome-own

[![npm](https://img.shields.io/npm/v/homebridge-myhome-own.svg)](https://www.npmjs.com/package/homebridge-enlighten-power) [![npm](https://img.shields.io/npm/dt/hhomebridge-myhome-own.svg)](https://www.npmjs.com/package/homebridge-myhome-own)

</span>

## Why own?

Short answer: OpenWebNet.
This is a fork of a great but unpublished package of [simon77](https://github.com/simont77/homebridge-myhome), forked from [angeloxx](https://github.com/angeloxx/homebridge-myhome) which was created 4 years ago.
Furthermore, on npm the __homebridge-myhome__ package is already occupied by [another inactive user](https://www.npmjs.com/package/homebridge-myhome) for 4 years as well. In addition, links to Github are directed to a "Lockitron" stuff...
So I decided to add a revealing acronym to distinguish them on npm with the ability to stay in a bramch of the original homebridge-myhome github fork.

## Description
Legrand (BTicino) MyHome plugin with Elgato Eve history feature for contact, motion sensor and powermeter.

Legrand [MyHome](http://www.homesystems-legrandgroup.com/BtHomeSystems/home.action) is an Home Automation solution that can manage:
- lighting (standard on/off/dimmed lights)
- thermoregulation 
- curtains, doors
- security systems
- contact and motion sensors

With this plugin, the support of a IP gateway installed in your plant and a configuration of all installed 
systems (MyHome does not support the autodiscovery of the system) you can control it. You need to disable the OpenWebNet password-based authentication from the IP of the device that runs homebridge (ie. Raspberry) or set the auhentication to HMAC; 
HMAC authentication is supported by all recent IP gateways or older one with updated firmware (eg. F454 with v2 firmware).

# Installation
Use Homebridge web UI or install plugin with
```shell
npm install -g homebridge-myhome-own`
```
Add platform within config.json of you homebridge instance:

```json
{
    "platforms": [{
        "platform": "MyHome Gateway",
        "ipaddress": "192.168.1.1",
        "password": "12345",
        "setclock": true,
        "devices": [
                /*Static list of devices*/
            ]
        }], 
    "bridge": {
        "username": "CC:22:3D:E3:CE:31", 
        "name": "MyHome HomeBridge Adapter", 
        "pin": "342-52-220", 
        "port": 51826
    }, 
    "description": "My Fantastic Legrand MyHome System", 
    "accessories": []
}
````
Restart homebridge.

Sample log is:
```log
[1/14/2017, 12:11:29 AM] Plugin /usr/lib/nodejs does not have a package name that begins with 'homebridge-'.
[1/14/2017, 12:11:29 AM] Loaded plugin: homebridge-myhome-tng
[1/14/2017, 12:11:29 AM] Registering platform 'homebridge-myhome.LegrandMyHome'
[1/14/2017, 12:11:29 AM] ---
[1/14/2017, 12:11:29 AM] Loaded config.json with 0 accessories and 1 platforms.
[1/14/2017, 12:11:29 AM] ---
[1/14/2017, 12:11:29 AM] Loading 1 platforms...
[1/14/2017, 12:11:29 AM] Initializing LegrandMyHome platform...
[1/14/2017, 12:11:29 AM] LegrandMyHome: adds accessory
[1/14/2017, 12:11:29 AM] LegrandMyHome::MHRelay create object: 0/1/5
[1/14/2017, 12:11:29 AM] LegrandMyHome: adds accessory
[1/14/2017, 12:11:29 AM] LegrandMyHome::MHRelay create object: 0/1/1
[1/14/2017, 12:11:29 AM] LegrandMyHome: adds accessory
[1/14/2017, 12:11:29 AM] LegrandMyHome::MHRelay create object: 0/1/4
[1/14/2017, 12:11:29 AM] LegrandMyHome: adds accessory
[1/14/2017, 12:11:29 AM] LegrandMyHome::MHRelay create object: 0/1/2
[1/14/2017, 12:11:29 AM] LegrandMyHome: adds accessory
[1/14/2017, 12:11:29 AM] LegrandMyHome::MHThermostat create object: 21
[1/14/2017, 12:11:29 AM] LegrandMyHome: adds accessory
[1/14/2017, 12:11:29 AM] LegrandMyHome for MyHome Gateway at 192.168.157.213:20000
[1/14/2017, 12:11:29 AM] Initializing platform accessory 'Bathroom Light'...
[1/14/2017, 12:11:29 AM] Initializing platform accessory 'Night hallway Light'...
[1/14/2017, 12:11:29 AM] Initializing platform accessory 'Office'...
[1/14/2017, 12:11:29 AM] Initializing platform accessory 'Master bedroom Central'...
[1/14/2017, 12:11:29 AM] Initializing platform accessory 'Living Room Thermostat'...
[1/14/2017, 12:11:29 AM] Loading 0 accessories...
Scan this code with your HomeKit App on your iOS device to pair with Homebridge:

    ┌────────────┐
    │ 342-52-220 │
    └────────────┘

[1/14/2017, 12:11:29 AM] Homebridge is running on port 51827.
```

## Configuration

Only the platforms section of the config.json file should be edited, the pair code and the port in bridge section can be ignored in most of the cases. 
The first part of the config file contains details about the MyHome Gateway used to interface the IP network with the plant:
```json
"platforms": [{
    "platform": "LegrandMyHome",
    "ipaddress": "192.168.1.35",
    "port": 20000,
    "ownpassword": "12345",
    "setclock": true,
    "devices": [{
```

You need to change:
- ipaddress: put the IP address or name of the MyHome Gateway (eg. F454 or MH201)
- port: should be 20000 and keep this value
- ownpassword: the OpenWebNet password, default is 12345 but everyone will suggest to you to change it with another password, but you will keep the default one, I know...
- setclock: set to true if you want your homebridge server to set the time of your gateway every hour
- devices: list of installed devices

The devices section contains the list of devices that will be managed. All devices contains three standard properties:

- accessory: the technical name of the device, should be one of the names listed in this document
- name: mnemonic name, will be displayed by iOS HomeKit application
- address: the MyHome address, usually in **B/A/PL** format for lights and curtaints or single/double digits for other devices. **B** stands for BUS (usually 0), **A** and **PL** is the name of the addressing object used by BTicino and stands for Ambient and Light Point (Punto Luce in the original italian version)

## Supported devices

* MHRelay: Standard (Lighting) Relay (eg. F411), address is B/A/PL (eg. 0/1/10). This device supports the definition of a custom frame for on and off command, so you can specify frame\_on and/or frame\_off:

    ```json
    {
    "accessory": "MHRelay",
    "name": "Bathroom Light",
    "address": "0/1/5",
    "frame_on": "*1*1*14##",
    "frame_off": "*1*0*14##"
    }
    ```
    to use a Group, CEN, CEN+ or other command to turn on and off that load, using the _address_ load for the status monitor. Remember that HomeKit will think that the load has changed the status after the command even if is not true.

* MHDimmer: Lighting Dimmer (eg. F427, F413N), address is B/A/PL (eg. 0/1/10)
* MHThermostat: Standard Thermostat controlled by a 99-Zones Central Station (code 3550), address is the Zone Identifier (1-99)
* MHExternalThermometer: External Probe controlled by a 99-Zones Central Station (code 3550), address is the Zone Identifier (1-9)
* MHOutlet: Standard (not-Lighting) Relay, address is B/A/PL (eg. 0/1/10). See MHRelay for custom frame support
* MHBlind: Standard Automation Relay (eg. F411, I need to check the F401), address is B/A/PL (eg. 0/1/10)
  * this device defines another property called "time" that defines the configured "stop time" in seconds; using this property the driver can evaluate the current position of the blind
* MHBlindAdvanced: Advanced version of standard Blind (eg. F401 that manages internally the current position), address is B/A/PL (eg. 0/1/10)
* MHContactSensor: Dry Contact sensor (eg. 3477 or some burgalarm sensors), address range is 1-201. Supported types are "motion" and "contact". Elgato Eve history feature is supported.
* MHPowerMeter: Only supported with F421 Load Control Central, use "refresh" to set the update interval. Elgato Eve history feature is supported.
* MHAlarm: tested on central 3486. Zones for Away, Night and At Home activation are currently hard coded in plugin code. Alarm activation/deactivation from Homekit is not implemented for security reasons, so only monitor of the current status is supported
* MHTimedRelay: to issue temporized command to relays. Default duration set in "duration"
* MHControlledLoad: to control status of old generation Load Control outlets
* MHAux: to deliver AUX events to Homekit. Supported type are "leak", "gas" and contact".
* MHIrrigation: modified MHTimedRelay using the new Homekit irrigation service

See sample-config.json for the additional parameters of each accessory.

## Tested devices
- F453, F454v1, MH200N and MH201 as IP Gateway
- F411/2 as MHRelay, MHOutlet and MHCurtain
- F401 as MHBlindAdvanced
- F416U1 as MHDimmer
- 3455 as MHExternalThermometer
- 3477 as MHContactSensor

## Known Bug List

- Groups are not managed

## Credits
https://github.com/angeloxx/homebridge-myhome : First version
https://github.com/simont77/homebridge-myhome : Added accessories and Elgato Eve history

# Disclaimer

I'm furnishing this software "as is". I do not provide any warranty of the item whatsoever, whether express, implied, or statutory, including, but not limited to, any warranty of merchantability or fitness for a particular purpose or any warranty that the contents of the item will be error-free.
The development of this module is not supported by Legrand, BTicino or Apple. These vendors and me are not responsible for direct, indirect, incidental or consequential damages resulting from any defect, error or failure to perform.  
