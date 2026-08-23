
# Lab 01: Basic LAN Connectivity

## Objective
Create a basic Local Area Network (LAN) using one Cisco switch and two PCs. Configure IPv4 addresses and verify connectivity using ping.

## Devices Used
- 1 Cisco 2960 Switch
- 3 PCs
- Copper straight-through cables

## IP Addressing Plan

| Device | IP Address | Subnet Mask | Default Gateway |
|--------|------------|-------------|-----------------|
| PC0 | 192.168.10.2 | 255.255.255.0 | 192.168.10.1 |
| PC1 | 192.168.10.3 | 255.255.255.0 | 192.168.10.1 |
| PC2 | 192.168.10.4 | 255.255.255.0 | 192.168.10.1 |

## Configuration Steps
1. Connected both PCs to the switch using copper straight-through cables.
2. Configured IPv4 addresses on PC0 and PC1.
3. Verified that both PCs are in the same subnet.
4. Tested connectivity from PC0 to PC1 using ping.

## Verification

```text
ping 192.168.1.3
```

Expected result: Successful replies with 0% packet loss.

## Commands Used

```text
ipconfig
ping 192.168.1.3
```

## Learning Outcomes
- Understood basic LAN connectivity.
- Configured IPv4 addresses on end devices.
- Verified same-subnet communication using ping.
- Used `ping` and `ipconfig` for basic troubleshooting.
