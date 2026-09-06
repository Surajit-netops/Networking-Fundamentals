# Lab 11: Standard Numbered Access Control List

## Objective
Configure a Standard Numbered ACL on a Cisco router to block one host from reaching a remote LAN while allowing other hosts to communicate normally.

## Topology
- 2 Cisco Routers: R1 and R2
- 2 Cisco 2960 Switches: SW1 and SW2
- 4 PCs: PC0, PC1, PC2 and PC3
- One point-to-point WAN network between the routers

## Security Policy

| Source Device | IP Address | Access to LAN 2 |
|---|---|---|
| PC0 | 192.168.10.10 | Denied |
| PC1 | 192.168.10.11 | Allowed |

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

## Static Routing

### R1

```text
ip route 192.168.20.0 255.255.255.0 10.0.0.2
```

### R2

```text
ip route 192.168.10.0 255.255.255.0 10.0.0.1
```

## Standard ACL Configuration

The ACL is applied outbound on R2's LAN-facing interface. It blocks traffic sourced from PC0 and allows other source addresses.

```text
enable
configure terminal

access-list 10 deny host 192.168.10.10
access-list 10 permit any

interface gigabitEthernet 0/0
 ip access-group 10 out
exit

end
write memory
```

## Verification

### Blocked host test

From PC0:

```text
ping 192.168.20.10
```

Expected result: Ping fails because ACL 10 denies source IP address `192.168.10.10`.

### Allowed host test

From PC1:

```text
ping 192.168.20.10
```

Expected result: Ping succeeds because the `permit any` ACL entry permits PC1.

### ACL counter check

On R2:

```text
show ip access-lists
```

Expected result: The deny and permit entries show packet match counters after the ping tests.

## Evidence

### Topology

![Topology](topology.png)

### PC0 blocked by ACL

![PC0 denied ping](pc0-denied-ping.png)

### PC1 permitted by ACL

![PC1 permitted ping](pc1-permitted-ping.png)

### ACL rule matches on R2

![ACL counters](r2-acl-matches.png)

## Learning Outcomes

- Configured static routes between two LANs.
- Created a Standard Numbered ACL using ACL number 10.
- Filtered traffic based on source IP address.
- Applied a Standard ACL close to the destination network.
- Understood ACL rule order and the implicit deny behavior.
- Verified ACL operation using host ping tests and `show ip access-lists`.

## Files

- `standard-acl.pkt` — Cisco Packet Tracer lab file
- `topology.png` — Complete network topology
- `pc0-denied-ping.png` — Blocked connectivity test
- `pc1-permitted-ping.png` — Allowed connectivity test
- `r2-acl-matches.png` — ACL hit-counter verification
