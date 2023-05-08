## Changelog:

#### 2023-05-08 Update Snapshot 4.0.0 version:

uploaded snapshot 4.0.0 version with new features:
   L530 now supports light effects
   P300 Multi Socket supported


#### 2022-11-25 ADD L610, L630 and L930 TTHINGS:

Added L610 and L630 SmartpSpots and L930 Multicolor LightStrip

#### 2022-11-07 FIXED CONIFÍGURATION ERROR:
   
Fixed configuration error: 'error reading device-configuration: 'For input string: "30.0"''

#### 2022-01-10 ENERGY MONITORING SUPPORTED :

   !! Things L510_series and L530_series were renamed to L510 and L530 because of manufacturer changed naming with new HW-rev. 
   You may need to remove and create again this things.

   Added Energy Monitoring support for P110


#### 2021-10-27 NEW BRIDGE VERSION : 

   !! Devices need now a bridge where the cloud-login-credetntials has to be set !!
   So you have to add the bridge-thing manually and add the devices again from your inbox or discovered devices of your binding!

![AddBridge](https://user-images.githubusercontent.com/40909464/139082881-e7e1eeed-c550-4d91-8a3a-e6e238e94c4b.png)
![addDiscovered](https://user-images.githubusercontent.com/40909464/139082902-6de4d9f2-4d78-40c7-9d34-987e787b38cc.png)


# TapoControl Binding

This binding adds support to control Tapo (Copyright © TP-Link Corporation Limited) Smart Home Devices from your local openHAB system.

## Supported Things

The following Tapo-Devices are supported. For precise channel-description look at `channels-table` below

| DeviceType                         | ThingType   | Description                                 |
|------------------------------------|-------------|---------------------------------------------|
| SmartPlug (Wi-Fi)                  | P100        | Smart Socket                                |
|                                    | P105        | Smart Mini Socket                           |
| EnergyMonitoring SmartPlug (Wi-Fi) | P110        | Energy Monitoring Smart Socket              |
|                                    | P115        | Energy Monitoring Mini Smart Socket         |
| Power Strip (Wi-Fi)                | P300        | Smart Wi-Fi Power Strip - 3 sockets         |
| Dimmable SmartBulb (Wi-Fi)         | L510        | Dimmable White-Light Smart-Bulb (E27)       |
|                                    | L610        | Dimmable White-Light Smart-Spot (GU10)      |
| MultiColor SmartBulb (Wi-Fi)       | L530        | Multicolor Smart-Bulb (E27)                 |
|                                    | L630        | Multicolor Smart-Spot (GU10)                |
| MultiColor LightStrip (Wi-Fi)      | L900        | Multicolor RGB Dimmable LightStrip (5m)     |
|                                    | L920        | Multicolor RGB-IC ColorZone LightStrip (5m) |
|                                    | L930        | Multicolor RGBW-IC 50-Zone LightStrip (5m)  |

## Prerequisites

Before using Smart Plugs with openHAB the devices must be connected to the Wi-Fi network.
This can be done using the Tapo provided mobile app.
You need to setup a bridge (Cloud-Login) to commiunicate with your devices.

**Note:** If the Tapo device is to be isolated from the internet e.g. on an IoT LAN, the P110 will not expose its energy and power data until it has successfully synchronised it's clock with an NTP server - at time of writing, this was `pool.ntp.org`.
To satisfy this requirement while keeping the device isolated, your router should be configured to either permit `udp/123` out to the internet or a NAT rule created to redirect all internet bound NTP traffic to a local NTP server.

## Discovery

Discovery is done by connecting to the Tapo-Cloud Service. 
All devices stored in your cloud account will be detected even if they are not in your network.
You need to know the IP-Adress of your device. This must be set manually in the thing configuration

## Bridge Configuration

The bridge needs to be configured with by `username` and `password` (Tapo-Cloud login) .
This is used for device discovery and to create a handshake (cookie) to act with your devices over the local network.

The thing has the following configuration parameters:

| Parameter          | Description                                                          |
|--------------------|----------------------------------------------------------------------|
| username           | Username (eMail) of your Tapo-Cloud                                  |
| password           | Password of your Tapo-Cloud                                          |

## Thing Configuration

The thing needs to be configured with `ipAddress`.

The thing has the following configuration parameters:

| Parameter          | Description                                                          |
|--------------------|----------------------------------------------------------------------|
| ipAddress          | IP Address of the device.                                            |
| pollingInterval    | Refresh interval in seconds. Optional. The default is 30 seconds     |


## Channels

All devices support some of the following channels:

| group     | channel          | type                   | description                  | things supporting this channel                                   |
|-----------|----------------- |------------------------|------------------------------|------------------------------------------------------------------|
| actuator  | output           | Switch                 | Power device on or off       | P100, P105, P110, P115, L510, L530, L610, L630, L900, L920, L930 |
|           | output1          | Switch                 | Power socket 1 on or off     | P300                                                             |
|           | output2          | Switch                 | Power socket 2 on or off     | P300                                                             |
|           | output3          | Switch                 | Power socket 3 on or off     | P300                                                             |
|           | brightness       | Dimmer                 | Brightness 0-100%            | L510, L530, L610, L630, L900                                     |
|           | colorTemperature | Number                 | White-Color-Temp 2500-6500K  | L510, L530, L610, L630, L900                                     |
|           | color            | Color                  | Color                        | L530, L630, L900                                                 |
| device    | wifiSignal       | Number                 | WiFi-quality-level           | P100, P105, P110, P115, L510, L530, L610, L630, L900, L920, L930 |
|           | onTime           | Number:Time            | seconds output is on         | P100, P105, P110, P115, L510, L530, L900, L920, L930             |
| energy    | actualPower      | Number:Power           | actual Power (Watt)          | P110, P115                                                       |
|           | todayEnergyUsage | Number:Energy          | used energy today (Wh)       | P110, P115                                                       |
|           | todayRuntime     | Number:Time            | seconds output was on today  | P110, P115                                                       |


## Channel Refresh

When the thing receives a `RefreshType` command the thing will send a new refreshRequest over http.
To minimize network traffic the default refresh-rate is set to 30 seconds. This can be reduced down to 10 seconds in advanced settings of the device. If any command was sent to a channel, it will do an immediately refresh of the whole device.


## Full Example

### tapocontrol.things:

```
tapocontrol:bridge:myTapoBridge                 "Cloud-Login"               [ username="you@yourpovider.com", password="verysecret" ]
tapocontrol:P100:myTapoBridge:mySocket          "My-Socket"     (tapocontrol:bridge:myTapoBridge)   [ ipAddress="192.168.178.150", pollingInterval=30 ]
tapocontrol:L510:myTapoBridge:whiteBulb         "white-light"   (tapocontrol:bridge:myTapoBridge)   [ ipAddress="192.168.178.151", pollingInterval=30 ]
tapocontrol:L530:myTapoBridge:colorBulb         "color-light"   (tapocontrol:bridge:myTapoBridge)   [ ipAddress="192.168.178.152", pollingInterval=30 ]
tapocontrol:L900:myTapoBridge:myLightStrip      "light-strip"   (tapocontrol:bridge:myTapoBridge)   [ ipAddress="192.168.178.153", pollingInterval=30 ]

Bridge tapocontrol:bridge:secondBridgeExample            "Cloud-Login"        [ username="youtoo@anyprovider.com", password="verysecret" ] {
   Thing P110 mySocket   "My-Socket"          [ ipAddress="192.168.101.51", pollingInterval=30 ]
}
```

### tapocontrol.items:

```
Switch       TAPO_SOCKET      "socket"                { channel="tapocontrol:P100:myTapoBridge:mySocket:actuator#output" }
``` 
