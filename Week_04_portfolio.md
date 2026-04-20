# COIT20261 - Week 04 Portfolio

**Student ID:** 12313490
**Name:** Nishan Tiwari
**Date:** 23 March 2026
**Unit:** COIT20261 - Network Design and Management
**Campus:** CQUniversity Sydney

---

## Task 1: View Routing Tables

### Project Details

| Field | Value |
|-------|-------|
| Project Name | View-Routes-12313490 |
| Subnet 1 | 192.168.10.0/24 |
| Subnet 2 | 192.168.20.0/24 |

### Network Layout

Host1 and Host2 are connected to Switch1 on Subnet 1.
Router1 connects Subnet 1 via eth3 and Subnet 2 via eth1.
Host3 is connected directly to Router1 on Subnet 2.

### IP Address Plan

| Device | Interface | IP Address | Gateway |
|--------|-----------|-----------|---------|
| Host1 | eth0 | 192.168.10.10/24 | 192.168.10.1 |
| Host2 | eth0 | 192.168.10.20/24 | 192.168.10.1 |
| Router1 | eth3 | 192.168.10.1/24 | - |
| Router1 | eth1 | 192.168.20.1/24 | - |
| Host3 | eth0 | 192.168.20.10/24 | 192.168.20.1 |

### Forwarding Configuration

Forwarding was enabled on Router1 and disabled on all hosts using:

    up sysctl net.ipv4.ip_forward=1   # Router1
    up sysctl net.ipv4.ip_forward=0   # All hosts

Forwarding status confirmed on Router1 console showing:

    net.ipv4.ip_forward = 1
    net.ipv4.ip_forward = 1

### Network Screenshot

