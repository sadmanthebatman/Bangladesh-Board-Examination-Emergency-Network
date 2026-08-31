# Bangladesh Board Examination Emergency Network (BBEEN)

## Cisco Packet Tracer Network Design & Implementation

The **Bangladesh Board Examination Emergency Network (BBEEN)** is a multi-site enterprise network designed to provide secure, reliable, and resilient communication between key examination-related locations in Bangladesh.

The project was implemented and tested using **Cisco Packet Tracer** and demonstrates practical networking concepts including **VLSM, RIPv2, static routing, floating static routing, DHCP, DNS, HTTP, SMTP, POP3, and network redundancy**.

---

## Project Overview

The network connects five major sites:

* **National Education Board (HQ)**
* **Divisional Board Office (DBO)**
* **District Examination Center (DEC)**
* **Secure Printing Press (SPP)**
* **Emergency Distribution Center (EDC)**

The design provides communication between these sites while implementing specific routing policies and backup paths to maintain connectivity during network failures.

---

## Network Technologies

The following technologies and concepts were implemented:

* Cisco Packet Tracer
* IPv4 addressing
* Variable Length Subnet Masking (VLSM)
* RIPv2
* Static Routing
* Floating Static Routing
* Administrative Distance
* DHCP
* DNS
* HTTP/HTTPS
* SMTP
* POP3
* Serial point-to-point links
* LAN switching
* Network redundancy
* Connectivity testing
* Traceroute analysis

---

## Network Addressing

The project uses the address space:

```text
23.63.0.0/16
```

Major LANs were assigned using VLSM according to host requirements, while point-to-point links use `/30` networks.

The shared backbone network is:

```text
23.63.30.0/24
```

---

## Routing Design

### RIPv2

All five routers use RIPv2 with automatic summarization disabled:

```text
router rip
 version 2
 network 23.0.0.0
 no auto-summary
```

RIPv2 provides dynamic route exchange between the network's routers.

---

### DEC-to-HQ Routing Policy

The District Examination Center is configured to reach the HQ network through the Divisional Board Office.

Router-DEC uses the following static route:

```text
ip route 23.63.8.0 255.255.248.0 23.63.30.1
```

This ensures that traffic from DEC to HQ follows the required path:

```text
DEC → DBO → HQ
```

A traceroute test confirmed the expected routing path.

---

## HQ-DBO Redundancy

The HQ and DBO locations have two possible communication paths:

### Primary Path

```text
DBO → Serial Link → HQ
```

with:

```text
Administrative Distance = 1
```

### Backup Path

```text
DBO → Shared Backbone → HQ
```

with:

```text
Administrative Distance = 10
```

The backup route is configured as a floating static route:

```text
ip route 23.63.8.0 255.255.248.0 23.63.30.4 10
```

During testing, the primary serial interface was administratively shut down. The routing table then selected the backup route and the HQ Web Server remained reachable.

---

## SPP Floating Static Route

The Secure Printing Press also uses a floating static route to provide an alternative path toward HQ.

The required administrative distance was calculated as:

```text
AD = Number of courses of second-last member × Number of group members

AD = 4 × 4

AD = 16
```

Therefore, the backup route was configured with an administrative distance of 16:

```text
ip route 23.63.8.0 255.255.248.0 23.63.30.1 16
```

The backup route was tested by temporarily removing the primary route, after which the AD-16 route became active successfully.

---

## Network Services

### DHCP

DHCP services are provided to multiple sites.

DBO:

```text
Network: 23.63.16.0/22
Gateway: 23.63.16.1
DNS: 23.63.8.10
```

DEC:

```text
Network: 23.63.0.0/21
Gateway: 23.63.0.1
DNS: 23.63.8.10
```

SPP uses a dedicated DHCP server.

---

### DNS

A dedicated DNS server provides hostname resolution for the network's services.

The web server is associated with:

```text
www.educationboard.gov.bd
```

---

### Web Server

The HQ Web Server is configured at:

```text
23.63.8.20
```

The project web page displays:

```text
National Examination Control System LIVE
```

---

### Email Services

SMTP and POP3 services were configured to support communication between sites.

Cross-site email communication between HQ and SPP was successfully tested after configuring the appropriate mail domain and DNS settings.

---

## Network Redundancy Testing

One of the major objectives of the project was to verify that communication remains available when a primary route fails.

The HQ-DBO serial connection was intentionally disabled during testing.

The network successfully switched to the backup floating static route:

```text
Primary Route
AD = 1

        ↓ Failure

Backup Route
AD = 10
```

The HQ Web Server remained reachable after failover.

This demonstrates the network's ability to maintain connectivity during a link failure.

---

## Connectivity Testing

The implementation was validated using several Cisco Packet Tracer testing methods, including:

* `ping`
* `traceroute`
* `show ip route`
* DHCP lease verification
* DNS hostname resolution
* Web server access
* Email communication
* Floating static route failover testing

---

## Project Files

### Packet Tracer

The `Packet-Tracer` directory contains the final Cisco Packet Tracer implementation files.

### Documentation

The `Documentation` directory contains the complete technical documentation and project report.

### Configuration

The `Configuration` directory contains the router configuration files used in the implementation.

### Evidence

The `Evidence` directory contains topology and testing screenshots.

---

## Learning Outcomes

Through this project, the following practical networking skills were demonstrated:

1. Designing a multi-site enterprise network.
2. Applying VLSM to efficiently allocate IPv4 addresses.
3. Configuring Cisco routers and switches.
4. Implementing RIPv2 dynamic routing.
5. Configuring static and floating static routes.
6. Using Administrative Distance to control route selection.
7. Implementing DHCP and DNS services.
8. Configuring web and email services.
9. Testing network connectivity and routing paths.
10. Designing and testing network redundancy.

---

## Tools Used

| Tool                | Purpose                               |
| ------------------- | ------------------------------------- |
| Cisco Packet Tracer | Network simulation and implementation |
| Cisco IOS           | Router configuration                  |
| Ping                | Connectivity testing                  |
| Traceroute          | Path verification                     |
| DHCP                | Automatic IP configuration            |
| DNS                 | Hostname resolution                   |
| HTTP/HTTPS          | Web service                           |
| SMTP/POP3           | Email communication                   |

---

## Project Status

**Status:** Completed and Tested

The final implementation was tested for routing, connectivity, DHCP, DNS, web services, email communication, and routing failover.

