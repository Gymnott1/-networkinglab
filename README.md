![](fulllayout.png)

```

[admin@AGREGATOR_SWITCH] > export
# mar/24/2026 12:56:34 by RouterOS 7.6
# software id = 
#
/interface bridge
add fast-forward=no ingress-filtering=no name=OUT_BRIDGE vlan-filtering=yes
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no speed=1Gbps
set [ find default-name=ether5 ] disable-running-check=no
set [ find default-name=ether6 ] disable-running-check=no
set [ find default-name=ether7 ] disable-running-check=no
set [ find default-name=ether8 ] disable-running-check=no
set [ find default-name=ether9 ] disable-running-check=no
set [ find default-name=ether10 ] disable-running-check=no
set [ find default-name=ether11 ] disable-running-check=no
set [ find default-name=ether12 ] disable-running-check=no
set [ find default-name=ether13 ] disable-running-check=no
set [ find default-name=ether14 ] disable-running-check=no
set [ find default-name=ether15 ] disable-running-check=no
set [ find default-name=ether16 ] disable-running-check=no
set [ find default-name=ether17 ] disable-running-check=no
set [ find default-name=ether18 ] disable-running-check=no
set [ find default-name=ether19 ] disable-running-check=no
set [ find default-name=ether20 ] disable-running-check=no
set [ find default-name=ether21 ] disable-running-check=no
set [ find default-name=ether22 ] disable-running-check=no
set [ find default-name=ether23 ] disable-running-check=no
set [ find default-name=ether24 ] disable-running-check=no
set [ find default-name=ether25 ] disable-running-check=no
set [ find default-name=ether26 ] disable-running-check=no
set [ find default-name=ether27 ] disable-running-check=no
set [ find default-name=ether28 ] disable-running-check=no
/interface vlan
add interface=OUT_BRIDGE name=VLAN10_HS vlan-id=10
add interface=OUT_BRIDGE name=VLAN20_PPPOE vlan-id=20
/interface bonding
add comment="BONDING UPLINKETHER1_2" mode=802.3ad name=bonding1 slaves=\
    ether1,ether2 transmit-hash-policy=layer-3-and-4
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/port
set 0 name=serial0
/interface bridge port
add bridge=OUT_BRIDGE interface=ether3
add bridge=OUT_BRIDGE interface=ether4
add bridge=OUT_BRIDGE interface=ether5 pvid=20
/interface bridge vlan
add bridge=OUT_BRIDGE tagged=OUT_BRIDGE,ether4 vlan-ids=10
add bridge=OUT_BRIDGE tagged=OUT_BRIDGE,ether3 untagged=ether5 vlan-ids=20
/ip address
add address=102.0.6.133/30 comment=HS_STATIC_IP interface=VLAN10_HS network=\
    102.0.6.132
add address=102.203.85.5/28 comment=PPPOE_STATIC_IP interface=VLAN20_PPPOE \
    network=102.203.85.0
/ip dhcp-client
add interface=bonding1
/ip dns
set allow-remote-requests=yes servers=8.8.8.8
/ip firewall filter
add action=accept chain=forward
/ip firewall nat
add action=masquerade chain=srcnat out-interface=bonding1
/system identity
set name=AGREGATOR_SWITCH
/tool romon
set enabled=yes
[admin@AGREGATOR_SWITCH] > 
```
### setting the static ip to the server
```
echo "# -networkinglab" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:Gymnott1/-networkinglab.git
git push -u origin main
```
![](servergivenstaticip.png)
```
[admin@PPPOE_GNS3] > export
# mar/24/2026 12:57:03 by RouterOS 7.6
# software id = 
#
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
set [ find default-name=ether5 ] disable-running-check=no
set [ find default-name=ether6 ] disable-running-check=no
set [ find default-name=ether7 ] disable-running-check=no
set [ find default-name=ether8 ] disable-running-check=no
set [ find default-name=ether9 ] disable-running-check=no
/interface vlan
add interface=ether1 name=PPPOE_VLAN_50 vlan-id=50
add interface=ether9 name=VLAN20_UPLINK vlan-id=20
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip pool
add name=dhcp_pool0 ranges=172.16.0.2-172.16.255.254
/ip dhcp-server
add address-pool=dhcp_pool0 interface=PPPOE_VLAN_50 name=dhcp1
/port
set 0 name=serial0
/ppp profile
add dns-server=8.8.8.8,1.1.1.1 local-address=172.16.0.1 name=pppoe_profile \
    remote-address=dhcp_pool0
/interface pppoe-server server
add disabled=no interface=PPPOE_VLAN_50 service-name=pppoe_server
/ip address
add address=102.203.85.6/28 interface=VLAN20_UPLINK network=102.203.85.0
add address=172.16.0.1/16 interface=PPPOE_VLAN_50 network=172.16.0.0
/ip dhcp-server network
add address=172.16.0.0/16 gateway=172.16.0.1
/ip dns
set allow-remote-requests=yes servers=102.203.85.5
/ip firewall nat
add action=masquerade chain=srcnat out-interface=VLAN20_UPLINK
/ip route
add gateway=102.203.85.5
/ppp secret
add name=test1 profile=pppoe_profile
/system identity
set name=PPPOE_GNS3
/tool romon
set enabled=yes
[admin@PPPOE_GNS3] > 
```

