# Asuswrt Binding

This binding adds support to control get informations from ASUS-Router (Copyright © ASUS).

#  !! EARLY BETA !!

#  BINDING AND REDME NOT FULLY WORKING/COMPLETED

## Supported Things

Binding supports ASUS-Router with AsusWRT or -AsusWRT-Merlin Firmware.
Firmware 5.x.x (some DSL models) is NOT supported (not AsusWRT).

| ThingType     | Name       | Descripion                              |
|---------------|------------|-----------------------------------------|
| bridge        | router     | router binding is connecting            |
| -             | interface  | network interface of router             |
| -             | client     | client is connected to the bridge       |

### `router` Thing Configuration

| Name            | Type    | Description                           | Default             | Required | Advanced |
|-----------------|---------|---------------------------------------|---------------------|----------|----------|
| hostname        | text    | Hostname or IP address of the device  | router.asus.com     | yes      | no       |
| username        | text    | Username to access the device         | N/A                 | yes      | no       |
| password        | text    | Password to access the device         | N/A                 | yes      | no       |
| useSSL          | boolean | Connect over SSL or use http://       | false               | no       | no       |
| refreshInterval | integer | Interval the device is polled in sec. | 20                  | no       | yes      |
| httpPort        | integer | HTTP-Port                             | 80                  | no       | yes      |
| httpsPort       | integer | HTTPS-Port                            | 443                 | no       | yes      |

### `interface` Thing Configuration

| Name            | Type    | Description                           | Default             | Required | Advanced |
|-----------------|---------|---------------------------------------|---------------------|----------|----------|
| interfaceName   | text    | options name of interface (wan/lan)   | N/A                 | yes      | no       |

### `client` Thing Configuration

| Name            | Type    | Description                           | Default             | Required | Advanced |
|-----------------|---------|---------------------------------------|---------------------|----------|----------|
| macAddress      | text    | Unique MAC-Address of the device      | N/A                 | yes      | no       |
| clientNick      | text    | Nickname used by OH                   | N/A                 | no       | no       |


## Properties

All devices support some of the following properties:

| property         | description                  | things supporting this channel        |
|------------------|------------------------------|---------------------------------------|
| vendor           | Vendor of device             | router, client                        |
| dnsName          | DNS-Name of device           | router, client                        |


## Channels

All devices support some of the following channels:

| group            | channel            |type                    | description                                | things supporting this channel    |
|------------------|--------------------|------------------------|--------------------------------------------|-----------------------------------|
| networkInfo      | macAddress         | text (RO)              | HW-Address                                 | interface, client                 |
|                  | ipAddress          | text (RO)              | IP-Address                                 | interface                         |
|                  | ipMethod           | text (RO)              | Ip-Method (static/dhcp)                    | interface, client                 |
|                  | subnet             | text (RO)              | Subnetmask                                 | interface                         |
|                  | gateway            | text (RO)              | Default-Gateway                            | interface                         |
|                  | dnsServers         | text (RO)              | DNS-Servers                                | interface                         |
|                  | networkState       | Switch (RO)            | Client is online                           | interface, client                 |
|                  | internetState      | Switch (RO)            | Client connected to Internet               | client                            |
| sysInfoGroup     | memTotal           | Number:DataAmountype   | Total memory in MB                         | router                            |
|                  | memUsed            | Number:DataAmountype   | Used memory in MB                          | router                            |
|                  | memFree            | Number:DataAmountype   | Free memory in MB                          | router                            |
|                  | memUsedPercent     | Number:Dimensionles    | Used memory in %                           | router                            |
|                  | cpuUsedPercent     | Number:Dimensionles    | Total CPU usage in percent over all cores  | router                            |
| clientListGroup  | knownClients       | text (RO)              | Known clients with name and MAC-Addresses  | router                            |
|                  | onlineClients      | text (RO)              | Online clients with name and MAC-Addresses | router                            |
|                  | onlineMACs         | text (RO)              | List with mac-addresses of online clients  | router                            |    
|                  | onlineClientsCount | Number:Dimensionless   | Count of online clients                    | router                            |


## Events

All devices support some of the following Events:
| group            | event           |kind        | description                                                            | things supporting this event    |
|------------------|-----------------|------------|------------------------------------------------------------------------|---------------------------------|
| networkInfo      | connectionEvent | Trigger    | Fired if Client leaves ('gone') or enters ('connected') the network    | interface, client               |



## Debugging and Tracing

If you want to see what's going on in the binding, switch the loglevel to TRACE in the Karaf console

`log:set TRACE org.openhab.binding.asuswrt`

Set the logging back to normal

`log:set INFO org.openhab.binding.asuswrt`+

