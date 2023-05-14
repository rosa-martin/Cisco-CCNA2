# Cisco-CCNA2 commands

## Basic configuration
Setting for enabling IPv6 

Use the following commands to assign the dual-ipv4-and-ipv6 template as the default SDM template. 

S1# configure terminal 

S1(config)# sdm prefer dual-ipv4-and-ipv6 default 

S1(config)# end 

S1# reload 

To enable IPv6 routing, enter the command ipv6 unicast-routing. 

R1(config)# ipv6 unicast-routing

## IP route

ip route [destination ip of network] [mask of ip of network] [next-hop address/outbounds interface of current router]

ip route [destination ip of device] 255.255.255.255 [next-hop address/outbounds interface of current router]

## Trunks
### Put PC to vlan

Set port to access mode - switchport mode access

Set the vlan - switchport access vlan [id of vlan]

### Set trunk mode

set int to trunk mode - switchport mode trunk

if you have switch 3rd layer - switchport trunk encapsulation dot1q

set native vlan for trunking - switchport trunk native vlan [id of vlan]

## Router-on-stick
Create subinterfaces on router (done on a interface g0/1):

Example for vlans 10, 99 (native)

int g0/1.10

encapsulation dot1q 10

ip address 192.168.10.1 255.255.255.0

int g0/1.99

encapsulation dot1q 99 native

int g0/1

no shutdown

## Routing via 3rd layer switch
enable ip routing (command: ip routing)

Create interface vlans

Example for vlans 10, 99 (native)

int vlan 10

ip address 192.168.10.1 255.255.255.0

no shutdown

Afterwards configure the used ports for trunking as shown above

For a port that is connected to a router use 'no switchport' before asigning it an IP address

## Ether-channel
example on ports f0/5, f0/6:

int range f0/5-6

channel-group [id of group] mode active

shutdown

exit

int port-channel [id of group]

switchport mode trunk

switchport trunk native vlan [id of native vlan]

exit

int range f0/5-6

no shutdown

Do this on both connected devices

## DHCPv4
Exclude addreses that are already used by other devices in the client's vlan

ip dhcp excluded-address [ip address]

Creat dhcp pool
ip dhcp pool [name of dhcp pool]

Specify the network range of ip addresses for the dhcp pool

network [network address] [mask]

Set the clients' default-router (it has the same address as their default-gateway)

default-router [defualt-gateway of the clients]

If you wish to show all assigned addresses - show ip dhcp binding

### DHCPv4 relay agent
Go to the interface (or subinteface if there are vlans in the network) where the defalut-gateway of clients is located

Example on subinterface g0/1.5:

int g0/1.5

ip helper-address [address of the router where the dhcp server is present]

Note: in the case the default gateway of the clients is on the relay agent router rahter than on the dhcp router, make sure it's set properly


## HSRP
In case there are vlans present in the network put these on the subinterface (vlan interface, in case of a L3-switch) that would normaly have default-gateway

Choose virtual ip (this will be the default gateway of the network)

standby [group number] ip [virtual ip]

standby [group number] priority [priority value] -> highest priority value = active router

standby [group number] preempt -> set on active router (it's better to put it on both)

## Security stuff

### SSH
Enabling SSH 

Disable DNS lookup. 

RTA(config)# no ip domain-lookup 

Set the domain name to CCNA.com (case-sensitive for scoring in PT). 

RTA(config)# ip domain-name CCNA.com 

Create a user of your choosing with a strong encrypted password. 

RTA(config)# username any_user secret any_password 

Generate 1024-bit RSA keys 

RTA(config)# crypto key generate rsa modulus 1024 

Block anyone for three minutes who fails to log in after four attempts within a two-minute period. 

RTA(config)# login block-for 180 attempts 4 within 120 

Configure all VTY lines for SSH access and use the local user profiles for authentication. 

RTA(config)# line vty 0 4 

RTA(config-line)# transport input ssh 

RTA(config-line)# login local 

Set the EXEC mode timeout to 6 minutes on the VTY lines. 

RTA(config-line)# exec-timeout 6

## Port security

port must be an access port or a trunk port - switchport mode access/trunk

switch(config-if)# switchport port-security

switch(config-if)# switchport port-security mac-address sticky -> in case of dynamic mac addresses

switch(config-if)# switchport port-security maximum 5 -> set max amount of MAC addresses

switch(config-if)# switchport port-security violation [shutdown | restrict | protect] 
  
  shutdown - vypne port
  
  restrict - zahadzuje pakety a spravi syslog incidentu
  
  protect - zahadzuje pakety