```
[admin@HS_GNS3] > export
# mar/24/2026 12:57:27 by RouterOS 7.6
# software id = 
#
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
set [ find default-name=ether5 ] disable-running-check=no
set [ find default-name=ether6 ] disable-running-check=no
set [ find default-name=ether7 ] disable-running-check=no
set [ find default-name=ether8 ] disable-running-check=no
set [ find default-name=ether9 ] disable-running-check=no speed=1Gbps
/interface vlan
add interface=ether1 name=HS_VLAN100 vlan-id=100
add interface=ether9 name=VLAN10_UPLINK vlan-id=10
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip hotspot profile
add dns-name=gns.test hotspot-address=10.0.0.1 name=hsprof1
/ip pool
add name=dhcp_pool0 ranges=172.16.0.2-172.16.255.254
add name=dhcp_pool1 ranges=10.0.0.2-10.0.255.254
/ip dhcp-server
add address-pool=dhcp_pool1 interface=HS_VLAN100 name=dhcp1
/ip hotspot
add address-pool=dhcp_pool1 disabled=no interface=HS_VLAN100 name=hotspot1 \
    profile=hsprof1
/port
set 0 name=serial0
/ip address
add address=102.0.6.134/30 interface=VLAN10_UPLINK network=102.0.6.132
add address=10.0.0.1/16 interface=HS_VLAN100 network=10.0.0.0
/ip dhcp-server network
add address=10.0.0.0/16 gateway=10.0.0.1
add address=172.16.0.0/16 gateway=172.16.0.1
/ip dns
set allow-remote-requests=yes servers=102.0.6.133
/ip firewall filter
add action=passthrough chain=unused-hs-chain comment=\
    "place hotspot rules here" disabled=yes
/ip firewall nat
add action=passthrough chain=unused-hs-chain comment=\
    "place hotspot rules here" disabled=yes
add action=masquerade chain=srcnat out-interface=VLAN10_UPLINK
add action=masquerade chain=srcnat comment="masquerade hotspot network" \
    src-address=10.0.0.0/16
/ip hotspot user
add name=admin
/ip route
add check-gateway=ping disabled=no distance=1 dst-address=0.0.0.0/0 gateway=\
    102.0.6.133 pref-src="" routing-table=main scope=30 suppress-hw-offload=no \
    target-scope=10
/system identity
set name=HS_GNS3
/tool romon
set enabled=yes
[admin@HS_GNS3] > 
```

```
[admin@SWITCH_GNS3] > export
# mar/24/2026 12:57:50 by RouterOS 7.6
# software id = 
#
/interface bridge
add name=bridge1-trunk vlan-filtering=yes
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
set [ find default-name=ether5 ] disable-running-check=no
set [ find default-name=ether6 ] disable-running-check=no
set [ find default-name=ether7 ] disable-running-check=no
set [ find default-name=ether8 ] disable-running-check=no
set [ find default-name=ether9 ] disable-running-check=no
set [ find default-name=ether10 ] disable-running-check=no
set [ find default-name=ether11 ] disable-running-check=no
set [ find default-name=ether12 ] disable-running-check=no
set [ find default-name=ether13 ] disable-running-check=no
set [ find default-name=ether14 ] disable-running-check=no
set [ find default-name=ether15 ] disable-running-check=no
set [ find default-name=ether16 ] disable-running-check=no
set [ find default-name=ether17 ] disable-running-check=no
set [ find default-name=ether18 ] disable-running-check=no
set [ find default-name=ether19 ] disable-running-check=no
set [ find default-name=ether20 ] disable-running-check=no
set [ find default-name=ether21 ] disable-running-check=no
set [ find default-name=ether22 ] disable-running-check=no
set [ find default-name=ether23 ] disable-running-check=no
set [ find default-name=ether24 ] disable-running-check=no
set [ find default-name=ether25 ] disable-running-check=no
set [ find default-name=ether26 ] disable-running-check=no
set [ find default-name=ether27 ] disable-running-check=no
set [ find default-name=ether28 ] disable-running-check=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/port
set 0 name=serial0
/interface bridge port
add bridge=bridge1-trunk interface=ether1
add bridge=bridge1-trunk interface=ether5
add bridge=bridge1-trunk interface=ether6
/interface bridge vlan
add bridge=bridge1-trunk tagged=ether1,ether5,ether6 vlan-ids=50,100
/ip dhcp-client
# DHCP client can not run on slave or passthrough interface!
add interface=ether1
/system identity
set name=SWITCH_GNS3
/tool romon
set enabled=yes
[admin@SWITCH_GNS3] > 
```

```
[admin@ROUTING_ROUTER] > export
# mar/24/2026 12:58:14 by RouterOS 7.6
# software id = 
#
/interface bridge
add name=bridge-vlan vlan-filtering=yes
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
set [ find default-name=ether5 ] disable-running-check=no
set [ find default-name=ether6 ] disable-running-check=no
set [ find default-name=ether7 ] disable-running-check=no
set [ find default-name=ether8 ] disable-running-check=no
set [ find default-name=ether9 ] disable-running-check=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/port
set 0 name=serial0
/interface bridge port
add bridge=bridge-vlan interface=ether1 pvid=50
add bridge=bridge-vlan interface=ether9
add bridge=bridge-vlan interface=ether2 pvid=100
/interface bridge vlan
add bridge=bridge-vlan tagged=ether9 vlan-ids=50
add bridge=bridge-vlan tagged=ether9 vlan-ids=100
/ip dhcp-client
add interface=ether1
/ip dns
set allow-remote-requests=yes servers=8.8.8.8
/system identity
set name=ROUTING_ROUTER
/tool romon
set enabled=yes
[admin@ROUTING_ROUTER] > 
```
# -networkinglab


### captive portal hotspot setup
![](hotspotcaptivedisplayed.png)

![](hscaptiveautheticatestheuser.png)

## hotspot ip
![](usergivenhsip.png)

![](pppoegivenpppoeip.png)

