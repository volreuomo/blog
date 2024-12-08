# 200-301 CCNA

Notes for the [200-301 CCNA Training from YouTube](https://www.youtube.com/playlist?list=PLh94XVT4dq02frQRRZBHzvj2hwuhzSByN) and [Cisco CCNA 200-301](https://www.udemy.com/course/ccna-complete/) courses:

1. [Network Fundamentals](#network-fundamentals)
1. [IP Address](#ip-address)
1. [Networking Devices and Protocols](#networking-devices-and-protocols)
1. [Cisco IOS](#cisco-ios)
1. [Switches](#switches)
1. [VLANs](#vlans)
1. [VTP](#vtp)
1. [DTP](#dtp)
1. [Switchport Security](#switchport-security)
1. [Routers](#routers-and-routing)
1. [RIP Routing](#rip-routing)
1. [Routing Advanced](#routing-advanced)
1. [IPv6](#ipv6)
1. [ACL](#acl)
1. [NAT & PAT](#nat--pat)
1. [Troubleshooting Techniques](#troubleshooting-techniques)
1. [CDP, Syslog & NTP](#cdp-syslog--ntp)
1. [Password Reset & Licensing](#password-reset--licensing)
1. [STP](#stp)
1. [RSTP](#rstp)
1. [PortFast and BPDU Guard](#portfast-and-bpdu-guard)
1. [Other STP Modes](#other-stp-modes)
1. [EtherChannel](#etherchannel)
1. [EIGRP](#eigrp)
1. [OSPF](#ospf)
1. [HSRP](#hsrp)
1. [QoS](#qos)
1. [Network Automation & Programmability](#network-automation--programmability)
1. [Task 1 - Router Configuration](#task-1---router-configuration)
1. [Task 2 - Router Switch Configuration](#task-2---router-switch-configuration)
1. [Task 3 - VLANs](#task-3---vlans)
1. [Task 4 - Router on a Stick](#task-4---router-on-a-stick)
1. [Task 5 - Static Routing](#task-5---static-routing)
1. [Task 6 - NAT and PAT](#task-6---nat-and-pat)
1. [Task 7 - IPv6 Routing](#task-7---ipv6-routing)
1. [Task 8 - DNS & DHCP](#task-8---dns--dhcp)
1. [Task 9 - SNMP & Syslog](#task-9---snmp--syslog)
1. [Task 10 - NTP](#task-10---ntp)
1. [Task 11 - EtherChannel](#task-11---etherchannel)
1. [Task 12 - RIP & EIGRP](#task-12---rip--eigrp)
1. [Task 13 - OSPF](#task-13---ospf)
1. [Task 14 - HSRP](#task-14---hsrp)

## Network Fundamentals

* Network - multiple computers linked to share resources and communicate; common types include LAN (Local Area Network) and WAN (Wide Area Network).

* OSI Model and TCP-IP Model:

  ![OSI Model - TCP-IP Model](Images/osi-tcpip.png)

* Layers of the OSI (Open Systems Interconnection) model:

  * Application layer - PoC for all network-aware apps
  * Presentation layer - converts data from session layer and sends it to application layer; offers encryption services
  * Session layer - creates and maintains sessions
  * Transport layer - uses segments as data unit, and adds header to segments for encapsulation; can use reliable (TCP) or unreliable (UDP) communication - also adds source port numbers
  * Network layer - uses packets as data unit; responsible for IP addressing and finding best path
  * Data link layer - uses frames as data unit; responsible for MAC addressing and error checking
  * Physical layer - uses bits as data unit; actual data transfer and hardware connections

* Simplified working of OSI model:

```markdown
Suppose we send an email to someone; this email would first pass through application layer towards presentation layer - this will compress the data.

The session layer is responsible for communication and encryption. Then, the data is divided into segments in transport layer - this layer also adds a transport header to the data.

Now, the segments are sent to the network layer, which adds a header of its own to the segments, thus creating packets.

These packets are forwarded to data link layer, which adds a data link header to it, where it would be called a frame now.

Finally, this frame is converted into bits (0s and 1s) in the physical layer.

Once the physical layer sends these bits to the recipient physical layer, the receiving layers strip the headers of the corresponding layers until it gets all the segments, which is then taken care of by the upper layers.
```

## IP Address

* An IP address consists of 4 octets, with numbers in each octet ranging from 0-255; the address is of 32 bits in binary format.

* IP address has two parts - network (all 1s) and host (all 0s); this can be determined using the subnet mask.

* If an IP address needs to communicate with another IP address, it has to go through the gateway.

* IP address classes identified by first octet:

  * Class A - 1-126
  * Class B - 128-191
  * Class C - 192-223
  * Class D - 224-239
  * Class E - 240-255

* Example:

```markdown
IP address - 192.168.100.225 (/24)

Subnet mask - 255.255.255.0

Gateway - 192.168.100.1

Network Id (first) - 192.168.100.0

Broadcast Id (last) - 192.168.100.255

Number of valid hosts - 254
```

* Number of valid IP addresses in network is ```2^(number of host bits) - 2```

* Private IP addresses - IP addresses which can communicate only on local network:

  * Class A - 10.0.0.0 - 10.255.255.255
  * Class B - 172.16.0.0 - 172.31.255.255
  * Class C - 192.168.0.0 - 192.168.255.255

* Subnetting - dividing network into smaller subnets; uses CIDR (Classless Inter Domain Routing) notation. We can divide a network only in even numbers of subnets.

* Subnetting examples:

```markdown
IP address - 192.168.100.225
Subnet mask - 255.255.255.0

Suppose we need to divide this network into 2 subnets.
We can denote subnet mask in binary format - 11111111.11111111.11111111.00000000

To divide in 2 subnets, extend network bits by 1, such that we have total of 25 network bits now.
New subnet mask - 11111111.11111111.11111111.10000000 (or 255.255.255.128)

Using this format, we have -

  Network Id 1 - 192.168.100.0/25
  Network Id 2 - 192.168.100.128/25

  Broadcast Id 1 - 192.168.100.127/25
  Broadcast Id 2 - 192.168.100.255/25
```

```markdown
IP address - 192.168.100.225
Subnet mask - 255.255.255.192 - 11111111.11111111.11111111.11000000

We have 24+2=26 network bits here; so the number of possible subnets is 2^2=4

Network Id 1 - 192.168.100.0/26
Network Id 2 - 192.168.100.64/26
Network Id 3 - 192.168.100.128/26
Network Id 4 - 192.168.100.192/26

Broadcast Id 1 - 192.168.100.63/26
Broadcast Id 2 - 192.168.100.127/26
Broadcast Id 3 - 192.168.100.191/26
Broadcast Id 4 - 192.168.100.255/26
```

```markdown
Given IP - 192.168.225.212/27

Here, /27 means we have 27 (24+3) network bits, so number of possible subnets would be 8 (2^3); each subnet would have 30 (256/8 - 2) hosts.

Therefore, Network Id is 192.168.225.192/27 and Broadcast Id is 192.168.225.223/27
```

```markdown
IP - 165.245.12.88/20

Here, /20 means we have 20 (16+4) network bits, so the 4 network bits in binary representation would be 11110000, which is 240 in binary.

So subnet mask - 255.255.240.0

Also, as we have 4 network bits in third octet, we will have 2^4=16 subnets.

So, network Id for the subnets would be 165.245.0.0, 165.245.16.0, 165.245.32.0, and so on (since 256/16=16)

For corresponding IP, network Id - 165.245.0.0 and broadcast Id - 165.245.15.255
```

```markdown
IP - 18.172.200.77/11

/11 means we have 11 (8+3) network bits; converting the 3 network bits from binary to decimal gives us 224.

So, subnet mask - 255.224.0.0

As we have 3 network bits in second octet, we have 2^3=8 subnets.

So, network Id for subnets would be 18.0.0.0, 18.32.0.0, 18.64.0.0 and so on.

Therefore, for given IP, network Id - 18.160.0.0 and broadcast Id - 18.191.255.255
```

```markdown
We need to design a network with 3 subnets with requirements of 60, 100 and 34 computers for each subnet (VLSM).

For the requirement of 100 computers, we can use the subnet with 126 hosts such that Network Id - 192.168.1.0/25, and Broadcast Id - 192.168.1.127/25

For the next requirement of 60 computers, we can use a subnet with 62 hosts, that is, with Network Id - 192.168.1.128/26 and Broadcast Id - 192.168.1.191/26

Similarly, for the final 34 computers, we can use the Network Id 192.168.1.192/26 and Broadcast Id 192.168.1.255/26
```

```markdown
IP address - 172.16.100.225
Subnet mask - 255.255.0.0

For dividing this network into 2 subnets, we can use the approach of extending network bits by 1, ending up with 17 network bits.

Network Id 1 - 172.16.0.0/17
Network Id 2 - 172.16.128.0/17

Broadcast Id 1 - 172.16.127.255/17
Broadcast Id 2 - 172.16.255.255/17
```

```markdown
IP address - 10.20.100.225
Subnet mask - 255.0.0.0

Dividing this network into 2 subnets, extend network bits by 1, so we have 9 network bits now.

Network Id 1 - 10.0.0.0/9
Network Id 2 - 10.128.0.0/9

Broadcast Id 1 - 10.127.255.255/9
Broadcast Id 2 - 10.255.255.255/9
```

```markdown
We have to find network and broadcast Id of 172.10.21.21/24

As this is a class B address, subnet mask is 255.255.0.0

So, the network Id would start from 172.10.0.0 for the subnets.
According to given IP, the corresponding network Id would be 172.10.21.0 and Broadcast Id 172.10.21.255
```

## Networking Devices and Protocols

* Hub:

  * Non-intelligent
  * 1 collision domain
  * 1 broadcast domain
  * Layer 1 device
  * On basis of broadcasting

* Switch:

  * Intelligent device (uses ASIC, can store MAC address info)
  * Many collision domains (as many as number of ports)
  * 1 broadcast domain and 1 IP network
  * Layer 2 device
  * On basis of MAC address

* Router:

  * Intelligent device (can act as gateway)
  * Many collision domains
  * Many broadcast domains
  * Layer 3 device
  * On basis of IP address

* DHCP (Dynamic Host Configuration Protocol) - to configure IP addresses to hosts dynamically; DHCP has mainly 6 types of messages (4 are mandatory):

  * DHCP Discover
  * DHCP Offer
  * DHCP Request
  * DHCP Ack
  * DHCP Information (optional)
  * DHCP Release (optional)

* TCP (Transmission Control Protocol):

  * Features:

    * Reliable
    * Connection-oriented
    * Error-checking and recovery mechanism
    * Flow control and QoS (Quality of Service)
    * Full-duplex
    * Point-to-point
    * Windowing (flow control mechanism to determine buffer for receiver)

  * Working:

    * Connection is established using 3-way handshake
    * SYN segment (client to server) -> SYN+ACK segment (server to client) -> ACK segment (client to server)

* UDP (User Datagram Protocol):

  * Features:

    * Unreliable
    * Connection-less
    * Faster transmission
    * Independently-handled segments
    * Stateless (no ACK)
    * Process-to-process

  * Working:

    * UDP takes a datagram from Network Layer, attaches its header, and sends it to user
    * Used mainly in streaming services, real-time apps

* ARP (Address Resolution Protocol):

  * Used to find MAC address of device corresponding to its IP address.
  * To establish connection between two devices, source device needs to generate ARP request.
  * ARP request is broadcast, but ARP reply is unicast.

## Cisco IOS

* Cisco IOS (Internetwork Operating System) - proprietary OS for Cisco switches & routers.

* Configuration modes:

  * User EXEC mode - denoted by prompt ```>```; type ```enable``` (or ```en``` or ```ena```) to escalate to higher mode.

  * Privileged EXEC mode - denoted by prompt ```#```; type ```configure terminal``` (or ```conf t```) to escalate to highest mode.

  * Global configuration mode - denoted by prompt ```(config)#```; this mode can be used for configuration in interfaces, routers and lines as well.

* In Cisco IOS, ```?``` is used in command line for help.

* Switch config commands:

```shell
en

conf t
#enter global config mode

hostname newName
#change hostname

banner motd
#modify banner MOTD

line con 0
password p@55wd
login
#enforce console password

no ip domain-lookup
#disable domain lookup

line vty 0 15
password t3ln3t
login
#enforce telnet password

enable password en4bl3
#enforce password for 'en' command

exit
#back to privileged exec mode

sh ip int br
#show ip interface brief - shows all interfaces

conf t
#switch to global config mode

int vlan 1
ip add 10.1.1.1 255.255.255.0
#add management IP

shutdown
#configure interface to shutdown

no shutdown
#change interface to UP status

ip default-gateway 10.1.1.10
#add default gateway (in case of multiple networks)

exit
#back to privileged exec mode

write
#save running config (temporary) to startup config (permanent)

copy running-config startup-config
#newer way to save config
```

* Enabling SSH:

```shell
en
conf t
#global config mode

ip domain-name sv.org
#add domain-name

crypto key generate rsa
#generate secure key

ip ssh version 2
#use SSH version 2

username testuser password pa55123
#create user-password

line vty 0 15
transport input ssh
#allow SSH only
```

## Switches

* Switch functions:

  * Address learning - switches learn source MAC addresses and store it in the CAM (Content Addressable Memory) table or MAC address table.

  * Forwarding decision - switches decide whether to forward or filter a frame based on destination MAC address.

  * Loop avoidance - switches use STP (Spanning Tree Protocol) to prevent layer 2 loops like MAC address flapping.

* Switchport modes:

  * Access mode - used when connecting to end device; enabled by command ```switchport mode access```

  * Trunk mode - used when connecting to another switch; enabled by command ```switchport mode trunk```

* Types of trunking protocols:

  * ISL (Inter-Switch Link) - old protocol; Cisco-proprietary
  * IEEE 802.1Q (dot1q) - industry standard; non-proprietary

* DTP (Dynamic Trunking Protocol) - Cisco-proprietary protocol for switches; its modes are:

  * Desirable - switchport starts sending DTP packets to negotiate; ```switchport mode dynamic desirable```
  * Auto - flexible state, waits for DTP negotiation; ```switchport mode dynamic auto```
  * No negotiate - disable DTP and trunking; ```switchport nonegotiate```

## VLANs

* VLANs (virtual LANs) are logically grouped devices, configured on switches in the same broadcast domain.

* Each VLAN is treated as its own subnet or broadcast domain, so the frames broadcasted onto the network will be switched only between ports in same VLAN.

* VLANs help in flexibility and security of network designs.

* When frames traverse a trunk port, a VLAN tag is added to distinguish which frames belong to which VLANs; access ports do not require a VLAN tag as all frames belong to a single VLAN.

* Native VLAN is a special VLAN whose traffic traverses a trunk port without a VLAN tag (untagged).

* VLAN config:

```shell
en
#in priv exec mode now

show vlan
#shows VLANs

conf t
#in global config mode now

vlan 10
#create VLAN 10 (can use any number from 1-1005)

name departmentName

#Ctrl+Z
#in priv exec mode

int f0/1
#interface config

switchport mode access

switchport access vlan 10
#access mode

#Ctrl+Z

int vlan1
#default VLAN

ip add 10.1.1.1 255.255.255.0
#assign management IPs

no sh

#Ctrl+Z

int f0/4

switchport mode trunk
#config trunk mode
#assign port to trunk

#Ctrl+Z

sh interfaces trunk
#shows trunk interfaces, native vlan

#Ctrl+Z

int range f0/2-3
#config for multiple ports

switchport mode access

switchport access vlan 20
#vlan 20 created for fa0/2 and fa0/3
```

* Native VLAN config:

```shell
en
conf t

#global config mode

int fa0/1

switchport trunk native vlan 10
#any traffic out of switch for vlan 10 goes untagged
#possible native vlan mismatch due to different configs
```

* Default VLAN vs Native VLAN:

| **Parameter**            | **Default VLAN**                                                                   | **Native VLAN**                                  |
|--------------------------|------------------------------------------------------------------------------------|--------------------------------------------------|
| _Modifying VLAN_         | Default VLAN is 1, cannot be changed                                               | Native VLAN can be changed                       |
| _Disabling VLAN_         | Cannot be disabled                                                                 | Can be 'disabled'                                |
| _Untagged VLAN_          | Untagged traffic sent to default VLAN only when native VLAN & default VLAN is same | Untagged traffic sent to native VLAN in any case |
| _Default VLAN values_    | 1, 1002-1005 (old)                                                                 | Any one VLAN per dot1q trunk port                |
| _Encapsulation_          | Supports both ISL and dot1q                                                        | Supports only dot1q                              |
| _Recommendation_         | Default VLAN is always 1                                                           | Native VLAN should be an unused VLAN id          |
| _Shutdown_               | Cannot be shut                                                                     | Can be shut                                      |
| _CDP, PAgp, VTP traffic_ | Sent on default VLAN                                                               | Not sent                                         |
| _DTP traffic_            | Not sent                                                                           | Sent on native VLAN                              |
| _Max VLANs_              | 1 per switch                                                                       | No. of dot1q trunk ports on switch               |

* Normal VLAN vs Extended VLAN:

| **Parameter**           | **Normal VLAN** | **Extended VLAN** |
|-------------------------|-----------------|-------------------|
| _VLAN IDs_              | 1-1005          | 1006-4095         |
| _Default or Restricted_ | 1, 1002-1005    | 4095              |
| _Stored in VLAN DB_     | Yes             | Only in VTP v3    |
| _Supported VTP_         | v1, v2, v3      | v3                |

## VTP

* VTP (VLAN Trunking Protocol) - Cisco-proprietary protocol, used to sync VLAN info in same VTP domain; it is not a trunking protocol.

* Using VTP, when a new VLAN is created, it is replicated across all switches with the help of a synced VLAN DB (we only need to create access port).

* VTP uses Revision no. to compare & prioritize VLAN DB, so it needs to be used carefully; best practice is to not use VTP unless required.

* Requirements:

  * VTP version must be same on the switches to be configured.
  * VTP domain name must be same on switches
  * One of the switches must be a server
  * Authentication should match (if used)

* VTP modes:

  * Server - for making config changes
  * Client - receives changes from server; cannot make any changes
  * Transparent - not using VTP; but relays info

* VTP config:

```shell
en

show vtp status
#shows vtp version, status, server by default

sh vlan brief
#view vlan info

conf t
#in global config mode

vtp version <1/2>

vtp mode <server/client/transparent>

vtp domain domainName.org
#keep same domains for all switches for vtp to work

vtp password pa55wd
```

* VTP Pruning - improves network performance & bandwidth by decreasing unnecessary flooded traffic; disabled by default in Cisco switches.

* VTP pruning sends broadcasts only to those trunk links that actually need the info.

* VLAN 1 (default VLAN) cannot be pruned as it's an administrative VLAN.

```shell
en
conf t
#enter global config mode

vtp pruning
#enables vtp pruning
#does not work in Cisco Packet Tracer

vtp status
#shows pruning enabled


#'allowed vlan' command can also be used alternatively
int f0/1

switchport mode trunk

switchport trunk allowed vlan <all/none/add/remove/VLAN id>
```

## DTP

* In Cisco switches, by default ports are in Dynamic Auto mode (Cisco-proprietary mode).

* Switchport mode combinations:

| _Modes_               | **Access** | **Dynamic Auto** | **Trunk** | **Dynamic Desirable** |
|-----------------------|------------|------------------|-----------|-----------------------|
| **Access**            | Access     | Access           | Don't use | Access                |
| **Dynamic Auto**      | Access     | Access           | Trunk     | Trunk                 |
| **Trunk**             | Don't use  | Trunk            | Trunk     | Trunk                 |
| **Dynamic Desirable** | Access     | Trunk            | Trunk     | Trunk                 |

* DTP:

```shell
#in switch
sh int trunk
#shows trunk interfaces

sh int f0/1 switchport
#shows mode of switchport for interface

#to change mode
#in global config
int f0/1

switchport mode dynamic desirable

exit

sh int trunk
#now we have trunk interfaces
```

## Switchport Security

* Reasons for slow connection include speed, duplex (half/full) mismatch:

```shell
en
conf t

int f0/1
#config interface

speed 100
#100 Mbps

duplex full

#Ctrl+Z

sh interfaces f0/1
#shows speed, duplex config
```

* Port security (can only be enabled on access ports):

```shell
#in interface config mode

switchport port-security ?
#shows all options
#this is also a valid command to enable port-security

switchport port-security mac-address 0001.6307.EEAE
#only MAC address allowed on switchport

switchport port-security mac-address sticky
#for configuring addresses dynamically

switchport port-security maximum 2
#max addresses to be learnt by switchport

switchport port-security violation <protect/restrict/shutdown>
#marks as security violation

#Ctrl+Z to go to user exec mode

sh interfaces f0/1
#can check violations from here

sh port-security
#tabular view

sh port-security address
#shows secure MAC address table

sh port-security interface f0/1
#more details

#restart switch to make port-security work

int f0/1

shutdown
no shutdown
```

## Routers and Routing

* Routers - Layer 3 device, which can act as gateways; responsible for the routing process (path selection).

* Routers receive packets when the destination node is in a different network; this process of sending packets between two networks is aided by the routing table (repo of all routes to all destinations in network).

* Types of routing:

  * Static (or non-adaptive) routing - network admin uses static tables to manually config & select network routes; helpful where constant parameters are required, but this also leads to decreased adaptibility and flexibility of networks.

  * Dynamic (or adaptive) routing - routers create & update routing tables at runtime based on network conditions; they attempt to find fastest path using dynamic routing protocols (which maintain the routing table).

* Types of routing protocols:

  * Interior gateway protocols (IGP) - these protocols assess the Autonomous System and make routing decisions based on metrics like hop counts, delay and bandwidth. Types of IGP include:

    * RIP (Routing Information Protocol)
    * OSPF (Open Shortest Path First)
    * EIGRP (Enhanced Interior Gateway Routing Protocol) - Cisco-proprietary, efficient

  * Exterior gateway protocols (EGP) - these protocols manage multiple Autonomous Systems. Types of EGP include:

    * BGP (Border Gateway Protocol)
    * EGP (Exterior Gateway Protocol)

* Types of routing algorithms:

  * Distance vector routing - checks distance (hops); goes for shortest path (e.g. - RIP)
  * Link state routing - checks link state/speed; calculates cost of resources associated with each path (e.g. - OSPF)

* Cisco routers are enhanced by CEF (Cisco Express Forwarding); this minimises the load on the router's processor and makes routing more efficient.

* Router config:

```shell
en
conf t
#enter global config mode

hostname R1
#change hostname

do sh ip int br

int g0/0

no shutdown
#change interface to UP

line con 0

password c0ns0le
login
#console password

#Ctrl+Z to go to global config

service password-encryption
#encrypts password in running-config

line vty 0 4

password t3ln3t
#telnet password

#in global config mode

int g0/0

ip address 10.1.1.1 255.255.255.0
no sh
#assign ip address

banner motd &
#enter logon banner message

#in priv exec mode

copy running-config startup-config
#save config

sh ip interface br
show interfaces
#to verify changes in config

show ip route
#view routing table
```

* Static routing config:

```shell
#after initial router config

sh ip route
#view routing table
#static routing to add routes manually

#format - ip route <network id> <mask> <next hop/exit interface>

#in global config mode

ip route 192.168.4.0 255.255.255.0 192.168.2.2
#this router will send any traffic for 192.168.4.0 to 192.168.2.2

do sh ip route
#view updated routing table

#update other routers as well for both directions via static routing
```

## RIP Routing

* RIP (Routing Information Protocol) versions:

  * RIPv1:

    * Classful (does not support VLSM)
    * No authentication
    * Uses broadcast

  * RIPv2:

    * Classless (supports VLSM)
    * Adds authentication
    * Uses multicast

* RIP uses hop counts as a metric (measure used to decide which route is better; lower number is better); only 15 hops in a path are supported.

* RIP config:

```shell
#after initial router config
#in global config mode

router ?
#shows supported protocols

router rip
#use rip

version 2

network 10.0.0.0
#enter classful version

network 192.168.1.0
#enter other adjacent network id (classful)

no auto-summary
#turn off auto-summarization

#router starts advertising routes to other networks

#Ctrl+Z

sh ip route
#view routing table

#follow rip config for other routers as well

#troubleshooting commands

debug ip rip
#to debug all rip updates

undebug all
#turn off debug mode

sh ip protocols
#view routing protocols used
```

* RIP timers:

  * Update timer - 30 seconds - to send routing updates
  * Invalid timer - 180 seconds - since last valid update was received
  * Holddown timer - 180 seconds - waiting time before any new updates are accepted
  * Flush timer - 240 seconds - since last valid update was received, until route is flushed

* If the updates are not received, the invalid and flush timers begin simultaneously; after 180 seconds, the invalid timer ends and the holddown timer begins.

* RIP limitations:

  * Auto summarization, leads to inaccurate info
  * Slow convergence (shared info between routers)
  * Loops
  * Problem with distance vector, should consider link state metric as well

* Administrative distance (AD) - number assigned to routes (static, dynamic, directly-connected) which denotes trustworthiness of routing info received from neighbor router; the lower the AD value, the more preferred the route.

* Default AD values:

  * Directly connected - 0
  * Static route - 1
  * Internal EIGRP - 90
  * OSPF - 110
  * RIP - 120
  * External EIGRP - 170
  * Unknown - 255

* ```Counting to Infinity``` problem:

```markdown
This problem occurs due to routing loops in the network, which happens in two cases:

  * When 2 neighboring routers in network send an update simultaneously at the same time to the router

  * When an interface between two routers goes down in the network

This leads to a case of incorrect updates being sent by one router to its neighboring router, and it is about the number of hops required to reach the unreachable router (as the interface is down).

This incorrect update is continued by the other router, and it keeps on increasing the number of hops until infinity (15 is the highest number of hops in RIP, so 16 is infinity).
```

* RIP mechanisms to solve ```Counting to Infinity```:

  * Split Horizon - according to this, the router cannot send routing information back in the direction from which it was received; this can help in avoiding routing loops.

  * Route Poisoning - when an interface/link is down, this info is spread to all other routers in the network by indicating the cost of the distance between the routers with the broken interface is infinity; therefore, these routers are considered poisoned. However, this method can increase the size of routing announcements.

## Routing Advanced

* Example entry in routing table:

  ```R  192.168.30.0/24 [120/1] via 192.168.20.1, 00:00:01, FastEthernet0/1```

  ```markdown
  'R' denotes a RIP route for 192.168.30.0/24, connected to the IP via 192.168.20.1.

  [120/1] denotes 120 AD and 1 hop (metric) between IP and route.
  ```

* In routing table, '*' denotes candidate default entry or a default static route; this defines where packets will be sent if no specific route for the destination network is listed in the routing table.

* If no default route is set, the router discards all packets with destination addresses not found in its routing table.

* Types of static routes:

  * Default route
  * Network route
  * Host route
  * Summary route
  * Floating route

* Inter-VLAN routing - way to forward traffic between different VLANs by implementing a router in the network; can be achieved using:

  * Traditional inter-VLAN routing:

    * A router is usually connected to the switch using multiple interfaces (one for each VLAN).

    * Router interfaces are configured as default gateways for the VLANs; and switch ports connected to router are in access mode.

    * When user node sends message to user in different VLAN, the message moves to the access port that connects to the router on their VLAN. The router examines the packet's destination IP and forwards it to the correct network using access port; now the switch can forward the frame to destination node since router changed VLAN info from source to destination VLAN.

    * In this form, router needs to have as many LAN interfaces as the number of VLANs.

  * Router-on-a-stick:

    * In this, router is connected to the switch using a single interface; switchport connecting to router is configured as trunk link.

    * The single interface is configured with many IPs corresponding to VLANs on switch; this interface accepts traffic from all VLANs and determines destination network based on packet headers, and then forwards data to switch with correct VLAN info.

    * On router, the physical interface is divided into smaller interfaces called subinterfaces; when it receives the tagged traffic, it forwards the traffic out to the subinterface with the destination IP.

    * Configuration:

    ```shell
    #on switch, define interface connected to router as trunk link
    #in global config mode
    int f0/1

    switchport mode trunk

    #create subinterface in router
    #global config mode
    int f0/0.10
    #subinterface for VLAN10

    #in subinterface config mode
    #link VLAN id with subinterface
    encapsulation dot1q 10

    #assign ip (default gateway) and subnet mask for VLAN 10
    ip address 192.168.10.1 255.255.255.0

    #config remaining subinterfaces for respective VLANs

    int f0/0

    no sh
    #activates VLAN interfaces
    ```

## IPv6

* While IPv4 is a 32-bit address, IPv6 is a 128-bit address; NAT used to conserve the number of IPv4 addresses required.

* IPv6 also allows a single interface to have multiple addresses, unlike IPv4.

* IPv6 format:

  * Zeros groups can be abbreviated with double colon, only once per IP
  * Leading zeros can be omitted
  * For example, ```FE80:0000:0000:0000:5400:04FF:FEDD:53F8``` can be also written as ```FE80::5400:4FF:FEDD:53F8```

* APIPA (Automatic Private IP Addressing) - enables computers (DHCP clients) to auto-configure an IP address and subnet mask when DHCP server is unreachable; IP format is 169.254.x.x and subnet mask is 255.255.0.0

* IPv4 header vs IPv6 header:

  ![Comparison of IPv4 and IPv6 headers](Images/ipv4-ipv6-headers.png)

* Types of IPv6 addresses:

  * Unicast - one-to-one
  * Multicast - one-to-many (range is FF00::/8)
  * Anycast - one-to-closest

* Types of unicast addresses:

  * Global address - similar to public IPv4 address; assigned range of 2001::/16, and initial 3 bits cannot be changed.

  * Link-local address - private IP (not routable), used for addressing a single link; range is FE80::/10, and it is auto-generated when IPv6 is enabled.

  * Unique local address - routable except on (public) Internet, similar to private IPv4 address; range is FC00::/7

* IPv6 configuration types:

  * Auto link local (EUI-64, identified using FFFE bit in address)
  * Manual link local
  * Auto IPv6 address
  * Manual IPv6 address

* IPv6 config:

```shell
#in router, global config mode

int g0/0

ipv6 address 2001::1/64
#slash notation accepted for ipv6

no sh

exit

sh ipv6 int br
#view ipv6 info
#we can add multiple ipv6 addresses for same interface

int g0/0

ipv6 address FE80::1 link-local
#add link-local address manually, this does not accept slash notation
#this replaces auto link-local address

#for auto ipv6 address in eui-64 form
ipv6 address 2010::/64 eui-64

no sh
```

## ACL

* ACL (Access Control List) - list of ```Permit```/```Deny``` statements for movement of data from network layer (layer 3) and above; applied in routers.

* Types of ACLs:

  * Standard ACL:

    * Identification no. from 1-99 or 1300-1999
    * Named version
    * Classify traffic based on source IP
  
  * Extended ACL:

    * Identication no. from 100-199 or 2000-2699
    * Named version
    * Classify traffic based on source IP, destination IP, protocol and port no.

* ACL config guidelines:

  * List of conditions
  * Possible actions - Permit, Deny
  * Packet compared to list from top to bottom
  * Once match is found, there are no more comparisons
  * Every ACL has implicit deny (Deny Any) at end, if ACL is non-empty
  * Standard ACL applied close to destination
  * Extended ACL applied close to source

* Standard ACL syntax:

  * Classic syntax:

  ```shell
  #in global config mode
  access-list <acl-no> <deny/permit> <matching params>

  int f0/0

  #apply ACL
  ip access-group <acl-no/name> <in/out>
  ```

  * Modern syntax:

  ```shell
  #in global config mode
  ip access-list standard <acl-no/name>

  <deny/permit> <matching params>

  #apply ACL in same way as before
  ```

* Wildcard masks:

  ```markdown
  Given IP - 192.168.100.225/24
  Subnet mask - 255.255.255.255.0
  So, wildcard mask - 0.0.0.255 (subtract subnet mask from 255.255.255.255)
  ```

  ```markdown
  Given IP - 192.168.100.225/26
  Subnet mask - 255.255.255.192
  Wildcard mask - 0.0.0.63
  ```

  ```markdown
  Identify hosts with even number in 4th octet
  Given IP - 192.168.100.0
  Subnet mask - 255.255.255.0
  Wildcard mask - 0.0.0.254 (last binary bit is 0 to allow only even numbers)
  ```

* Extended ACL syntax:

  * Classic syntax:

  ```shell
  #in global config mode
  access-list <acl-no> <deny/permit> <protocol> <source ip> <wildcard mask> <protocol info> <destination ip> <wildcard mask> <protocol info>

  #apply acl
  int f0/0

  ip access-group <acl-no/name> <in/out>
  ```

  * Modern syntax:

  ```shell
  ip access-list extended <acl-no/name>

  <deny/permit> <protocol> <source ip> <wildcard mask> <protocol info> <destination ip> <wildcard mask> <protocol info>
  ```

## NAT & PAT

* Private and public IPs:

|           | **Private IP**                | **Public IP**                                                |
|-----------|-------------------------------|--------------------------------------------------------------|
| _Class A_ | 10.0.0.0 - 10.255.255.255     | 1.0.0.0 - 9.255.255.255<br>11.0.0.0 - 126.255.255.255        |
| _Class B_ | 172.16.0.0 - 172.31.255.255   | 128.0.0.0 - 172.15.255.255<br>172.32.0.0 - 191.255.255.255   |
| _Class C_ | 192.168.0.0 - 192.168.255.255 | 192.0.0.0 - 192.167.255.255<br>192.169.0.0 - 223.255.255.255 |

* NAT (Network Address Translation) - translates private IPs to public IPs.

* Key terms:

  * Inside local - private IP address in internal network

  * Inside global - public IP address that can be used by host in internal network to access Internet

  * Outside global - public IP address for a device on Internet

  * Outside local - local IP address for any external network

* Types of NAT:

  * Static NAT - one-to-one mapping of inside local (private) address to inside global (public) address

  * Dynamic NAT - pool of IP addresses used but no static mapping; translation timeout

  * PAT (Port Address Translation) - NAT overload; maps inside local address' random source ports to random destination ports of same inside global address.

## Troubleshooting Techniques

* Three-layer hierarchical model - developed by Cisco for a reliable network infra with reduced complexity; the three layers are:

  * Access:
  
    * Controls user, workgroup access to network resources
    * Includes Layer 2 switches and APs (Access Points) for connectivity
    * At this layer, we can manage access control & policy, and implement port security

  * Distribution:

    * Communication point between access & core layer
    * Consists of routers & multilayer switches
    * Provides routing, filtering and WAN access

  * Core:

    * Network backbone, responsible for processing high traffic
    * Provides interconnectivity between distribution layer devices
    * Includes high speed devices like high-end routers & switches with redundant links

* Collapsed Core Architecture - combines the core & distribution layers; core & distribution functions implemented by distribution layer devices.

  * Pros - lower costs, simplified network
  * Cons - less scalable, less resiliency, low redundancy

* Firewalls - used for packet filtering; creates inside zone (most secure - 100) and outside zone (least secure - 0); traffic from high to low security always allowed, while traffic from low to high security always dropped.

* Troubleshooting commands:

  * ```ipconfig```
  * ```nslookup```
  * ```ping```
  * ```tracert```

## CDP, Syslog & NTP

* By default, logging messages cannot be viewed on SSH & telnet as it is enabled only for console line.

* To view logging messages, we need to use the command ```terminal monitor```; to disable it - ```terminal no monitor```.

* Also, to prevent syslog messages from interrupting commands while being typed, we can use the command ```logging synchronous```.

* Severity levels (for syslog messages):

  * 0 - emergency
  * 1 - alert
  * 2 - critical
  * 3 - error
  * 4 - warning
  * 5 - notification
  * 6 - informal
  * 7 - debug

* To instruct a device to send logs to a syslog server, we can use ```logging 10.0.0.10```, where 10.0.0.10 is the IP of the server (with syslog service enabled).

* While logging, we can also use the commands ```service sequence numbers``` and ```service timestamps log``` to log messages with sequence numbers and timestamps, respectively.

* NTP (Network Time Protocol) - application layer protocol used for clock syncing between hosts; it uses a client-server architecture:

```shell
#config router as NTP client
ntp server 192.168.0.100
#where 192.168.0.100 is IP address of NTP server

show ntp status
#verify NTP

#config router as NTP server
ntp master
```

* CDP (Cisco Discovery Protocol) - proprietary protocol used to discover info about neighboring Cisco devices; enabled by default.

```shell
#display info about directly connected devices
show cdp neighbors

#more info
show cdp neighbors detail

#enable CDP globally
cdp run

#enable CDP on specific interface
cdp enable
```

* LLDP (Link Layer Discovery Protocol) - vendor-neutral (IEEE-defined) data link layer protocol; alternative to CDP and can be used for non-Cisco devices.

```shell
#enable LLDP globally
#disabled by default on Cisco devices
lldp run

#display info about neighbors
show lldp neighbors

#more info
show lldp neighbors detail

#display global info
show lldp

#enable receiving/transmitting on interface
lldp transmit

lldp receive
#disable using 'no' commands
```

## Password Reset & Licensing

* Router password recovery:

  1. Bouncing the router (switching off and bringing it back on)
  1. Prevent IOS load at beginning using Ctrl+C (break sequence) to enter 'rommon' mode
  1. Change configuration-register using '0x2142' (ignore NVRAM content)
  1. Reload router using ```reset```
  1. Load startup-config into running-config, then change password
  1. Change configuration-register back to '0x2102'
  1. Save config using ```write```

* Switch password recovery:

  1. Press and hold 'MODE' button for 3 seconds
  2. Rename 'flash:config.txt'
  3. Reboot switch

* IOS backup/recovery:

  * Backup of current IOS image file - ```copy flash tftp```
  * Load a new IOS image file from TFTP - ```copy tftp://<path of .bin> flash:```
  * Restore IOS image file in 'rommon' mode - ```tftpdnld ?```

* In Cisco, before IOS version 15.0, software image was selected based on customer requirements; with IOS 15.0, there is one universal image.

## STP

* Problems caused by L2 (layer 2) loop:

  * Broadcast storm
  * MAC table instability
  * Multiple copies of frame

* STP (Spanning Tree Protocol):

  * Network protocol designed to prevent L2 loops

  * Standardized as IEEE 802.D protocol

  * In loop, STP places some ports in a blocking state; these ports will no longer process/receive any frames except STP messages

  * Enables L2 redundancy

  * Uses spanning tree algorithm (STA) to prevent loops

  * ON by default in Cisco switches

* STP criteria:

  * BPDU (Bridge Protocol Data Units) are sent by switches among themselves

  * A root switch is elected - all its working ports are placed in forwarding state

  * All other switches (non-root switches) determine best path to get to root switch

  * Port used to reach root switch (root port) is placed in forwarding state

  * On shared Ethernet segments, the switch with best path to reach root switch is in forwarding - this switch is the designated switch and its port is the designated port

  * All other ports are in blocking state & won't forward frames

* STP ports:

  * Root ports - primarily used for forwarding frames in STP; all ports connecting to root switch on neighboring switches are root ports and will be in forwarding state

  * Designated ports - ports on root switch and one port on other links that are not directly connected to root switch; these are also in forwarding state

  * Non-designated ports - redundant, blocked ports; active only in case of failure

* BPDUs:

  * Messages to share STP info between switches
  * Used in root switch election & loop detection
  * A common BPDU is ```Hello BPDU```, sent every 2 seconds (includes root ID, switch ID, root cost, Hello, Max Age and forward delay timers)

* STP convergence steps:

  * Election of root bridge
  * Election of root ports
  * Elect designated & non-designated ports

* Root switch election:

  * Election based on BIDs (bridge IDs) sent in BPDUs

  * Each participating switch has a 8-byte switch ID - this includes a 2-byte priority field (default value 32768) and a 6-byte system ID (based on MAC address)

  * By default, a switch always believes it is root, until it receives a superior BPDU

  * Switch with lowest BID becomes root switch

  * STA adds VLAN ID to priority value (4-bit priority plus 12-bit VLAN ID or extended system ID), and by default all switchports are in VLAN 1 - so BID is 32769

  * Config custom BID priority using ```spanning-tree vlan <ID> priority <VALUE>```, where value must be in multiples of 4096

* Root switch election example:

![Before root switch election](Images/pre-root-election.jpg)

```markdown
BPDUs are broadcasted by switches in election process.

The switches compare their own info with the BPDU info to find root switch.

DS1 is identified as root switch as it has a lower MAC address and lower priority.

By STA, all ports leading to DS1 on neighbors are root ports and all ports on DS1 are designated ports.

Now, between DS2 and DS3, the port on DS2 offers a lower BPDU, and therefore it will be the designated port; the DS3 port will be blocked.
```

![After root switch election](Images/post-root-election.jpg)

* Root switch optimization:

  * Older switches usually have lower MAC addresses, thus higher chances of getting elected as root switch

  * Old switch as root switch will cause poor network operation

  * To solve this, manual configuration of root switch is required

  * Using ```root primary``` command, we can set root switch with priority 24576

  * Using ```root secondary```, we can set backup root switch with priority 28672

* On shared Ethernet segments, if there is a tie in best root cost, these criteria are used (the switch/port with best path is called designated switch/port):

  * Lowest neighbor bridge ID
  * Lowest neighbor port priority (128 by default)
  * Lowest neighbor internal port number

* Default port cost (can be overridden) depends on port speed:

| Speed    | Cost |
|----------|------|
| 10 Mbps  | 100  |
| 100 Mbps | 19   |
| 1 Gbps   | 4    |
| 10 Gbps  | 2    |

* STP port states and time spent in each state:

  * Blocking - 20 seconds (10 * Hello timer duration)
  * Listening (sends & receives BPDUs until elected) - 15 seconds
  * Learning (preparing MAC address table) - 15 seconds
  * Forwarding (forwards frames as usual)
  * Disabled (administratively shutdown)

* STP timers and default transmission time:

  * Hello timer - 2 seconds (checks if link is up)
  * Forward delay - 15 seconds (listening, learning)
  * Max Age - 20 seconds (max time that port can save BPDU info)

* BPDUs exchanged when building topology database:

  * Configuration BPDUs - to elect root switches, root & designated ports

  * TCN (Topology Change Notification) BPDUs - when port transitions into forwarding, or when forwarding/learning transitions into blocking/down.

  * TCA (Topology Change Ack) BPDUs - confirm receiving TCN

* All modes of STP:

| **Encapsulation** | **802.1q** | **ISL** | **Both** |
|-------------------|------------|---------|----------|
| _Normal_          | STP        | PVSTP   | PVSTP+   |
| _Rapid_           | RSTP       | RPVSTP  | RPVSTP+  |

## RSTP

* RSTP (Rapid STP):

  * Faster version of STP
  * IEEE 802.1w
  * Backwards-compatible with STP

* Similarities between STP & RSTP:

  * Root switch elected using same set of rules
  * Root ports and designated ports selected with same rules
  * Both place each port in either forwarding or blocking state; in RSTP, blocking state is called discarding state

* Differences between STP & RSTP:

  * RSTP has faster convergence
  * STP port states - listening, blocking and disabled - merged into a single state in RSTP, called discarding state
  * RSTP has 4 port types - root, designated, alternate and backup port (applied when single switch has two links to same segment, or attached to hub)
  * In STP, root switch generates & sends Hellos to other switches, and relayed by non-root switches; in RSTP, each switch can generate its own Hellos

* For example, in STP, to avoid loops it places one port in blocking state. In RSTP, that port is placed in alternate state, which won't process/forward any frames except RSTP messages; and if one root port fails, alternate port becomes root port.

* Newer switches use RSTP by default; if not used, it can be enabled with ```spanning-tree mode rapid-pvst```

* In RSTP, alternate & backup ports are in discarding state.

* RSTP port states:

  * Discarding (blocking, listening and disabled combined)
  * Learning
  * Forwarding

## PortFast and BPDU Guard

* PortFast - feature that enables switch to instantaneously transition from blocking/discarding to forwarding state, skipping listening & learning state.

* PortFast is recommended only on non-trunking access ports like edge ports as these ports do not send/receive BPDUs.

* As PortFast can be enabled on non-trunking ports connecting switches, L2 loops can still occur due to transmission of BPDUs.

* This can be prevented by enabling BPDU Guard; this moves non-trunking switch ports into an ```errdisable``` state when BPDU is accepted on that port.

* By enabling BPDU Guard, STP shuts down interfaces with PortFast which received BPDUs, instead of putting them in blocking state.

* Root guard - feature that helps in enforcing position of root switch in network.

## Other STP Modes

* PVST (Per VLAN Spanning Tree):

  * Cisco-proprietary, uses only ISL
  * Used for topologies involving VLANs
  * Can create different root switches for different VLANs
  * Should not keep same root switch for multiple VLANs as the same interface will be blocked in both VLANs
  * Adds VLAN number to BID of switch

* RPVST (Rapid PVST):

  * Cisco-proprietary, uses only ISL
  * Creates one STP instance per VLAN

* MSTP (Multiple STP):

  * Open standard (IEEE 802.1q)
  * Allows to create multiple separate spanning-tree instances
  * Maps multiple VLANs to an instance
  * Resource-saving due to grouping, compared to PVST+
  * When MSTP region is connected to PVST+ topology, MST simulates PVST+ with a feature 'PVST simulation mechanism'

* PVST+:

  * Cisco-proprietary
  * Improved version of PVST
  * Supports both ISL and 802.1q
  * Maps a VLAN for each spanning-tree instance

* RPVST+:

  * Cisco-proprietary
  * Improved version of RPVST
  * Supports both ISL and 802.1q
  * Faster network convergence
  * Like PVST+, every VLAN has single instance of spanning tree

## EtherChannel

* EtherChannel:

  * link aggregation
  * combines multiple links between switches into one logical link (e.g. - three 100Mbps links into one 300 Mbps link)
  * can assign upto 16 physical interfaces to an EtherChannel, but only 8 can be active at a time
  * all interfaces should have same duplex, same speed, same VLAN config and same switchport modes
  * benefits - increased bandwidth, redundancy and load balancing

* Types of EtherChannels:

  * Static - does not check any parameters; EtherChannel ON mode
  * Port Aggregation Protocol (PAgP) - Cisco-proprietary - can combine max 8 physical links into single virtual link
  * Link Aggregation Control Protocol (LACP) - IEEE standard - can combine upto 8 ports that can be active and another 8 that can be in standby

* PAgP and LACP are not interoperable.

```shell
#in switch, int config mode

channel-group 1 mode on
#static EtherChannel
#channel-group no. between 1-6

#for all switchports, same channel-group no.
#for another linked switch, different no. can be used, but EtherChannel should be enabled

channel-group 1 mode <desirable/auto>
#PAgP
#if both ports between switches in auto mode, no EtherChannel formed
#in switch, all ports should be in same mdoe

channel-group 1 mode <active/passive>
#LACP
#if both ports in passive mode, no EtherChannel formed

#verification
show etherchannel 1 port-channel
#where 1 is the channel-group no.

show etherchannel summary
```

* To switch from LACP to PAgP, disable LACP first and then enable PAgP; or create another ```channel-group```.

* On high availability networks, use static mode over PAgP or LACP as it is more reliable and faster.

* EtherChannel & load balancing:

  ```shell
  #load balancing happens on source MAC by default.
  channel-group 1 mode <option>

  #in global config mode
  port-channel load-balance <option>

  #in priv exec mode
  show etherchannel load-balance
  ```

* In the case of multiple Access layer (L2) switches connected to a few Distribution layer (L3) switches, bandwidth is wasted due to STP blocking ports on L2 switches; to solve this, we can use switch stacking or chassis aggregation.

* Switch stacking:

  * Combines multiple switches into one logical switch
  * One switch acts as Master, via which we can config other switches (e.g. - Flex stack module)
  * Only switches with stacking module or modular slot can be used
  * Creates logical Access layer switch in ring formation
  * We can create EtherChannel between Access and Distribution layer switches as well, reducing overhead

* Chassis aggregation:

  * Combines switches into a single logical switch
  * Does not require external modules unlike switch stacking
  * Usually in Distribution or Core layer

## EIGRP

* EIGRP (Enhanced Interior Gateway Routing Protocol):

  * advanced distance vector routing protocol
  * supports large networks
  * quick convergence times
  * supports bounded updates (network topology change updates only sent to routers affected by change)
  * messages sent using multicast
  * performs equal cost load balancing on upto 4 paths by default and can be increased upto 16 paths
  * only protocol that can be configured to perform unequal cost load balancing

* EIGRP config:

```shell
router eigrp 100
#here, 100 is AS - Autonomous System
#routers with same AS number can peer

network 10.0.0.0 0.0.255.255
#uses wildcard mask
#if no wildcard mask used, defaults to classful boundary wildcard mask
```

* The ```network``` command indicates to look for interfaces with IP in that range & enable EIGRP on those interfaces - so it will send/listen for EIGRP Hello messages and form adjacencies; it will also advertise network config on those interfaces.

* For example, ```network 10.0.0.0 0.0.255.255``` will be advertising networks like 10.0.1.1/24 and 10.0.2.1/24, but it would not be advertising 10.1.0.1/24 or 10.0.0.0/16

* EIGRP verification:

```shell
sh run | section eigrp

sh ip protocols

sh ip eigrp interfaces

sh ip eigrp neighbors

sh ip route
```

* EIGRP Router ID:

  * routers identify themselves using router ID, in form of an IP address
  * by default, highest IP of any loopback interfaces configured on router, or highest IP if loopback does not exist
  * router ID will never change as loopback does not go down
  * best practice to use loopback or set router ID manually

## OSPF

* OSPF (Open Shortest Path First):

  * link state routing protocol
  * each router describes itself & its interfaces to its directly connected neighbors; info passed unchanged between routers
  * supports large networks
  * very fast convergence time
  * messages sent using multicast
  * open standard protocol
  * uses Dijkstra's Shortest Path First algo to find best path to learned networks
  * uses LSA (link state advertisements) to pass on routing updates

* OSPF operations:

  * discover neighbors
  * form adjacencies
  * flood link state database (LSDB)
  * compute shortest path
  * install best routes in routing table
  * respond to network changes

* OSPF packet types:

  * Hello - router sends/listens for Hello packets when OSPF is enabled on interface, and forms adjacencies with other OSPF routers
  * DBD (Database Description) - adjacent routers will share their known networks with the DBD packet
  * LSR (Link State Request) - if router is missing info about any of the networks in received DBD, it will send the neighbor an LSR
  * LSA (Link State Advertisement) - routing update, sent as a reply to LSR
  * LSU (Link State Update) - contains list of LSAs to be updated, used during flooding
  * LSAck - receiving routers acknowledging LSAs

* OSPF config:

```shell
router ospf 1
#process ID 1
#process ID is locally significant only

network 10.0.0.0 0.0.0.255 area 0
#for learning network 10.0.0.0/24

network 10.1.0.0 0.0.0.255 area 0
#for learning network 10.1.0.0/24
```

* OSPF verification:

```shell
sh run | section ospf

sh ip protocols

sh ip ospf int br

sh ip ospf neighbors
#check adjacencies

sh ip ospf database
#check LSDB

sh ip route
#check OSPF routes
```

* In OSPF, similar to EIGRP, router ID is used to identify routers; best practice is to use loopback or set router ID manually.

* OSPF areas:

  * need for areas - too many routes can lead to memory consumption; network changes and reconverging can lead to high usage of processing resources

  * OSPF follows a hierarchical design by segmenting large networks into smaller areas

  * each router maintains full info about its own area, but only summary info about other areas

  * 2-level hierarchy used:

    * transit area - backbone, area 0 - does not contain end-users
    * regular areas - non-backbone areas - to connect end-users to transit area
  
  * for router to form adjacency, its neighbor must be in same area

  * types of OSPF routes:

    * intra area (routes received from other routers in same area)
    * inter area
    * external (redistributed into OSPF)

* OSPF router types:

  * Backbone routers:

    * routers with all their OSPF interfaces in area 0
    * routers maintain full LSDB of other routers & links in own area
  
  * ABR (Area Border Routers):

    * routers with interfaces in multiple areas
    * separates LSA flooding zones
    * primary point for area address summarization
    * functions as source for default routes
    * ABRs do not automatically summarize
  
  * Normal area routers:

    * routers with all their OSPF interfaces in a non-backbone area
    * learn inter-area routers to other areas from ABRs
  
  * ASBRs (Autonomous System Boundary Routers):

    * routers which redistribute into OSPF
    * might also run other routing protocols

## HSRP

* FHRP (First Hop Redundancy Protocols):

  * uses virtual IP (VIP) and MAC address to allow for automated gateway failover
  * hosts uses VIP as default gateway
  * if physical gateway fails, another gateway takes over
  * protocols:

    * HSRP (Hot Standby Router Protocol) - Cisco proprietary; deployed in active/standby pair
    * VRRP (Virtual Router Redundancy Protocol) - open standard; deployed in active/standby pair, similar to HSRP
    * GLBP (Gateway Load Balancing Protocol) - Cisco proprietary; supports active/active load balancing across multiple routers

* HSRP operations:

  * both routers have normal physical IP and MAC on HSRP interface; unique address used on both routers
  * both have HSRP VIP and MAC configured on interface; same VIP shared
  * when they come online, one is elected HSRP active router, and the other one is standby
  * active router owns VIP and MAC, and responds to ARP requests
  * all traffic for VIP goes through active router
  * routers send Hello messages to each over their HSRP interface
  * if standby router stops receiving Hellos, it will transition to be active router

* HSRP config:

```shell
#R1
int g0/1
ip address 10.10.10.2 255.255.255.0
no sh
standby 1 ip 10.10.10.1 (matching config on other router)

#R2
int g0/1
ip address 10.10.10.3 255.255.255.0
no sh
standby 1 ip 10.10.10.1

end
#for verification
show standby
```

## QoS

* Quality requirements for voice & SD viceo packets (one-way):

  * latency (delay) <= 150ms
  * jitter (variation in delay) <= 30ms
  * loss <= 1%

* Congestion:

  * occurs when traffic comes in quicker compared to traffic leaving device
  * in this case, packets sent in FIFO order
  * causes delay as packets wait in queue
  * as queue size changes, it causes jitter
  * if queue size limit exceeded, further packets are dropped
  * to mitigate congestion, either add bandwidth or use QoS (Quality of Service)

* Effects of QoS queuing:

  * reduces latency, jitter, loss for particular traffic
  * gives each type of traffic the service it requires
  * for mitigating temporary periods of congestion

* Classification & marking - for router/switch to give particular level of service to traffic type, it has to recognise traffic first; common ways to do so:

  * CoS (Class of Service):

    * L2 marking
    * 3-bit field in L2 802.1q frame header which carries CoS QoS marking
    * value of 0-7 can be set; default value 0 (best effort traffic)
    * CoS 6,7 reserved for network use
    * IP phones mark call signalling traffic as CoS 3 and voice payload as CoS 5
  
  * DSCP (Differentiated Service Code Point):

    * L3 marking
    * ToS (Type of Service) byte in L3 IP header used to carry DSCP QoS marking
    * 6 bits (64 values) are used; default value 0 (best effort traffic)
    * IP phones mark call signalling traffic as 24 (CS3, its DSCP code) and voice payload as 46 (EF)
    * standard markings for other traffic types such as 26 (AF31) for mission critical data and 34 (AF41) for SD video

  * ACL (Access Control List) - L3, L4 info used to recognise traffic

  * NBAR (Network Based Application Recognition) - L3 to L7 info used to recognise traffic

## Network Automation & Programmability

* Data serialization:

  * process of converting structured data to standardized format that allows sharing/storage of data in a form that allows recovering original structure
  * for transferring data between different systems, apps, languages
  * XML, JSON, YAML - human & machine readable, plaintext data encoding formats

* API (Application Programming Interfaces):

  * for one program to communicate directly with another program
  * to perform CRUD operations (Create Read Update Delete)
  * two main types of APIs for web services are SOAP and REST
  * NETCONF & RESTCONF are APIs for network devices

* SOAP (Simple Object Access Protocol):

  * standard communication protocol system that permits processes using different OS to communicate
  * transport is HTTP(S), and data format is XML
  * stricter requirements

* REST (Representational State Transfer):

  * architecture, not protocol
  * gives guidelines for structure & organization of an API
  * supports any transport & data format, typically HTTP(S) & JSON used
  * faster performance and easier than SOAP

* REST constraints:

  * client-server architecture
  * uniform interface
  * statelessness
  * cacheability
  * layered system (intermediary devices like load balancers must be transparent to client & server)
  * code on demand (optional)

* YANG (Yet Another Next Generation):

  * open standard defined by IETF (2010)
  * data modelling language which provides a standardized way to represent operational & config data of a network device
  * can used both internally & when packaged for transmission

* NETCONF communications:

  * NETCONF & YANG provide standard way to programmatically inspect & modify config of a network device
  * NETCONF (IETF 2006) is protocol that remotely reads/applies changes to device data
  * XML encoding used, and transport over SSH or TLS

* RESTCONF:

  * RESTCONF (IETF 2017) describes how to map YANG specification to a RESTful interface
  * uses HTTP verbs over REST API
  * simpler than NETCONF
  * XML/JSON encoding over HTTP(S) transport

* Config Management Tools:

  * designed to make administrating devices easy and automated
  * can automate provisioning & deployment of servers & network devices
  * includes version control, testing, etc.
  * common options (open source & free) - Ansible, Puppet, Chef

* Ansible:

  * Python required
  * agentless (no installation on devices to be managed)
  * push model (push config to devices to be managed)
  * communicates over SSH by default
  * simpler
  * released in 2012
  * Ansible modules are pre-built Python scripts
  * Ansible inventory files define all hosts that will be managed by control workstation
  * Ansible playbooks are YAML files that outline instructions required to run

* Puppet:

  * uses agent on target devices
  * 'Puppet Master' runs on Linux server
  * clients use TCP 8140 to comm with Puppet Master
  * pull model, agent checks in every 30 mins by default
  * written in Ruby
  * uses proprietary DSL, not YAML
  * comm over HTTPS via REST API
  * Manifest defines device properties
  * can check config consistency
  * created in 2005

* Chef:

  * agent to be installed on target devices
  * pull model
  * comm over HTTPS via REST API
  * written in Ruby
  * 'Cook books' and 'Recipes' used
  * released in 2009
  * Chef server uses TCP 10002 to send config to clients

## Task 1 - Router Configuration

![Task 1](Images/Task1.png)

* Power on router devices by turning on the switch in physical view

* Config IP address in routers:

  ```shell
  #router 1
  en; conf t
  int GigabitEthernet 0/0/0
  ip address 10.1.1.1 255.0.0.0
  no sh
  exit

  #router 2
  en; conf t
  int GigabitEthernet 0/0/0
  ip address 10.1.1.2 255.0.0.0
  no sh
  exit
  ```

* Router 1 can ping Router 2 using ```ping 10.1.1.2``` command.

* Configure enable password of 'cisco' in both routers:

  ```shell
  en; conf t
  enable password cisco
  ```

* Encrypt the enable password:

  ```shell
  en; conf t
  service password-encryption
  ```

* Configure secret password of 'cisco123':

  ```shell
  en; conf t
  enable secret cisco123
  ```

* Configure the first 5 telnet lines and use a line password of 'cisco' on them:

  ```shell
  en; conf t
  line vty 0 4
  password cisco
  login
  ```

* We can telnet from Router 1 to Router 2 using ```telnet 10.1.1.2```

* Configure console password of 'cisco':

  ```shell
  en; conf t
  line console 0
  password cisco
  login
  ```

## Task 2 - Router Switch Configuration

![Task 2](Images/Task2.png)

* First, build given topology using PCs, switches and router. Then connect according to the given config

* Now, we can config each PC by going to Config > Interface > FastEthernet0 > modify IPv4 address, subnet mask, and last octet of MAC address.

* Now, HostA and HostB can ping each other; and HostC and HostD can ping each other, but we need to configure the routers as well for HostA and HostD to communicate.

* Now, we can config Default Gateway - 192.168.10.1 for A, B; 192.168.20.1 for C, D - in the same way as we configured IPv4 address for PC

* Router config:

  ```shell
  en; conf t
  int G0/0/0
  ip address 192.168.10.1 255.255.255.0
  no sh
  int G0/0/1
  ip address 192.168.20.1 255.255.255.0
  no sh

  #now we can ping from HostA to HostD
  ```

## Task 3 - VLANs

![Task 3](Images/Task3.png)

* First, build the required topology and connect the components.

* In Switch, using the command ```en; sh vlan br```, we can see that all interfaces are part of VLAN1 (default VLAN).

* First config IP address for all 4 laptops. Then, create VLAN10 and VLAN20 on the switches and assign it to the required interfaces; and we have to assign trunk mode between two switches:

  ```shell
  #in Switch1

  en; conf t
  vlan 10
  vlan 20
  exit; exit
  sh vlan br

  #vlans have been created, assign it to required port now  
  
  int f0/1
  switchport mode access
  switchport access vlan 20
  int f0/2
  switchport mode access
  switchport access vlan 10
  int f0/3
  switchport mode trunk
  int f0/4
  switchport mode trunk
  exit; exit
  
  sh int trunk
  #shows trunk mode between two switches on f0/3 and f0/4

  #similarly, config other two switches
  ```

* Now, check if VLAN 10 devices can ping each other; and if VLAN 20 devices can ping each other; if everything is configured correctly, VLAN 10 device cannot ping VLAN 20 device.

## Task 4 - Router on a Stick

![Task 4](Images/Task4.png)

* First, assign IPs and DGs to the PCs by navigating to Desktop > IP Configuration

* Create VLANs in switch:

  ```shell
  en; conf t
  vlan 10; name VLAN10
  vlan 20; name VLAN20
  vlan 30; name VLAN30
  exit; exit
  sh vlan
  #shows VLANs created
  ```

* Config IPs for each VLAN in switch:

  ```shell
  en; conf t
  int vlan 10; ip add 192.168.10.2 255.255.255.0
  int vlan 20; ip add 192.168.20.2 255.255.255.0
  int vlan 30; ip add 192.168.30.2 255.255.255.0
  ```

* Assign interfaces to VLANs:

  ```shell
  en; conf t
  int f0/2; switchport mode access; switchport access vlan 10
  int f0/3; switchport mode access; switchport access vlan 20
  int f0/4; switchport mode access; switchport access vlan 30
  ```

* Config trunk port between switch and router:

  ```shell
  int f0/1; switchport mode trunk
  ```

* Config sub-interfaces in router:

  ```shell
  en; conf t
  int G0/0; no sh; exit
  #brings interface up
  #now add sub-int  
  int G0/0.10
  encap dot1Q 10
  ip address 192.168.10.1 255.255.255.0
  #similar for other sub-interfaces
  ```

* Now PCs are in different VLANs but can still ping each other.

## Task 5 - Static Routing

![Task 5](Images/Task5.png)

* View routing table in router using ```sh ip route```

* Currently, we cannot ping from PC0 to PC1 as route is not set properly, we need to setup static routing.

* In Router1:

  ```shell
  ip route 192.168.30.0 255.255.255.0 10.10.10.2
  #to route to 192.168.30.0 network, send packets to next hop at 10.10.10.2 
  
  ping 192.168.30.1
  #this works
  ```

* Similarly, in Router0:

  ```shell
  ip route 192.168.2.0 255.255.255.0 10.10.10.1
  ```

* Static route to be done from both ends, otherwise the ICMP packet won't be able to traverse back to source. After that, ping from PC0 to PC1 works.

## Task 6 - NAT and PAT

![Task 6](Images/Task6.png)

* For static NAT, first connect the required components; configure the IP and DG for the PCs given.

* Next, config both the routers for gateway:

  ```shell
  #in Router0
  en; conf t
  int G0/0/0
  ip add 192.168.10.1 255.255.255.0
  no sh
  int G0/0/1
  ip add 100.100.100.1 255.255.255.252
  no sh 
  
  #in Router1 (Internet)
  en; conf t
  int G0/0/0
  ip add 100.100.100.2 255.255.255.252
  no sh
  ```

* Now, we need to define inside/outside conditions on Router0 for static NAT:

  ```shell
  en; conf t
  int G0/0/1
  ip nat outside
  #as it is outside-facing
  
  int G0/0/0
  ip nat inside
  #as it is inside-facing
  
  #we also need to config static NAT in Router0
  #map private IP to public IP for one PC
  ip nat inside source static 192.168.10.10 100.100.100.1
  ```

* Now, we can ping from PC0 to Router1 (public); we can check in Router0 that NAT translations has taken place:

  ```shell
  en
  sh ip nat translations
  sh ip nat statistics
  ```

* However, static NAT uses one-to-one mapping, and we cannot map all PCs due to limited IP addresses; we can use NAT overload or PAT.

* Config for NAT overload (with same topology); config the routers accordingly, then config egress/ingress in Router0:

  ```shell
  en; conf t
  int G0/0/1
  ip nat outside
  int G0/0/0
  ip nat inside
  exit

  #define access-list
  access-list 1 permit 192.168.10.0 0.0.0.255
  
  #for NAT overload or PAT
  ip nat inside source list 1 interface G0/0/1 overload
  ```

* Now, we can ping from all PCs to the external router (Router1) 100.100.100.2

* This will use different ports with the IP associated with interface G0/0/1; we can check it using ```sh ip nat translations``` in Router0.

## Task 7 - IPv6 Routing

![Task 7](Images/Task7.png)

* First config IPv6 & gateway in PC by leaving IPv4 fields empty and static config IPv6 (no slash notation in gateway)

* Router0:

  ```shell
  en; conf t
  int g0/0/0
  ipv6 add 2001:11::10/64; no sh
  int g0/0/1
  ipv6 add 2001:120:1:1::1/64; no sh
  ```

* Router1:

  ```shell
  en; conf t
  int g0/0/0
  ipv6 add 2001:120:1:1::2/64; no sh
  int g0/0/1
  ipv6 add 2001:22::10/64; no sh
  ```

* We can now ping from Router0 to Router1

* Troubleshooting commands in router:

  ```shell
  sh ipv6 int br
  sh ipv6 route
  sh ipv6 int g0/0/0
  ```

* We need to setup routing now, to ping between PC0 and PC1.

* Static routing:

  ```shell
  #Router0, in global config mode
  ipv6 route 2001:22::0/64 2001:120:1:1::2
  #inform about 2001:22::0 network and next hop Router1
  
  ipv6 unicast-routing
  #required for static routing

  #Router1, in global config mode
  ipv6 route 2001:11::0/64 2001:120:1:1::1
  ipv6 unicast-routing
  ```

* Dynamic routing (rip-ng):

  ```shell
  #Router0, in global config mode
  ipv6 unicast-routing
  #enable routing first

  int g0/0/0
  ipv6 rip CCNA enable
  #same name to be used on the router, can use diff name for diff router
  
  int g0/0/1
  ipv6 rip CCNA enable

  #similarly for Router1

  #now we can ping from PC0 to PC1
  ```

## Task 8 - DNS & DHCP

![Task 8](Images/Task8.png)

* Create given layout, and configure IP, DG for servers and routers only; config DNS Server addr as well for both servers.

* Config such that router 'DHCP Server' is able to assign PCs IP addresses automatically if required; PCs should also be able to visit website at Web server using DNS server.

* Furthermore, Router1 should relay DHCP request from PC2 for getting IP from DHCP server.

* DHCP Server:

  ```shell
  #in global config
  #to create IP pool for 1st network
  ip dhcp pool NET1
  
  #exclude some address range
  ip dhcp excluded-address 192.168.1.1 192.168.1.10
  
  default-router 192.168.1.1
  
  #define network for which it is created
  network 192.168.1.0 255.255.255.0
  
  dns-server 192.168.1.2
  ```

* Now we can enable DNS service in DNS server, and add 'A Record' entry for name 'www.cisco.com' which will redirect to 192.168.1.3 (web server).

* Now, we can automatically config IP addresses via DHCP for PC0 and PC1; we can also navigate to 'www.cisco.com' from PC0 and PC1; we need PC2 to have similar functionality.

* DHCP Server:

  ```shell
  ip dhcp pool NET2
  network 192.168.3.0 255.255.255.0
  default-router 192.168.3.2
  dns-server 192.168.1.2
  ```

* We also need to enable RIP routing for comms

  ```shell
  #DHCP Server
  router rip
  ver 2
  network 192.168.1.0
  network 192.168.2.0

  #Router1
  router rip
  ver 2
  network 192.168.2.0
  network 192.168.3.0
  ```

* Config relay in Router1:

  ```shell
  int g0/0/1
  #ingress interface for Router1
  
  ip helper-address 192.168.2.1
  #Router1 will relay any request to this address now
  ```

* Now, PC2 can get DHCP address from DHCP server; we can also access the website.

## Task 9 - SNMP & Syslog

![Task 9](Images/Task9.jpeg)

* In R1:

  ```shell
  en; conf t
  #config SNMP communities
  snmp-server community Password1 ro
  snmp-server community Password2 rw

  #enable logging
  logging 10.0.0.100
  logging trap debugging

  exit

  sh logging
  ```

* Verify Syslog is working by shutting down an interface on R1 and bringing it back up

* The logs can be viewed on 10.0.0.100 in Syslog, under the Services tab

## Task 10 - NTP

![Task 10](Images/Task10.jpeg)

* NTP is enabled on the server by default

* Before configuring, 'sh clock' on router shows inaccurate time

* If we were using a router instead of a server for NTP server, we can use ```ntp master``` to set that device as NTP server

* Configure both routers as NTP clients:

  ```shell
  clock timezone PST -8
  ntp server 10.0.0.50
  ```

* Verify NTP on both client routers:

  ```shell
  sh clock
  sh ntp status
  ```

## Task 11 - EtherChannel

![Task 11](Images/Task11.jpeg)

* We have to convert uplinks from Acc3 to CD1 and CD2 to LACP EtherChannel.

* Acc3:

  ```shell
  en; conf t
  int range f0/23-24
  channel-group 1 mode active
  
  exit
  
  int port-channel 1
  description Link to CD1
  switchport mode trunk
  switchport trunk native vlan 199
  #to config native VLAN on switchport
  ```

* Matching settings on CD1:

  ```shell
  en; conf t
  int range f0/23-24
  channel-group 1 mode active
  
  exit
  
  int port-channel 1
  desc Link to Acc3
  switchport mode trunk
  switchport trunk native vlan 199
  ```

* Similarly, create the EtherChannel config between Acc3 and CD2, setting both sides to ```active``` and using a different port channel number

* We can verify the EtherChannels come up using ```show etherchannel summary```

* For configuring PAgP instead, we need to use ```channel-group 1 mode desirable```

* And for configuring static EtherChannel, we can use ```channel-group 1 mode on```

## Task 12 - RIP & EIGRP

![Task 12](Images/Task12.jpeg)

* RIP config - enabling RIPv2 on every router, we have to advertise all networks except 203.0.113.0/24. run following command on all routers:

  ```shell
  router rip
  version 2
  no auto-summary
  network 10.0.0.0
  ```

* Verify using ```sh ip route```

* Now, for all routers to have route to 203.0.113.0/24, it must be added to RIP process on R4, and interface f1/1 facing Internet has to be configured as passive interface to avoid sending out internal info.

* On R4:

  ```shell
  router rip
  passive-interface f1/1
  network 203.0.113.0
  ```

* To configure a default static route on R4 to Internet via 203.0.113.2, use the command ```ip route 0.0.0.0 0.0.0.0 203.0.113.2```

* EIGRP config - enabling EIGRP on every router for AS 100:

  ```shell
  router eigrp 100
  network 10.0.0.0
  ```

* ```sh ip eigrp neighbors``` shows adjacencies formed with other routers.

* ```sh ip route``` shows EIGRP routes in routing table, and two RIP routes for Internet and default route - as EIGRP has lower AD than RIP

## Task 13 - OSPF

![Task 13](Images/Task13.jpeg)

* Enable loopback interface on R1 - R5 in the format 192.168.0.x/32 where x is the router no.:

  ```shell
  #on R1, for example
  int loopback0
  ip address 192.168.0.1 255.255.255.255
  ```

* Enable single area OSPF on R1 - R5 to advertise all networks except 203.0.113.0/24. On all routers:

  ```shell
  router ospf 1
  network 10.0.0.0 0.255.255.255 area 0
  network 192.168.0.0 0.0.0.255 area 0
  ```

* Use commands ```sh ip protocols```, ```sh ip ospf neighbor```, ```sh ip route``` for troubleshooting

* For advertising route to 203.0.113.0/24 from R4, without advertising internal routes to Internet, we have to configure it as passive interface. On R4:

  ```shell
  router ospf 1
  passive-interface f1/1
  network 203.0.113.0 0.0.0.255 area 0
  ```

* For default static route injection, on R4:

  ```shell
  ip route 0.0.0.0 0.0.0.0 203.0.113.2
  router ospf 1
  default-information originate
  ```

## Task 14 - HSRP

![Task 14](Images/Task14.jpeg)

* Configure basic HSRP for 10.10.10.0/24 network; on interface towards end-hosts.

* To configure Virtual IP on R1:

  ```shell
  int g0/1
  standby 1 ip 10.10.10.1
  ```

* Configure Virtual IP on R2:

  ```shell
  int g0/1
  standby 1 ip 10.10.10.1
  ```

* Check for HSRP, on R1 using the command ```show standby``` - this shows us the active & standby router.

* Configure HSRP so R1 will be preferred and active router always:

  ```shell
  int g0/1
  standby 1 priority 110
  #default priority is 100
  standby 1 preempt
  ```
