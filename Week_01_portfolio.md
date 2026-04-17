# COIT20261 - Week 01 Portfolio

**Student ID:** 12313490
**Name:** Nishan Tiwari
**Date:** 02 March 2026
**Unit:** COIT20261 - Network Design and Management
**Campus:** CQUniversity Sydney

---

## Section A - Setup

### 1. Unit Review
- Reviewed unit profile and assessment tasks
- Understood portfolio requirements and submission process

### 2. Software Setup
- [x] VirtualBox installed
- [x] GNS3 installed

### 3. GitHub Repository
- Created private repository named: `12313490-COIT20261-2026T1`
- Shared repository with tutor

---

## Section B - Task 1: GNS3 Introduction

### Project Details
| Field | Value |
|-------|-------|
| Project Name | GNS3-Intro-12313490 |
| IP Address | 192.168.10.10 |
| Netmask | 255.255.255.0 |

### Network Configuration
Configuration used in `/etc/network/interfaces`:

```bash
auto eth0
iface eth0 inet static
   address 192.168.10.10
   netmask 255.255.255.0
   up sysctl net.ipv4.ip_forward=0
```

### Commands Used
```bash
ip address show
```

### Outputs
- [x] GNS3-Intro-12313490.gns3project exported
- [x] GNS3-Intro-12313490-network.png
- [x] GNS3-Intro-12313490-ipaddress.png

### Screenshots

**Network Topology:**

![Network](GNS3-Intro-12313490-network_png.png)

**Console showing IP address:**

![IP Address](GNS3-Intro-12313490-ipaddress_png.png)

---

## Learnings
- How to create a new GNS3 project
- How to add and configure a Linux Host node
- How to set a static IP address using `/etc/network/interfaces`
- How to disable IP forwarding on a host node
- How to verify IP address using `ip address show` command
- How to use the GNS3 web console
- The loopback interface (lo) is always present and uses 127.0.0.1
- eth0 is the main network interface used for LAN communication
