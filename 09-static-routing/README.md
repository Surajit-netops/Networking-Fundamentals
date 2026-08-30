# Lab 09: Static Routing and Default Routing Between Two Routers

## Objective
Configure static routing and default routing between two Cisco routers connecting two separate LAN segments across a point-to-point WAN link. Verify route tables and test end-to-end IP reachability.

## Topology
- 2 Cisco Routers (R1, R2)
- 2 Cisco Catalyst 2960 Switches (SW1, SW2)
- 4 PCs (PC0, PC1, PC2, PC3)
- LAN subnets interconnected via a /30 WAN link

## IP Addressing Plan

| Device | Interface | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|
| R1 | GigabitEthernet0/0 | 192.168.10.1 | 255.255.255.0 | N/A |
| R1 | GigabitEthernet0/1 | 10.0.0.1 | 255.255.255.252 | N/A |
| R2 | GigabitEthernet0/1 | 10.0.0.2 | 255.255.255.252 | N/A |
| R2 | GigabitEthernet0/0 | 192.168.20.1 | 255.255.255.0 | N/A |
| PC0 | FastEthernet0 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| PC1 | FastEthernet0 | 192.168.10.11 | 255.255.255.0 | 192.168.10.1 |
| PC2 | FastEthernet0 | 192.168.20.10 | 255.255.255.0 | 192.168.20.1 |
| PC3 | FastEthernet0 | 192.168.20.11 | 255.255.255.0 | 192.168.20.1 |

---

## Router Configurations

### 1. Router 1 (R1)
```text
enable
configure terminal
hostname R1

interface gigabitEthernet 0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown
 exit

interface gigabitEthernet 0/1
 ip address 10.0.0.1 255.255.255.252
 no shutdown
 exit

ip route 192.168.20.0 255.255.255.0 10.0.0.2
end
write memory
```

### 2. Router 2 (R2)
```text
enable
configure terminal
hostname R2

interface gigabitEthernet 0/0
 ip address 192.168.20.1 255.255.255.0
 no shutdown
 exit

interface gigabitEthernet 0/1
 ip address 10.0.0.2 255.255.255.252
 no shutdown
 exit

ip route 0.0.0.0 0.0.0.0 10.0.0.1
end
write memory
```

---

## Verification & Commands

### 1. Routing Table Verification
```text
R1# show ip route
R2# show ip route
```
- Confirmed static route entry `S` on R1 pointing to `192.168.20.0/24` via `10.0.0.2`.
- Confirmed default static route `S*` on R2 pointing to `0.0.0.0/0` via `10.0.0.1`.

### 2. End-to-End Connectivity
From PC0:
```text
ping 192.168.20.10
```
- Ping successful across both routers with 0% packet loss.

---

## Evidence / Screenshots

### 1. Topology
![Topology](topology.png)

### 2. R1 Routing Table (Static Route S)
![R1 Route](r1-routing-table.png)

### 3. End-to-End Ping Test
![Ping Test](ping-test.png)

---

## Learning Outcomes
- Understood the concept of hop-by-hop packet forwarding.
- Configured point-to-point `/30` WAN subnets between routers.
- Applied destination-specific static routes and default gateway static routes (`0.0.0.0 0.0.0.0`).
- Verified Cisco IOS routing table flags (`C` for Connected, `S` for Static, `S*` for Default Candidate).

## Files
- `static-routing.pkt` — Cisco Packet Tracer Lab File
- `topology.png` — Network layout screenshot
- `r1-routing-table.png` — R1 static route verification
- `ping-test.png` — End-to-end ping verification
