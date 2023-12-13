If you like my work and you'll support me for buying new hardware please buy me a coffee  
[![buymeacoffee](doc/buymeacoffee.png)](https://www.buymeacoffee.com/wildcs)

## Install Binding manually:

1 - Remove the TapoControl Binding from the Binding interface  
2 - Download the latest snapshot “org.openhab.binding.tapocontrol-x.x.x-SNAPSHOT.jar” for your version from here   
3 - Put that file `/usr/share/openhab/addons/`  
4 - Set file permission : `sudo chown openhab:openhab /usr/share/openhab/addons/org.openhab.binding.tapocontrol-x.x.x-SNAPSHOT.jar`  
5 - Stop openhab : `sudo service openhab stop`  
6 - Clean cache (just in case) : `sudo openhab-cli clean-cache`  
7 - Start openhab : `sudo service openhab start`  
8 - Buy me a coffee :wink: 

## Changelog:

#### 2023-10-30 Integration local UDP-Discovery:
local online devices can now be discovered via udp and cloud.
   L9x0 LightStrips now supporting light effects

#### 2023-09-25 KLAP Protocol-integration:
completely new core code because of protocol change for some devices.
   new klap protocol integration
   T310 and T315 Wheather-Sensors supported
   ***!!! Known issues:***

   * Protocol has to be selected manually in advanced settings

#### 2023-08-23 Update Snapshot 4.1.0 version:
uploaded snapshot 4.1.0 version with new features:
   H100 SmartHub integrated
   T110 SmartContact integrated
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

| DeviceType                         | ThingType   | Description                                  |
|------------------------------------|-------------|----------------------------------------------|
| SmartPlug (Wi-Fi)                  | P100        | Smart Socket                                 |
|                                    | P105        | Smart Mini Socket                            |
| EnergyMonitoring SmartPlug (Wi-Fi) | P110        | Energy Monitoring Smart Socket               |
|                                    | P115        | Energy Monitoring Mini Smart Socket          |
| Power Strip (Wi-Fi)                | P300        | Smart Wi-Fi Power Strip - 3 sockets          |
| Dimmable SmartBulb (Wi-Fi)         | L510        | Dimmable White-Light Smart-Bulb (E27)        |
|                                    | L610        | Dimmable White-Light Smart-Spot (GU10)       |
| MultiColor SmartBulb (Wi-Fi)       | L530        | Multicolor Smart-Bulb (E27)                  |
|                                    | L630        | Multicolor Smart-Spot (GU10)                 |
| MultiColor LightStrip (Wi-Fi)      | L900        | Multicolor RGB Dimmable LightStrip (5m)      |
|                                    | L920        | Multicolor RGB-IC ColorZone LightStrip (5m)  |
|                                    | L930        | Multicolor RGBW-IC 50-Zone LightStrip (5m)   |
| Smart Hub (Wi-Fi / RF)             | H100        | Smart Hub with Chime to control Child Devices|
| Smart Contact Sensor (RF)          | T110        | Window/Door Smart Contact Sensor             |
| Smart Temperature Sensor (RF)      | T310        | Temperature and Humidity Sensor              |
|                                    | T315        | Temperature and Humidity Sensor with Display |

## Prerequisites

Before using Smart Plugs with openHAB the devices must be connected to the Wi-Fi network.
This can be done using the Tapo provided mobile app.
You need to setup a bridge (Cloud-Login) to commiunicate with your devices.

**Note:** If the Tapo device is to be isolated from the internet e.g. on an IoT LAN, the P110 will not expose its energy and power data until it has successfully synchronised it's clock with an NTP server - at time of writing, this was `pool.ntp.org`.
To satisfy this requirement while keeping the device isolated, your router should be configured to either permit `udp/123` out to the internet or a NAT rule created to redirect all internet bound NTP traffic to a local NTP server.

## Discovery

For Wi-Fi-Devices, discovery is done by connecting to the Tapo-Cloud-Service or use local udp-discovery.
If enabled, all devices stored in your cloud account will be detected even if they are not in your local network.
From cloud you can get more informations such as "Device-Alias" as from udp-discovery.
But you need to know the IP-Adress of your device. This must be set manually in the thing configuration.

Udp-Discovery can find only devices which are online in your local network and get less informations as from cloud. But therefore it set's device-ip and protocol automaticly.
If you have problems with udp-discovery, try to set the advanced setting 'broadcastAddress' to your local subnet ('e.g. 192.168.0.255'). Default is '255.255.255.255'

You can combine both discovery methods to get any informations from local devices. 
If you enable setting 'onlyLocalOnlineDevices' results will only generated for local online devices but with the combined data of cloud discovery. If this setting is disabled and you enabled both discovery-methods, you will get all devices stored in cloud, but if they are not online you have to set the device-ip and protocol manually.

RF-Devices will be discovered by the hub they are connected to. You can discover them manually or use ´backgroundDiscovery´ 

## Bridge Configuration

The bridge needs to be configured with by `username` and `password` (Tapo-Cloud login) .
This is used for device discovery and to create a handshake (cookie) to act with your devices over the local network.

The thing has the following configuration parameters:

| Parameter              | Description                                                                                         |
|------------------------|-----------------------------------------------------------------------------------------------------|
| username               | Username (eMail) of your Tapo-Cloud                                                                 |
| password               | Password of your Tapo-Cloud                                                                         |
| cloudDiscovery         | Use Cloud Discovery-Service to get all in Tapo-App registered devices. Includes DeviceName. IP-Address and Encryption has to set manually                     |
| udpDiscovery           | Use UDP Discovery-Service to discover online devices in the local network. Includes Encryption and IP-Address. Results will be merged with cloud discovery    |
| onlyLocalOnlineDevices | [advanced] Uses Cloud and UPD-Discovery to get more informations but will only discover online devices via UDP |
| broadcastAddress       | [advanced] Set broadcast address to your local subnet  if you have problems with default address   |
| discoveryInterval      | [advanced] Interval in minutes when a background device scan should be executed. Default is 60     |


## Thing Configurations

WiFi-Things needs to be configured with `ipAddress`.
RF-Things need a Hub (WiFi-Device) to operate. 

The things has the following configuration parameters:

| Parameter          | Description                                                           | Things supporting parameter |
|--------------------|-----------------------------------------------------------------------|-----------------------------|
| ipAddress          | IP Address of the device.                                             | Any Wi-Fi-Device            |
| pollingInterval    | [optional] Refresh interval in seconds. The default is 30 seconds     | Any Wi-Fi-Device            |
| httpPort           | [optional] HTTP-Communication Port. Default is 80                     | Any Wi-Fi-Device            |
| protocol           | [optional] Used Communication Protocol (AES/KLAP/'') Default 'AES'    | Any Wi-Fi-Device            |
| backgroundDiscovery| [optional] RF-Devices will be discovered after every polling request  | SmartHub                    |


## Channels

All devices support some of the following channels:

| group     | channel          | type                   | description                         | things supporting this channel                                   |
|-----------|----------------- |------------------------|-------------------------------------|------------------------------------------------------------------|
| actuator  | output           | Switch                 | Power device on or off              | P100, P105, P110, P115, L510, L530, L610, L630, L900, L920, L930 |
|           | output1          | Switch                 | Power socket 1 on or off            | P300                                                             |
|           | output2          | Switch                 | Power socket 2 on or off            | P300                                                             |
|           | output3          | Switch                 | Power socket 3 on or off            | P300                                                             |
|           | brightness       | Dimmer                 | Brightness 0-100%                   | L510, L530, L610, L630, L900, L920                               |
|           | colorTemperature | Number                 | White-Color-Temp 2500-6500K         | L510, L530, L610, L630, L900, L920                               |
|           | color            | Color                  | Color                               | L530, L630, L900, L920                                           |
| sensor    | isOpen           | Switch                 | Contact (Door/Window) is Open       | T110                                                             |
|           | currentTemp      | Number:Temperature     | Current Temperature                 | T310, T315                                                       |
|           | currentHumidity  | Number:Dimensionless   | Current relative humidity in %      | T310, T315                                                       |
| effects   | fxName           | String                 | Active lightning effect             | L530, L900, L920, L930                                                           |
| device    | wifiSignal       | Number                 | WiFi-quality-level                  | P100, P105, P110, P115, L510, L530, L610, L630, L900, L920, L930 |
|           | onTime           | Number:Time            | seconds output is on                | P100, P105, P110, P115, L510, L530, L900, L920, L930             |
|           | signalStrength   | Number                 | RF-quality-level                    | T110                                                             |
|           | isOnline         | Switch                 | Device is Online                    | T110                                                             |
|           | batteryLow       | Switch                 | Battery of device is low            | T110                                                             |
| energy    | actualPower      | Number:Power           | actual Power (Watt)                 | P110, P115                                                       |
|           | todayEnergyUsage | Number:Energy          | used energy today (Wh)              | P110, P115                                                       |
|           | todayRuntime     | Number:Time            | seconds output was on today         | P110, P115                                                       |
| alarm     | alarmActive      | Switch                 | Alarm is currntly active            | H100                                                             |
|           | alarmSource      | String                 | Source causes active alarm          | H100                                                             |

## Channel Refresh

When the thing receives a `RefreshType` command the thing will send a new refreshRequest over http.
To minimize network traffic the default refresh-rate is set to 30 seconds. This can be reduced down to 10 seconds in advanced settings of the device. If any command was sent to a channel, it will do an immediately refresh of the whole device.

## Full Example

### tapocontrol.things:

```java
tapocontrol:bridge:myTapoBridge                 "Cloud-Login"               [ username="you@yourpovider.com", password="verysecret" ]
tapocontrol:P100:myTapoBridge:mySocket          "My-Socket"     (tapocontrol:bridge:myTapoBridge)   [ ipAddress="192.168.178.150" ]
tapocontrol:L510:myTapoBridge:whiteBulb         "white-light"   (tapocontrol:bridge:myTapoBridge)   [ ipAddress="192.168.178.151", httpPort=80, pollingInterval=30, protocol="AES" ]
tapocontrol:L530:myTapoBridge:colorBulb         "color-light"   (tapocontrol:bridge:myTapoBridge)   [ ipAddress="192.168.178.152", pollingInterval=30, protocol="KLAP" ]
tapocontrol:L900:myTapoBridge:myLightStrip      "light-strip"   (tapocontrol:bridge:myTapoBridge)   [ ipAddress="192.168.178.153", pollingInterval=30, protocol="" ]

Bridge tapocontrol:bridge:secondBridgeExample            "Cloud-Login"        [ username="youtoo@anyprovider.com", password="verysecret" ] {
   Thing P110 mySocket   "My-Socket"          [ ipAddress="192.168.101.51", pollingInterval=30 ]
}
```

### tapocontrol.items:

```java
Switch       TAPO_SOCKET      "socket"                { channel="tapocontrol:P100:myTapoBridge:mySocket:actuator#output" }
```
