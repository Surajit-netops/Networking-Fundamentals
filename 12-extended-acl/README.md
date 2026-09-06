# Lab 12: Extended ACL - Block HTTP Access for One Host

## Objective
Configure a named Extended Access Control List (ACL) on Router R1 to block HTTP traffic from PC0 to the Web Server, while allowing ICMP ping traffic and all traffic from PC1.

## Topology
- 2 Cisco Routers: R1 and R2
- 2 Cisco 2960 Switches: SW1 and SW2
- 2 PCs: PC0 and PC1
- 1 Server-PT configured as a Web Server
- A point-to-point WAN link between R1 and R2

## Security Policy

| Source Device | Source IP | Destination | ICMP Ping | HTTP (TCP Port 80) |
|---|---|---|---|---|
| PC0 | 192.168.10.10 | Web Server: 192.168.20.10 | Allowed | Denied |
| PC1 | 192.168.10.11 | Web Server: 192.168.20.10 | Allowed | Allowed |

## IP Addressing Plan

| Device | Interface | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|
| R1 | GigabitEthernet0/0 | 192.168.10.1 | 255.255.255.0 | N/A |
| R1 | GigabitEthernet0/1 | 10.0.0.1 | 255.255.255.252 | N/A |
| R2 | GigabitEthernet0/1 | 10.0.0.2 | 255.255.255.252 | N/A |
| R2 | GigabitEthernet0/0 | 192.168.20.1 | 255.255.255.0 | N/A |
| PC0 | FastEthernet0 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| PC1 | FastEthernet0 | 192.168.10.11 | 255.255.255.0 | 192.168.10.1 |
| Web Server | FastEthernet0 | 192.168.20.10 | 255.255.255.0 | 192.168.20.1 |

## Static Routing

### R1

```text
ip route 192.168.20.0 255.255.255.0 10.0.0.2
```

### R2

```text
ip route 192.168.10.0 255.255.255.0 10.0.0.1
```

## Server Configuration

The Server-PT device was configured with the static IP address `192.168.20.10/24`, default gateway `192.168.20.1`, and the HTTP service was enabled under the `Services` tab.

## Extended ACL Configuration

The extended ACL was placed inbound on R1's LAN-facing interface, close to the traffic source.

```text
enable
configure terminal

ip access-list extended BLOCK-PC0-HTTP
 deny tcp host 192.168.10.10 host 192.168.20.10 eq 80
 permit ip any any
exit

interface gigabitEthernet 0/0
 ip access-group BLOCK-PC0-HTTP in
exit

end
write memory
```

## Verification

### PC0: ICMP Ping Allowed

From PC0:

```text
ping 192.168.20.10
```

Expected result: Successful ping replies because the ACL blocks only TCP port 80 traffic.

### PC0: HTTP Access Blocked

From PC0:

1. Open `Desktop` → `Web Browser`.
2. Browse to:

```text
http://192.168.20.10
```

Expected result: The web page does not open because HTTP traffic from PC0 is denied.

### PC1: HTTP Access Allowed

From PC1:

1. Open `Desktop` → `Web Browser`.
2. Browse to:

```text
http://192.168.20.10
```

Expected result: The default web page opens successfully because PC1 traffic is allowed.

### Router ACL Verification

On R1:

```text
show ip access-lists
show ip interface gigabitEthernet 0/0
```

Expected result: The ACL is applied inbound on R1 `GigabitEthernet0/0`, and the deny rule shows match counts after PC0 attempts HTTP access.

## Evidence

### Topology

![Topology](topology.png)

### PC0 Ping Test

![PC0 Ping Success](pc0-ping-success.png)

### PC0 HTTP Access Blocked

![PC0 HTTP Blocked](pc0-http-blocked.png)

### PC1 HTTP Access Allowed

![PC1 HTTP Allowed](pc1-http-allowed.png)

### R1 ACL Verification

![R1 Extended ACL](r1-extended-acl.png)

## Learning Outcomes

- Configured a named Extended ACL.
- Filtered traffic based on source IP address, destination IP address, protocol, and port number.
- Applied an Extended ACL inbound and close to the source network.
- Allowed ICMP traffic while blocking only HTTP TCP port 80 traffic for a selected host.
- Verified ACL policy enforcement using browser tests and ACL match counters.

## Files

- `extended-acl.pkt` — Cisco Packet Tracer lab file
- `topology.png` — Network topology screenshot
- `pc0-ping-success.png` — ICMP verification from blocked HTTP client
- `pc0-http-blocked.png` — Denied HTTP test from PC0
- `pc1-http-allowed.png` — Successful HTTP test from PC1
- `r1-extended-acl.png` — ACL rules and match-counter verification
