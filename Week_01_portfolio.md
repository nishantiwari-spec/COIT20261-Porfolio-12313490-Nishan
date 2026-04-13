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
| IP Address | 10.10.1.1 |
| Netmask | 255.255.255.0 |

### Network Configuration
Configuration used in `/etc/network/interfaces`:

```bash
auto eth0
iface eth0 inet static
   address 10.10.1.1
   netmask 255.255.255.0
   up sysctl net.ipv4.ip_forward=0
```

### Commands Used
```bash
ip address show
```

### Outputs
- [x] GNS3-Intro-12313490.gns3project exported
- [x] GNS3-Intro-12313490-network.png screenshot
- [x] GNS3-Intro-12313490-ipaddress.png screenshot

### Learnings
- How to create a new GNS3 project
- How to add and configure a Linux Host node
- How to set a static IP address using `/etc/network/interfaces`
- How to disable IP forwarding on a host node
- How to verify IP address using `ip address show` command
- How to use the GNS3 web console

---

## Notes
Completed Week 01 tutorial tasks including GNS3 project setup,
node configuration, and static IP address assignment.
Successfully verified IP address via console using `ip address show`.
