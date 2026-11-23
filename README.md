# Lab 3: SRWE Final PTSA

## Objective
Build and configure a small dual-stack network in Packet Tracer Physical
mode: one router, two switches and two PCs. The router provides
router-on-a-stick inter-VLAN routing and DHCP, the switches share a
LACP EtherChannel, and every device is hardened with SSH.

## IP addressing and subnetting
The lab gave each router subinterface an address with a prefix. The
prefix fixes the block size, which fixes the network and usable range.

| VLAN | Name | Network | Block | Usable range | Broadcast | Assignments |
|------|------|---------|-------|--------------|-----------|-------------|
| 2 | Bikes | 10.19.8.0/26 | 64 | .1 - .62 | .63 | R1 gateway .1 |
| 3 | Trikes | 10.19.8.64/27 | 32 | .65 - .94 | .95 | R1 gateway .65 |
| 4 | Management | 10.19.8.96/29 | 8 | .97 - .102 | .103 | R1 .97, S1 .98, S2 .99 |
| 5 | Parking | none | - | - | - | unused ports (shut down) |
| 6 | Native | none | - | - | - | native VLAN, no IP |

The three subnets are contiguous and do not overlap (0-63, 64-95, 96-103).

**IPv6:** VLAN 2 `2001:db8:acad:a::/64`, VLAN 3 `2001:db8:acad:b::/64`,
VLAN 4 `2001:db8:acad:c::/64`, router interfaces use `::1` and link-local
`fe80::1`. Loopback0 is `209.165.201.1/27` and `2001:db8:acad:209::1/64`.

### DHCP pools (last 10 host addresses)
| Pool | Range | Excluded | Gateway | Domain |
|------|-------|----------|---------|--------|
| CCNA-A | 10.19.8.53 - .62 | .1 - .52 | 10.19.8.1 | ccna-a.net |
| CCNA-B | 10.19.8.85 - .94 | .65 - .84 | 10.19.8.65 | ccna-b.net |

Excluding every address before the last ten leaves exactly ten in each pool.

## What I configured
- **R1:** no domain lookup, hostname, MOTD banner, console password,
  enable secret, password encryption, minimum password length 10,
  SSHv2 (1024-bit RSA, local user, SSH-only VTY), Loopback0, four
  subinterfaces (VLANs 2, 3, 4 and native 6) with IPv4, IPv6 and
  descriptions, default routes out Lo0, two DHCP pools
- **S1 and S2:** the same hardening and SSH, VLANs 2-6, management SVI
  on VLAN 4 with a default gateway, 802.1Q trunks with native VLAN 6, a
  LACP EtherChannel on F0/1-2, port security (three sticky addresses)
  on the host ports, unused ports in VLAN 5 and shut down
- **PC-A and PC-B:** IPv4 by DHCP, static IPv6 with gateway `fe80::1`

## Verification
[FILL IN with what you actually saw. Delete rows you did not test.]

| Test | Result |
|------|--------|
| PC-A DHCP address (expected 10.19.8.53 - .62) | [FILL IN] |
| PC-B DHCP address (expected 10.19.8.85 - .94) | [FILL IN] |
| `show etherchannel summary` | [FILL IN] |
| Ping from PC-A to the gateway and to the loopback | [FILL IN] |
| SSH to the switch SVI | [FILL IN] |
| Check Results score | [FILL IN or remove] |

## Troubleshooting story
[FILL IN with one real problem from your session: the symptom, the
command output that pointed to the cause, and the fix. If nothing went
wrong, delete this section.]

## Lessons learned
[FILL IN with 3 to 4 points from your own session.]

## Files
- [`configs/`](configs/): device command lists (secrets redacted)
- [`verification/`](verification/): show command output
- [`screenshots/`](screenshots/): evidence from Packet Tracer
