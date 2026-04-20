# COIT20261 - Week 05 Portfolio

**Student ID:** 12313490
**Name:** Nishan Tiwari
**Date:** 30 March 2026
**Unit:** COIT20261 - Network Design and Management
**Campus:** CQUniversity Sydney

---

## Task 1: Setup VLANs on Switch

### Project Details

| Field | Value |
|-------|-------|
| Project Name | Vlan-Basics-12313490 |
| Network Address | 192.168.10.0/24 |
| VLAN 490 | Host1 and Host2 |
| VLAN 491 | Host3 and Host4 |

VLAN IDs are based on the last three digits of student ID 12313490.

### Network Layout

Four Linux hosts connected to one OpenvSwitch. Each host connects to the
switch starting from eth1. eth0 on the switch is left unused for Task 2.

| Host | Switch Port | IP Address | VLAN |
|------|------------|-----------|------|
| Host1 | eth1 | 192.168.10.10/24 | 490 |
| Host2 | eth2 | 192.168.10.20/24 | 490 |
| Host3 | eth3 | 192.168.10.30/24 | 491 |
| Host4 | eth4 | 192.168.10.40/24 | 491 |

### Network Screenshot

![Network](https://github.com/nishantiwari-spec/COIT20261-Porfolio-12313490-Nishan/blob/main/images/Vlan-Basics-12313490-network.png.png)

### Connectivity Test Before VLANs

All hosts were on the same subnet before VLANs were configured.
Ping from Host1 to all other hosts was successful with 0% packet loss.

    ping -c 3 192.168.10.20
    ping -c 3 192.168.10.30
    ping -c 3 192.168.10.40

Result: All hosts reachable before VLAN configuration.

### VLAN Configuration on Switch

Started the OpenvSwitch service:

    /usr/share/openvswitch/scripts/ovs-ctl start

Created bridge and added ports:

    ovs-vsctl add-br br0
    ovs-vsctl add-port br0 eth1
    ovs-vsctl add-port br0 eth2
    ovs-vsctl add-port br0 eth3
    ovs-vsctl add-port br0 eth4

Set VLAN tags on access ports:

    ovs-vsctl set port eth1 tag=490
    ovs-vsctl set port eth2 tag=490
    ovs-vsctl set port eth3 tag=491
    ovs-vsctl set port eth4 tag=491

Viewed switch port configuration:

    ovs-vsctl show

### Switch Ports Screenshot

![Switch Ports](https://github.com/nishantiwari-spec/COIT20261-Porfolio-12313490-Nishan/blob/main/images/Screenshot%202026-04-20%20213914.png)

### Connectivity Test After VLANs

After VLAN configuration, hosts on the same VLAN can communicate but
hosts on different VLANs cannot.

- Host1 to Host2 (both VLAN 490) — reachable
- Host3 to Host4 (both VLAN 491) — reachable
- Host1 to Host3 (different VLANs) — not reachable
- Host1 to Host4 (different VLANs) — not reachable

### Outputs

- Vlan-Basics-12313490.gns3project exported
- Vlan-Basics-12313490-network.png screenshot taken
- Vlan-Basics-12313490-ports.png screenshot taken

---

## Task 2: Setup VLANs on a Router

### Project Details

| Field | Value |
|-------|-------|
| Project Name | Vlan-Router-12313490 |
| Subnet 1 (VLAN 490) | 192.168.10.0/24 |
| Subnet 2 (VLAN 491) | 192.168.20.0/24 |

### Network Layout

Same four hosts and OpenvSwitch from Task 1. A Linux Router was added
and connected to eth0 of the switch. Host1 and Host2 are on VLAN 490
with subnet 192.168.10.0/24. Host3 and Host4 are on VLAN 491 with
subnet 192.168.20.0/24.

| Device | IP Address | VLAN | Gateway |
|--------|-----------|------|---------|
| Host1 | 192.168.10.10/24 | 490 | 192.168.10.1 |
| Host2 | 192.168.10.20/24 | 490 | 192.168.10.1 |
| Host3 | 192.168.20.10/24 | 491 | 192.168.20.1 |
| Host4 | 192.168.20.20/24 | 491 | 192.168.20.1 |
| Router eth0.490 | 192.168.10.1/24 | 490 | - |
| Router eth0.491 | 192.168.20.1/24 | 491 | - |

### Switch Configuration

Set access ports for hosts:

    ovs-vsctl set port eth1 tag=490
    ovs-vsctl set port eth2 tag=490
    ovs-vsctl set port eth3 tag=491
    ovs-vsctl set port eth4 tag=491

Set eth0 as trunk port to carry all VLANs to the router:

    ovs-vsctl set port eth0 trunks=[]

### Router Sub-interface Configuration

Created sub-interfaces on the router for each VLAN:

    ip link add link eth0 name eth0.490 type vlan id 490
    ip link add link eth0 name eth0.491 type vlan id 491

Brought up the interfaces:

    ip link set eth0 up
    ip link set eth0.490 up
    ip link set eth0.491 up

Assigned IP addresses to sub-interfaces:

    ip address add 192.168.10.1/24 dev eth0.490
    ip address add 192.168.20.1/24 dev eth0.491

Enabled forwarding on the router:

    sysctl net.ipv4.ip_forward=1

### Connectivity Test

After router configuration all hosts could communicate across VLANs
through the router.

- Host1 to Host2 (same VLAN 490) — reachable directly
- Host3 to Host4 (same VLAN 491) — reachable directly
- Host1 to Host3 (different VLANs) — reachable via router
- Host1 to Host4 (different VLANs) — reachable via router

### Outputs

- Vlan-Router-12313490.gns3project exported
- Vlan-Router-12313490-network.png screenshot taken
- Vlan-Router-12313490-ports.png screenshot taken

---

## Learnings

- VLANs separate network traffic on the same physical switch into different logical networks
- Hosts on the same VLAN can communicate directly without a router
- Hosts on different VLANs cannot communicate unless a router is used
- VLAN IDs are assigned to access ports on the switch using ovs-vsctl set port
- A trunk port carries traffic from multiple VLANs between a switch and a router
- Sub-interfaces allow a router to handle multiple VLANs on a single physical interface
- Each VLAN sub-interface gets its own IP address and acts as the gateway for that VLAN
- OpenvSwitch is a virtual managed switch that supports VLAN configuration in GNS3

