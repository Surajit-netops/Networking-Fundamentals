# Lab 04: Basic VLAN Segmentation

## Objective
Create two VLANs on a Cisco switch, assign switch access ports to each VLAN, and verify that devices in the same VLAN can communicate while devices in different VLANs remain separated.

## Topology
- 1 Cisco 2960 Switch
- 4 PCs
- Copper straight-through cables

## VLAN Plan

| VLAN ID | VLAN Name | Assigned Devices | Network |
|---------|-----------|------------------|---------|
| 10 | HR | PC0, PC1 | 192.168.10.0/24 |
| 20 | FINANCE | PC2, PC3 | 192.168.20.0/24 |

## Port Assignment

| Switch Port | Connected Device | VLAN |
|-------------|------------------|------|
| FastEthernet0/1 | PC0 | 10 |
| FastEthernet0/2 | PC1 | 10 |
| FastEthernet0/3 | PC2 | 20 |
| FastEthernet0/4 | PC3 | 20 |

## IP Addressing Plan

| Device | VLAN | IP Address | Subnet Mask | Default Gateway |
|--------|------|------------|-------------|-----------------|
| PC0 | 10 | 192.168.10.10 | 255.255.255.0 | N/A |
| PC1 | 10 | 192.168.10.11 | 255.255.255.0 | N/A |
| PC2 | 20 | 192.168.20.10 | 255.255.255.0 | N/A |
| PC3 | 20 | 192.168.20.11 | 255.255.255.0 | N/A |

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

end
write memory
```

## Verification

### VLAN Verification

```text
show vlan brief
```

Expected result: Ports `Fa0/1` and `Fa0/2` should be assigned to VLAN 10. Ports `Fa0/3` and `Fa0/4` should be assigned to VLAN 20.

### Same-VLAN Connectivity Test

From PC0:

```text
ping 192.168.10.11
```

Expected result: Successful ping reply from PC1.

### Different-VLAN Connectivity Test

From PC0:

```text
ping 192.168.20.10
```

Expected result: Ping fails because inter-VLAN routing is not configured in this lab.

## Learning Outcomes

- Created VLANs on a Cisco switch.
- Assigned switch access ports to specific VLANs.
- Understood logical network segmentation using VLANs.
- Verified VLAN port membership using `show vlan brief`.
- Tested same-VLAN and different-VLAN connectivity.

## Files

- `basic-vlan.pkt` — Cisco Packet Tracer lab file
- `topology.png` — Network topology screenshot
- `vlan-verification.png` — Output of `show vlan brief`
- `ping-test.png` — Same-VLAN and different-VLAN ping results
