# COIT20261 - Week 06 Portfolio

**Student ID:** 12313490
**Name:** Nishan Tiwari
**Date:** 06 April 2026
**Unit:** COIT20261 - Network Design and Management
**Campus:** CQUniversity Sydney

---

## Task 1: Resolving IP Addresses to Hardware Addresses (ARP)

### Project Used

Setting-IP-12313490 with four Linux hosts and one Ethernet switch.

| Host | IP Address |
|------|-----------|
| Host1 (A) | 192.168.10.10/24 |
| Host2 (B) | 192.168.10.20/24 |
| Host3 (C) | 192.168.10.30/24 |
| Host4 (D) | 192.168.10.40/24 |

### What is ARP

Address Resolution Protocol is abbreviated as ARP. This protocol enables a computer to discover the MAC address of another device using only its IP address. This process is initiated through the sending out of a broadcast by the device that says "Who has this IP address?", to which the corresponding device responds by providing its MAC address. This is recorded in the ARP table for future reference.
### Command Used

    ip neigh show

### ARP Table - Before Pinging

Before any form of communication, there were no entries in the ARP table for Host1 since Host1 had not communicated to any other node yet.

![ARP Table Before](ARP-Basics-12313490-HostA-Table1.png)

### ARP Table - After Pinging Host2

Pinged from Host1 to Host2:

    ping -c 3 192.168.10.20

AOnce pinged, the ARP table for Host1 contained an entry for Host2
(192.168.10.20) with its hardware address and state REACHABLE. It also
showed the gateway Router1 (192.168.10.1) with state STALE.

    192.168.10.20 dev eth0 lladdr 02:42:c3:0f:46:00 REACHABLE
    192.168.10.1  dev eth0 lladdr 02:42:86:71:9a:01 STALE

REACHABLE means the address was recently confirmed as valid.
STALE means the entry exists but has not been used recently.

![ARP Table After Ping](ARP-Basics-12313490-HostA-Table2.png)

### ARP Table - After Host2 Pinged Host1

Pinged from Host2 to Host1:

    ping -c 3 192.168.10.10

After Host2 contacted Host1, the ARP table of Host1 updated. The gateway
entry changed to REACHABLE and Host2 entry changed to STALE as time passed.

    192.168.10.20 dev eth0 lladdr 02:42:c3:0f:46:00 STALE
    192.168.10.1  dev eth0 lladdr 02:42:86:71:9a:01 REACHABLE

![ARP Table After Host2 Ping](ARP-Basics-12313490-HostA-Table3.png)

### Outputs

- ARP-Basics-12313490-HostA-Table1.png screenshot taken
- ARP-Basics-12313490-HostA-Table2.png screenshot taken
- ARP-Basics-12313490-HostA-Table3.png screenshot taken

---

## Task 2: Default Gateways

### Project Details

| Field | Value |
|-------|-------|
| Project Name | Default-Gateway-12313490 |
| Subnet 1 | 192.168.10.0/24 |
| Subnet 2 | 192.168.20.0/24 |
| Subnet 3 | 192.168.30.0/24 |

### Network Layout

Host1 and Host2 connect to Router1 via Switch2 on Subnet 1.
Host3 and Host4 connect to Router2 via Switch1 on Subnet 2.
Router1 and Router2 connect directly to each other on Subnet 3.

### Network Screenshot

![Network](Default-Gateway-12313490-network.png)

### IP Address Plan

| Device | Interface | IP Address | Gateway |
|--------|-----------|-----------|---------|
| Host1 | eth0 | 192.168.10.10/24 | 192.168.10.1 |
| Host2 | eth0 | 192.168.10.20/24 | 192.168.10.1 |
| Router1 | eth0 | 192.168.30.1/24 | - |
| Router1 | eth1 | 192.168.10.1/24 | - |
| Router2 | eth0 | 192.168.30.2/24 | - |
| Router2 | eth1 | 192.168.20.1/24 | - |
| Host3 | eth0 | 192.168.20.10/24 | 192.168.20.1 |
| Host4 | eth0 | 192.168.20.20/24 | 192.168.20.1 |

### Routing Tables

Command used on all devices:

    ip route show

#### Router1

    192.168.10.0/24 dev eth1 scope link src 192.168.10.1
    192.168.20.0/24 via 192.168.30.2 dev eth0
    192.168.30.0/24 dev eth0 scope link src 192.168.30.1

![Router1 Routes](Default-Gateway-12313490-router1-routes.png)

#### Router2

    192.168.10.0/24 via 192.168.30.1 dev eth0
    192.168.20.0/24 dev eth1 scope link src 192.168.20.1
    192.168.30.0/24 dev eth0 scope link src 192.168.30.2

![Router2 Routes](Default-Gateway-12313490-router2-routes.png)

#### Host1

    default via 192.168.10.1 dev eth0
    192.168.10.0/24 dev eth0 scope link src 192.168.10.10

![Host1 Routes](Default-Gateway-12313490-host1-routes.png)

### Routing Table Summary

| Device | Destination | Next Hop |
|--------|-------------|----------|
| Host1 | 192.168.10.0/24 | directly connected |
| Host1 | default | 192.168.10.1 (Router1) |
| Host2 | 192.168.10.0/24 | directly connected |
| Host2 | default | 192.168.10.1 (Router1) |
| Host3 | 192.168.20.0/24 | directly connected |
| Host3 | default | 192.168.20.1 (Router2) |
| Host4 | 192.168.20.0/24 | directly connected |
| Host4 | default | 192.168.20.1 (Router2) |
| Router1 | 192.168.10.0/24 | directly connected via eth1 |
| Router1 | 192.168.30.0/24 | directly connected via eth0 |
| Router1 | 192.168.20.0/24 | via Router2 (192.168.30.2) |
| Router2 | 192.168.20.0/24 | directly connected via eth1 |
| Router2 | 192.168.30.0/24 | directly connected via eth0 |
| Router2 | 192.168.10.0/24 | via Router1 (192.168.30.1) |

### Ping Test

Pinged from Host1 to Host3 across all three subnets:

    ping -c 3 192.168.20.10

Result: 3 packets transmitted, 3 received, 0% packet loss.
TTL=62 confirms two routers were traversed (Host1 -> Router1 -> Router2 -> Host3).

![Ping Test](Default-Gateway-12313490-ping.png)

### Outputs

- Default-Gateway-12313490.gns3project exported
- Default-Gateway-12313490-network.png screenshot taken
- Default-Gateway-12313490-router1-routes.png screenshot taken
- Default-Gateway-12313490-router2-routes.png screenshot taken
- Default-Gateway-12313490-host1-routes.png screenshot taken
- Default-Gateway-12313490-ping.png screenshot taken

---

## Learnings
- ARP translates IP addresses into hardware addresses (MAC)
- ARP tables are formed automatically as hosts communicate
- REACHABLE indicates that the hardware address is valid as of the last check
- STALE means the entry was found but has not been recently used
- ARP records are deleted if a device does not receive traffic from the device in question over time
- Default gateway is the router to which the host forwards its packets in case the destination is     another subnet
- Router requires particular routes to be able to forward packets in between nonadjacent subnets
- ARP table can be viewed using the ip neigh show command
- Each time the packet passes through a router, TTL is decreased by 1

