# Lab 05: Inter-VLAN Routing Using Router-on-a-Stick

## Objective
Configure inter-VLAN routing using the Router-on-a-Stick method. Create VLAN 10 and VLAN 20 on a switch, configure a trunk link to the router, and use router subinterfaces as default gateways for both VLANs.

## Topology
- 1 Cisco Router
- 1 Cisco 2960 Switch
- 4 PCs
- Copper straight-through cables

## VLAN and Network Plan

| VLAN ID | Department | Network | Default Gateway |
|---------|------------|---------|-----------------|
| 10 | HR | 192.168.10.0/24 | 192.168.10.1 |
| 20 | FINANCE | 192.168.20.0/24 | 192.168.20.1 |

## Port Assignment

| Switch Port | Connected Device | VLAN / Mode |
|-------------|------------------|-------------|
| FastEthernet0/1 | PC0 | VLAN 10 |
| FastEthernet0/2 | PC1 | VLAN 10 |
| FastEthernet0/3 | PC2 | VLAN 20 |
| FastEthernet0/4 | PC3 | VLAN 20 |
| FastEthernet0/24 | Router0 | Trunk |

## IP Addressing Plan

| Device | Interface | IP Address | Subnet Mask | Default Gateway |
|--------|-----------|------------|-------------|-----------------|
| Router0 | G0/0.10 | 192.168.10.1 | 255.255.255.0 | N/A |
| Router0 | G0/0.20 | 192.168.20.1 | 255.255.255.0 | N/A |
| PC0 | FastEthernet0 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| PC1 | FastEthernet0 | 192.168.10.11 | 255.255.255.0 | 192.168.10.1 |
| PC2 | FastEthernet0 | 192.168.20.10 | 255.255.255.0 | 192.168.20.1 |
| PC3 | FastEthernet0 | 192.168.20.11 | 255.255.255.0 | 192.168.20.1 |

## Switch Configuration

```text
enable
configure terminal

vlan 10
name HR
exit

vlan 20
name FINANCE
exit

interface range fastEthernet 0/1-2
switchport mode access
switchport access vlan 10
exit

interface range fastEthernet 0/3-4
switchport mode access
switchport access vlan 20
exit

interface fastEthernet 0/24
switchport mode trunk
exit

end
write memory
```

## Router Configuration

```text
enable
configure terminal

interface gigabitEthernet 0/0
no shutdown
exit

interface gigabitEthernet 0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
exit

interface gigabitEthernet 0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
exit

end
write memory
```

## Verification

### Switch Commands

```text
show vlan brief
show interfaces trunk
```

### Router Command

```text
show ip interface brief
```

Expected result: Router subinterfaces `G0/0.10` and `G0/0.20` should be operational, and `Fa0/24` should be a trunk port.

### End-to-End Connectivity Test

From PC0:

```text
ping 192.168.10.1
ping 192.168.20.1
ping 192.168.20.10
```

Expected result: All pings should succeed. The final ping confirms that devices in VLAN 10 and VLAN 20 can communicate through the router.

## Learning Outcomes

- Created and configured VLAN 10 and VLAN 20.
- Configured switch access ports for VLAN membership.
- Configured an 802.1Q trunk link between switch and router.
- Created router subinterfaces using `encapsulation dot1Q`.
- Configured default gateways for separate VLANs.
- Verified inter-VLAN communication using `ping`.

## Files

- `inter-vlan-routing.pkt` — Cisco Packet Tracer topology
- `topology.png` — Network topology screenshot
- `trunk-verification.png` — Trunk configuration verification
- `ping-test.png` — Successful inter-VLAN ping test
