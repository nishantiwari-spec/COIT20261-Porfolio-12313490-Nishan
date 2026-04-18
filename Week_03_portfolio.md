# COIT20261 - Week 03 Portfolio

**Student ID:** 12313490
**Name:** Nishan Tiwari
**Date:** 16 March 2026
**Unit:** COIT20261 - Network Design and Management
**Campus:** CQUniversity Sydney

---

## Task 1: Simple Application Communications with Netcat

### Project Details

| Field | Value |
|-------|-------|
| Project Name | Setting-IP-12313490 |
| Network Address | 192.168.10.0/24 |

### Host IP Addresses

| Host | IP Address | Role |
|------|-----------|------|
| Host1 | 192.168.10.10/24 | Netcat Server |
| Host2 | 192.168.10.20/24 | Netcat Client |
| Host3 | 192.168.10.30/24 | - |
| Host4 | 192.168.10.40/24 | - |

### What is Netcat

Netcat (nc) is a tool that allows two devices to send text messages directly over a network connection. One device runs as a server and listens on a port. The other device runs as a client and connects to that port. Once connected, both sides can send and receive messages. Unlike ping which uses ICMP at the network level, Netcat uses TCP at the application level.

### Steps

Started Netcat server on Host1 listening on port 9999:

```bash
nc -l -p 9999
```

Connected from Host2 as client to Host1:

```bash
nc 192.168.10.10 9999
```

Sent name from Host2 (client) to Host1 (server): Nishan Tiwari
Sent student ID from Host1 (server) to Host2 (client): 12313490
Both messages were received and displayed correctly on each side. Used Ctrl+D to stop Netcat on both hosts.

### Screenshot

![Netcat Client and Server](Netcat-Basics-12313490-client-server.png)

### Outputs

- Netcat-Basics-12313490-client-server.png screenshot taken

---

## Task 2: Capturing Packets

### Steps

Clicked the link connecting Host1 to Switch1 in GNS3 and chose Start capture with Ethernet type and file name: Capture-Basics-12313490-ping-netcat.

Ping command to Host2 from Host1 with 3 pings:

```bash
ping -c 3 192.168.10.20
```

Utilized Netcat to transmit data from Host1 to Host3:

```bash
nc 192.168.10.30 9999
```

Ended the capture by clicking the link and choosing Stop capture.

Capture file saved on GNS3 server located at /opt/gns3/projects/ cannot be downloaded using the web interface. Capture file name: Capture-Basics-12313490-ping-netcat.pcap

### Outputs

- Capture-Basics-12313490-ping-netcat.pcap saved on GNS3 server

---

## Learnings

- Netcat is used for direct communication between two computers through TCP.
- One computer needs to act as a server before the client can communicate.
- The server will listen on a port and the client will connect to the server’s IP and port.
- Netcat checks application level connectivity, and ping checks network level connectivity.
- Packet capture captures all the information on the link in real time.
- All packets captured are saved into .pcap file and can be analyzed using Wireshark.
- Ping traffic and netcat traffic are both captured in packet capture.