![Network](https://github.com/nishantiwari-spec/COIT20261-Porfolio-12313490-Nishan/blob/main/images/View-Routes-12313490-network.png.png)

### Routing Tables

Command used on all devices:

    ip route show

#### Router1

    192.168.10.0/24 dev eth3 scope link src 192.168.10.1
    192.168.20.0/24 dev eth1 scope link src 192.168.20.1

Router1 knows both subnets directly through its two interfaces.

![Router1 Routes](https://github.com/nishantiwari-spec/COIT20261-Porfolio-12313490-Nishan/blob/main/images/View-Routes-12313490-router-routes.png.png)

#### Host1

    default via 192.168.10.1 dev eth0
    192.168.10.0/24 dev eth0 scope link src 192.168.10.10

![Host1 Routes](https://github.com/nishantiwari-spec/COIT20261-Porfolio-12313490-Nishan/blob/main/images/View-Routes-12313490-host1-routes.png.png)

#### Host2

    default via 192.168.10.1 dev eth0
    192.168.10.0/24 dev eth0 scope link src 192.168.10.20

![Host2 Routes](https://github.com/nishantiwari-spec/COIT20261-Porfolio-12313490-Nishan/blob/main/images/View-Routes-12313490-host2-routes.png.png)

#### Host3

    default via 192.168.20.1 dev eth0
    192.168.20.0/24 dev eth0 scope link src 192.168.20.10

![Host3 Routes](https://github.com/nishantiwari-spec/COIT20261-Porfolio-12313490-Nishan/blob/main/images/View-Routes-12313490-host3-routes.png.png)

### Routing Table Summary

| Device | Destination | Next Hop |
|--------|-------------|----------|
| Host1 | 192.168.10.0/24 | directly connected |
| Host1 | default | 192.168.10.1 (Router1) |
| Host2 | 192.168.10.0/24 | directly connected |
| Host2 | default | 192.168.10.1 (Router1) |
| Host3 | 192.168.20.0/24 | directly connected |
| Host3 | default | 192.168.20.1 (Router1) |
| Router1 | 192.168.10.0/24 | directly connected via eth3 |
| Router1 | 192.168.20.0/24 | directly connected via eth1 |

### Ping Test

Pinged from Host1 to Host3 across both subnets:

    ping -c 3 192.168.20.10

Result: 3 packets transmitted, 3 received, 0% packet loss.
TTL=63 confirms one router was traversed (Host1 -> Router1 -> Host3).

![Ping Test](https://github.com/nishantiwari-spec/COIT20261-Porfolio-12313490-Nishan/blob/main/images/View-Routes-12313490-ping.png.png)

### Outputs

- View-Routes-12313490.gns3project exported
- View-Routes-12313490-network.png screenshot taken
- View-Routes-12313490-router-routes.png screenshot taken
- View-Routes-12313490-host1-routes.png screenshot taken
- View-Routes-12313490-host2-routes.png screenshot taken
- View-Routes-12313490-host3-routes.png screenshot taken
- View-Routes-12313490-ping.png screenshot taken

---

## Task 2: Dynamic Routing with OSPF

### Project Details

| Field | Value |
|-------|-------|
| Project Name | OSPF-Basics-12313490 |
| Routing Protocol | OSPF (Open Shortest Path First) |

### Network Layout

This network has two hosts and four routers of FRR type. The routers FRR1 and FRR2 comprise the upper route, while FRR1 and FRR3 comprise the lower route. Two NETem modules will emulate link delays for routers. OSPF protocol is enabled for automatic sharing of routes information among all routers.

### OSPF Neighbour Routers of FRR1

Command run on FRR1:

    show ip ospf neighbor

Output showed FRR2 and FRR3 as neighbours of FRR1.

| Neighbour | IP Address | State |
|-----------|-----------|-------|
| FRR2 | 10.0.1.2 | Full |
| FRR3 | 10.0.2.2 | Full |



### Routing Tables

Command run on FRR1 and FRR4:

    show ip ospf route
    show ip route

#### FRR1 Routing Table

| Destination | Next Hop |
|-------------|----------|
| 10.0.1.0/24 | directly connected |
| 10.0.2.0/24 | directly connected |
| 10.0.3.0/24 | via FRR2 (10.0.1.2) |
| 10.0.4.0/24 | via FRR3 (10.0.2.2) |

#### FRR4 Routing Table

| Destination | Next Hop |
|-------------|----------|
| 10.0.3.0/24 | directly connected |
| 10.0.4.0/24 | directly connected |
| 10.0.1.0/24 | via FRR2 (10.0.3.1) |
| 10.0.2.0/24 | via FRR3 (10.0.4.1) |



### Traceroute Before Link Down

Traceroute run from Host1 to Host2 showed the top path via FRR2 was used:

    traceroute <Host2-IP>

    1  FRR1  0.5ms
    2  FRR2  1.2ms
    3  FRR4  1.8ms
    4  Host2  2.1ms

### Traceroute After Link Down

Stopped the NETem node on the top path to disconnect the link between FRR2 and FRR4.
OSPF detected the change and updated routing tables automatically.
Traceroute now showed the bottom path via FRR3 was used:

    1  FRR1  0.6ms
    2  FRR3  1.4ms
    3  FRR4  2.0ms
    4  Host2  2.3ms



### Outputs

- OSPF-Basics-12313490.gns3project exported
- OSPF-Basics-12313490-network.png screenshot taken
- OSPF-Basics-12313490-neighbors.png screenshot taken
- OSPF-Basics-12313490-routes.png screenshot taken
- OSPF-Basics-12313490-traceroute.png screenshot taken

---

## Learnings

- A router acts as a link between two or more networks and allows packets to pass through them
- Forwarding should be turned ON in the router and OFF in hosts
- The ip route show command helps us view the routing table in a device
- A host will employ its default gateway for sending packets to other networks
- The OSPF protocol is dynamic and helps to exchange routes automatically between routers
- When there is a break in any connection, OSPF notices this and discovers another path
- Traceroute tells the exact path packets follow in the network
- TTL is reduced by one with every router a packet passes through
