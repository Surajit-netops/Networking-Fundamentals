# Lab 03: Router as DHCP Server

## Objective
Configure a Cisco router as a DHCP server to automatically assign IP addresses, subnet masks, default gateway, and DNS server information to client PCs.

## Topology
- 1 Cisco Router
- 1 Cisco 2960 Switch
- 4 PCs
- Copper straight-through cables

## Network Details

| Setting | Value |
|--------|-------|
| Network Address | 192.168.30.0/24 |
| Router Interface | GigabitEthernet0/0 |
| Default Gateway | 192.168.30.1 |
| DHCP Pool Name | LAN-POOL |
| Excluded Address Range | 192.168.30.1 - 192.168.30.10 |
| DNS Server | 8.8.8.8 |

## IP Addressing Plan

| Device | Interface | IP Address | Subnet Mask | Default Gateway |
|--------|-----------|------------|-------------|-----------------|
| Router0 | GigabitEthernet0/0 | 192.168.30.1 | 255.255.255.0 | N/A |
| PC0 | FastEthernet0 | DHCP | DHCP | DHCP |
| PC1 | FastEthernet0 | DHCP | DHCP | DHCP |
| PC2 | FastEthernet0 | DHCP | DHCP | DHCP |
| PC3 | FastEthernet0 | DHCP | DHCP | DHCP |

## Router Configuration

```text
enable
configure terminal

interface gigabitEthernet 0/0
ip address 192.168.30.1 255.255.255.0
no shutdown
exit

ip dhcp excluded-address 192.168.30.1 192.168.30.10

ip dhcp pool LAN-POOL
network 192.168.30.0 255.255.255.0
default-router 192.168.30.1
dns-server 8.8.8.8
exit

end
write memory
```

## Client Configuration

1. Open each PC in Cisco Packet Tracer.
2. Go to `Desktop` → `IP Configuration`.
3. Select `DHCP`.
4. Verify that each PC receives an IP address automatically.

## Verification

### Router Commands

```text
show ip dhcp pool
show ip dhcp binding
show ip interface brief
```

### Client Commands

```text
ipconfig
ping 192.168.30.1
```

Expected result: Each PC receives an address from the `192.168.30.0/24` network and can ping the router gateway successfully.

## Learning Outcomes

- Configured a router interface with an IPv4 address.
- Created and configured a DHCP pool.
- Excluded gateway and reserved addresses from DHCP allocation.
- Configured default gateway and DNS server options for DHCP clients.
- Verified DHCP address allocation using `show ip dhcp binding`.
- Tested client-to-gateway connectivity using `ping`.

## Files

- `router-dhcp.pkt` — Cisco Packet Tracer topology
- `topology.png` — Lab topology screenshot
- `dhcp-binding.png` — Router DHCP binding verification
- `ping-test.png` — Client-to-gateway connectivity test
