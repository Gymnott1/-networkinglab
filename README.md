```

# GNS3 Network Lab

This project demonstrates a complex network topology using GNS3 and MikroTik RouterOS virtual devices. The lab includes multiple routers, switches, VLANs, PPPoE, Hotspot, and static IP assignments. Below you'll find a high-level overview, network layout, and configuration details for each device.

---

## Network Topology Overview

![Full Network Layout](fulllayout.png)
*Figure 1: Full GNS3 Lab Topology*

---

## Key Features

- Aggregator switch with VLAN trunking and bonding
- PPPoE server and client configuration
- Hotspot server with captive portal
- Multiple DHCP and static IP assignments
- Layer 2/3 switching and routing

---

## Screenshots & Diagrams

### Hotspot Captive Portal Display
![Hotspot Captive Portal](hotspotcaptivedisplayed.png)
*Figure 2: Hotspot captive portal as seen by users*

### Hotspot Authentication
![Hotspot Authentication](hscaptiveautheticatestheuser.png)
*Figure 3: Hotspot authenticates the user*

### PPPoE Assigned IP
![PPPoE Assigned IP](pppoegivenpppoeip.png)
*Figure 4: PPPoE client receives IP address*

### Static IP Assignment by Server
![Server Static IP](servergivenstaticip.png)
*Figure 5: Server assigns static IP to client*

### Hotspot User IP Assignment
![Hotspot User IP](usergivenhsip.png)
*Figure 6: Hotspot user receives IP address*

---

## Device Configurations

Below are the exported configurations for each MikroTik device in the lab. These can be imported or referenced for your own GNS3 setups.

---

### AGREGATOR_SWITCH
```shell
[admin@AGREGATOR_SWITCH] > export
# mar/24/2026 12:56:34 by RouterOS 7.6
# ...existing code...
```

### PPPOE_GNS3
```shell
[admin@PPPOE_GNS3] > export
# mar/24/2026 12:57:03 by RouterOS 7.6
# ...existing code...
```

### HS_GNS3
```shell
[admin@HS_GNS3] > export
# mar/24/2026 12:57:27 by RouterOS 7.6
# ...existing code...
```

### SWITCH_GNS3
```shell
[admin@SWITCH_GNS3] > export
# mar/24/2026 12:57:50 by RouterOS 7.6
# ...existing code...
```

### ROUTING_ROUTER
```shell
[admin@ROUTING_ROUTER] > export
# mar/24/2026 12:58:14 by RouterOS 7.6
# ...existing code...
```

---

## Usage

1. Open the project in GNS3.
2. Import the provided MikroTik RouterOS configurations to the respective devices.
3. Refer to the screenshots and diagrams for expected results and troubleshooting.

---

## License

This project is for educational and demonstration purposes.
