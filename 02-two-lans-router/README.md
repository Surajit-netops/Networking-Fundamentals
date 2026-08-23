# Lab 02: Two LANs Connected Using a Router

## Objective
Create two separate LANs and enable communication between them using a Cisco router.

## Topology
- 1 Cisco Router
- 2 Cisco 2960 Switches
- 4 PCs
- Copper straight-through cables

## Network Diagram
![Topology](topology.png)

## IP Addressing Plan

| Device | Interface | IP Address | Subnet Mask | Default Gateway |
|--------|-----------|------------|-------------|-----------------|
| Router0 | GigabitEthernet0/0 | 192.168.10.1 | 255.255.255.0 | N/A |
| Router0 | GigabitEthernet0/1 | 192.168.20.1 | 255.255.255.0 | N/A |
| PC0 | FastEthernet0 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| PC1 | FastEthernet0 | 192.168.10.11 | 255.255.255.0 | 192.168.10.1 |
| PC2 | FastEthernet0 | 192.168.20.10 | 255.255.255.0 | 192.168.20.1 |
| PC3 | FastEthernet0 | 192.168.20.11 | 255.255.255.0 | 192.168.20.1 |

## Router Configuration

```text
enable
configure terminal

interface gigabitEthernet 0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
exit

interface gigabitEthernet 0/1
ip address 192.168.20.1 255.255.255.0
no shutdown
exit

end
write memory
```

## Verification

From PC0:

```text
ping 192.168.10.1
ping 192.168.20.1
ping 192.168.20.10
```

Successful replies confirm that devices in both LANs can communicate through the router.

## Router Verification Commands

```text
show ip interface brief
show ip route
```

Expected result:
- Both router interfaces should show `up/up`.
- Both networks should appear as directly connected routes.

## Learning Outcomes
- Understood communication between different IP networks.
- Configured IPv4 addresses on router interfaces.
- Configured default gateways on end devices.
- Verified router-based connectivity using `ping`.
- Used `show ip interface brief` and `show ip route` for basic troubleshooting.

## Files
- `two-lans-router.pkt` — Cisco Packet Tracer topology
- `topology.png` — Network diagram
- `ping-test.png` — End-to-end connectivity verification
