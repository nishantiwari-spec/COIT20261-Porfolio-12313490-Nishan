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
| Network Address | 192.168.10.0/24 |

### IP Address Plan

| Host | IP Address | Method Used |
|------|-----------|-------------|
| Host1 | 192.168.10.10/24 | GNS3 Configure menu |
| Host2 | 192.168.10.20/24 | GNS3 Configure menu |
| Host3 | 192.168.10.30/24 | Edited /etc/network/interfaces in console |
| Host4 | 192.168.10.40/24 | ip addr add command |

### Method 1 and 2 - GNS3 Configure Menu

Used GNS3 Configure menu on Host1 and Host2 before starting the nodes.
IP address is applied automatically when the node starts.

### Method 3 - Editing interfaces file in console

Opened console on Host3 and ran:
```bash
nano /etc/network/interfaces
```
Added the following:
```bash
auto eth0
iface eth0 inet static
   address 192.168.10.30
   netmask 255.255.255.0
   up sysctl net.ipv4.ip_forward=0
```
Then reloaded with:
```bash
ifdown eth0
ifup eth0
```

### Method 4 - ip addr add command

Opened console on Host4 and ran:
```bash
ip addr add 192.168.10.40/24 dev eth0
```
Note: this method works immediately but does not survive a reboot.

### Verified all IP addresses with:
```bash
ip addr show
```

### Network Screenshot

![Network](https://github.com/nishantiwari-spec/COIT20261-Porfolio-12313490-Nishan/blob/main/images/Setting-IP-12313490-network.png.png)

### Host Screenshots

![Host1](https://github.com/nishantiwari-spec/COIT20261-Porfolio-12313490-Nishan/blob/main/images/Setting-IP-12313490-host1.png.png)
![Host2](https://github.com/nishantiwari-spec/COIT20261-Porfolio-12313490-Nishan/blob/main/images/Setting-IP-12313490-host2.png.png)
![Host3](https://github.com/nishantiwari-spec/COIT20261-Porfolio-12313490-Nishan/blob/main/images/Setting-IP-12313490-host3.png.png)
![Host4](https://github.com/nishantiwari-spec/COIT20261-Porfolio-12313490-Nishan/blob/main/images/Setting-IP-12313490-host4.png.png)

### Outputs

- Setting-IP-12313490.gns3project exported
- Setting-IP-12313490-network.png screenshot taken
- Setting-IP-12313490-host1.png screenshot taken
- Setting-IP-12313490-host2.png screenshot taken
- Setting-IP-12313490-host3.png screenshot taken
- Setting-IP-12313490-host4.png screenshot taken

---

## Task 2: Testing Network Connectivity with Ping

### What is Ping

Ping sends a request from one device to another. If the destination replies, it confirms the device is reachable. It also measures the round-trip time (RTT) which is how long it takes for the message to go and come back.

### Test 1 - Basic Ping

Pinged from Host1 to Host2 with no options.

```bash
ping 192.168.10.20
```

Result: 6 packets transmitted, 6 received, 0% packet loss, average RTT 0.444ms

![Basic Ping](https://github.com/nishantiwari-spec/COIT20261-Porfolio-12313490-Nishan/blob/main/images/Ping-Basics-12313490-simple.png.png)

### Test 2 - Ping to Wrong IP

Pinged a non-existent IP address and waited 10 seconds.

```bash
ping 192.168.10.99
```

Result: 100% packet loss. No device exists at that address so no reply was received.

![Error Ping]([Ping-Basics-12313490-error.png](https://github.com/nishantiwari-spec/COIT20261-Porfolio-12313490-Nishan/blob/main/images/Ping-Basics-12313490-error.png.png))

### Test 3 - Ping with Custom Options

```bash
ping -c 5 -i 2 192.168.10.20
```

- `-c 5` limits to 5 requests
- `-i 2` sets interval to 2 seconds between requests

Result: 5 packets transmitted, 5 received, 0% packet loss, average RTT 0.472ms

![Options Ping](https://github.com/nishantiwari-spec/COIT20261-Porfolio-12313490-Nishan/blob/main/images/Ping-Basics-12313490-options.png.png)

### Outputs

- Ping-Basics-12313490-simple.png screenshot taken
- Ping-Basics-12313490-error.png screenshot taken
- Ping-Basics-12313490-options.png screenshot taken

---

## Learnings

- Three different ways to set a static IP address in Linux
- The /etc/network/interfaces method is permanent across reboots
- The ip addr add command is quick but resets after reboot
- Ping confirms whether a device is reachable on the network
- 100% packet loss means no device exists at that IP address
- RTT measures the delay between two devices on a network
