# MyHomePlugin
Legrand (BTicino) MyHome plugin for homebridge: https://github.com/nfarina/homebridge

Legrand MyHome (http://www.homesystems-legrandgroup.com/BtHomeSystems/home.action) is an Home Automation solution that can manage:
- lighting (standard on/off/dimmed lights)
- thermoregulation 
- curtains, doors
- security systems

With this plugin, the support of a IP gateway installed in your plant and a configuration of all installed 
systems (MyHome does not support the autodiscovery of the system) you can control it. You need to disable the OpenWebNet password-based
authentication from the IP of the device that runs homebridge (ie. Raspberry) or set the auhentication to HMAC; this software does not 
support the proprietary "OWN" encryption schema even if was reverse-engineered and published on a wiki years ago. HMAC authentication is
supported by all recent IP gateways or older one with updated firmware (eg. F454 with v2 firmware).

# Installation (TBD)
Install plugin with npm install -g homebridge-myhome
Add platform within config.json of you homebridge instance:

    {
        "platforms": [{
            "platform": "MyHome Gateway",
            "ipaddress": "192.168.1.1",
            "password": "12345",
            "discovery": false,
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

Restart homebridge
Enjoy!

# Installation (TBD)

Run as root:

    npm -g install homebridge-myhome-tng
   
Create your config.json file starting from the template (sample-config.json) and then:

    homebridge -U .

Sample log is:

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


## Configuration

Only the platforms section of the config.json file should be edited, the pair code and the port in bridge section can be ignored in most of the cases. A little sample:

    {
        "platforms": [{
            "platform": "LegrandMyHome",
            "ipaddress": "192.168.157.207",
            "port": 20000,
            "ownpassword": "12345",
            "discovery": false,
            "devices": [{
                "accessory": "MHRelay",
                "name": "Bathroom Light",
                "address": "0/1/5"
                },{
                "accessory": "MHRelay",
                "name": "Night hallway Light",
                "address": "0/1/1"
                },{
                "accessory": "MHRelay",
                "name": "Office",
                "address": "0/1/4"
                },{
                "accessory": "MHDimmer",
                "name": "Master bedroom Central",
                "address": "0/1/2"
                },{
                "accessory": "MHThermostat",
                "name": "Living Room Thermostat",
                "address": "21"
                },{
                "accessory": "MHThermostatExternal",
                "name": "External Thermo Sensor",
                "address": "1"
                }]
            }],
        "bridge": {
            "username": "CC:22:3D:E3:CE:31", 
            "name": "MyHome HomeBridge Adapter", 
            "pin": "342-52-220", 
            "port": 51827
        }, 
        "description": "My MyHome Home System",
        "accessories": [
        ]
    }

The first part of the config file contains details about the MyHome Gateway used to interface the IP network with the plant:

        "platforms": [{
            "platform": "LegrandMyHome",
            "ipaddress": "192.168.157.207",
            "port": 20000,
            "ownpassword": "12345",
            "discovery": false,
            "devices": [{

You need to change:
- ipaddress: put the IP address or name of the MyHome Gateway (eg. F454 or MH201, I'm not so updated about all gateways that BTicino-Legrand releases after 2015); the IP should be static but in the future I can implement a UPNP dicovery because all gateways supports that method
- port: should be 20000 and keep this value
- ownpassword: the OpenWebNet password, default is 12345 but everyone will suggest to you to change it with another password (4 to 9 digits), but you will keep the default one, I know...
- discovery: boolean value, not supported but in the future allows the gateway to discover the plant and detect most of devices
- devices: list of installed devices

The devices section contains the list of devices that will be managed. All devices contains three standard properties:

- accessory: the technical name of the device, should be one of the names listed in this document
- name: mnemonic name, will be displayed by iOS HomeKit application
- address: the MyHome address, usually in B/A/PL format for lights and curtaints or single/double digits for other devices. B stands for BUS (usually 0), A and PL is the name of the addressing object used by BTicino and stands for Ambient and Light Point (Punto Luce in the original italian version)

- MHRelay: Standard (Lighting) Relay, address is B/A/PL (eg. 0/1/10)
- MHDimmer: Lighting Dimmer, address is B/A/PL (eg. 0/1/10)
- MHThermostat: Standard Thermostat controlled by a 99-Zones Central Station (code 3550), address is the Zone Identifier (1-99)
- MHThermostatExternal: (WILL BE SUPPORTED)
- MHOutlet: (WILL BE SUPPORTED) -- Standard (not-Lighting) Relay, address is B/A/PL (eg. 0/1/10)
- MHCurtain: (WILL BE SUPPORTED) -- Standard Automation Relay, address is B/A/PL (eg. 0/1/10)


# TODOS

- Reconnection and infinite retry
- Semi-auto discovery and/or read a plant configuration from MyHomeSuite configuration file
- Read the correct light level of 100-levels dimmer
- Read the light level state of all devices in all buses
- Re-order the code
- IP Gateway discovery

# Disclaimer

I'm furnishing this software "as is". I do not provide any warranty of the item whatsoever, whether express, implied, or statutory, including, but not limited to, any warranty of merchantability or fitness for a particular purpose or any warranty that the contents of the item will be error-free.
The development of this module is not supported by Legrand, BTicino or Apple. These vendors and me are not responsible for direct, indirect, incidental or consequential damages resulting from any defect, error or failure to perform.  