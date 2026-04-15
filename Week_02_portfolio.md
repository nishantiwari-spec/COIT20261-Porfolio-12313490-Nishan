# COIT20261 - Week 02 Portfolio

**Student ID:** 12313490
**Name:** Nishan Tiwari
**Date:** 09 March 2026
**Unit:** COIT20261 - Network Design and Management
**Campus:** CQUniversity Sydney

---

## Task 1: Setting Static IP Addresses

### Project Details
| Field | Value |
|-------|-------|
| Project Name | Setting-IP-12313490 |
| Network Address | 10.1.1.0/24 |

### IP Address Plan
| Host | IP Address | Method Used |
|------|-----------|-------------|
| Host 1 | 10.1.1.1/24 | GNS3 Configure menu |
| Host 2 | 10.1.1.2/24 | GNS3 Configure menu |
| Host 3 | 10.1.1.3/24 | Edited /etc/network/interfaces in console |
| Host 4 | 10.1.1.4/24 | ip address add command |

### Method 1 and 2 - GNS3 Configure Menu
Used GNS3 Configure menu on Host 1 and Host 2 before starting the nodes.
IP address is applied automatically when node starts.

### Method 3 - Editing interfaces file in console
Opened console on Host 3 and ran:
```bash
nano /etc/network/interfaces
```
Added the following:
```bash
auto eth0
iface eth0 inet static
   address 10.1.1.3
   netmask 255.255.255.0
   up sysctl net.ipv4.ip_forward=0
```
Then reloaded with:
```bash
ifdown eth0
ifup eth0
```

### Method 4 - ip address add command
Opened console on Host 4 and ran:
```bash
ip address add 10.1.1.4/24 dev eth0
```
Note: this method works immediately but does not survive a reboot.

### Verified all IP addresses with:
```bash
ip address show
```

### Outputs
- [x] Setting-IP-12313490.gns3project exported
- [x] Setting-IP-12313490-network.png
- [x] Setting-IP-12313490-host1.png
- [x] Setting-IP-12313490-host2.png
- [x] Setting-IP-12313490-host3.png
- [x] Setting-IP-12313490-host4.png

---

## Task 2: Testing Network Connectivity with Ping

### What is Ping
Ping sends a request message from one device to another. If the destination replies, it confirms the device is reachable. It also measures the round-trip time (RTT) — how long it takes for the message to go and come back.

### Test 1 - Basic Ping (Host A to Host B)
```bash
ping 10.1.1.2
```
Stopped after 5 responses using Ctrl-C.
Observed average RTT in the summary output.

### Test 2 - Ping to Non-Existent IP
```bash
ping 10.1.1.99
```
Waited 10 seconds then stopped.
Result showed 100% packet loss — no device at that address replied.

### Test 3 - Ping with Custom Options
```bash
ping -c 5 -s 100 -i 2 10.1.1.2
```
- `-c 5` limits to 5 requests
- `-s 100` sets data size to 100 bytes
- `-i 2` sets interval to 2 seconds between requests

### Outputs
- [x] Ping-Basics-12313490-simple.png
- [x] Ping-Basics-12313490-error.png
- [x] Ping-Basics-12313490-options.png

---

## Learnings
- Three different ways to set a static IP address in Linux
- The /etc/network/interfaces method is permanent across reboots
- The ip address add command is quick but resets after reboot
- Ping confirms whether a device is reachable on the network
- Packet loss means no device exists at that IP address
- RTT measures the delay between two devices on a network
