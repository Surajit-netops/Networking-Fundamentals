# Lab 06: VTP Server, Client and Transparent Mode

## Objective
Configure VTP Server, Client, and Transparent modes across three Cisco switches. Verify VLAN propagation from the VTP Server to the VTP Client and verify that the VTP Transparent switch maintains its own local VLAN database.

## Topology
- 3 Cisco 2960 Switches
- 2 PCs
- Copper straight-through cables
- Switch-to-switch trunk links

## Device Roles

| Device | Role |
|--------|------|
| SW0 | VTP Server |
| SW1 | VTP Client |
| SW2 | VTP Transparent |

## VTP Settings

| Setting | Value |
|---------|-------|
| VTP Domain | CCNA-LAB |
| VTP Password | cisco123 |
| VLAN 10 | USERS |
| VLAN 20 | SERVERS |

## Port Assignment

| Switch | Port | Connected To | Mode |
|--------|------|--------------|------|
| SW0 | Fa0/1 | PC0 | Access VLAN 10 |
| SW0 | Fa0/23 | SW1 Fa0/23 | Trunk |
| SW1 | Fa0/23 | SW0 Fa0/23 | Trunk |
| SW1 | Fa0/24 | SW2 Fa0/24 | Trunk |
| SW2 | Fa0/24 | SW1 Fa0/24 | Trunk |
| SW2 | Fa0/1 | PC1 | Access VLAN 10 |

## PC IP Addressing

| Device | VLAN | IP Address | Subnet Mask | Default Gateway |
|--------|------|------------|-------------|-----------------|
| PC0 | 10 | 192.168.10.10 | 255.255.255.0 | N/A |
| PC1 | 10 | 192.168.10.11 | 255.255.255.0 | N/A |

## Configuration Summary

### SW0 — VTP Server

```text
vtp domain CCNA-LAB
vtp mode server
vtp password cisco123

vlan 10
name USERS

vlan 20
name SERVERS
```

### SW1 — VTP Client

```text
vtp domain CCNA-LAB
vtp mode client
vtp password cisco123
```

### SW2 — VTP Transparent

```text
vtp domain CCNA-LAB
vtp mode transparent
vtp password cisco123

vlan 10
name USERS
```

## Verification Commands

```text
show vtp status
show vlan brief
show interfaces trunk
```

## Verification Results

- SW0 was configured as the VTP Server.
- SW1 received VLAN 10 and VLAN 20 information as the VTP Client.
- SW2 operated in VTP Transparent mode, so VLAN 10 was manually created locally.
- Trunk links carried VLAN 10 traffic between switches.
- PC0 successfully pinged PC1 across the switched network.

## Screenshots

### Network Topology
![Topology](topology.png)

### VTP Server Status
![VTP Server Status](vtp-server-status.png)

### Trunk Verification
![Trunk Verification](trunk-verification.png)

### End-to-End Ping Test
![Ping Test](ping-test.png)

## Learning Outcomes

- Configured Cisco VTP Server, Client, and Transparent modes.
- Created VLANs on a VTP Server and verified VLAN propagation to a VTP Client.
- Understood that a VTP Transparent switch requires local VLAN creation.
- Configured and verified trunk links.
- Verified same-VLAN connectivity across multiple switches.

## Files

- `vtp-lab.pkt` — Cisco Packet Tracer lab file
- `topology.png` — Network topology
- `vtp-server-status.png` — VTP Server verification
- `trunk-verification.png` — Trunk link verification
- `ping-test.png` — PC0-to-PC1 connectivity test
