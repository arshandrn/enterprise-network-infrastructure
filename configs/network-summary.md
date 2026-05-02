# Network Configuration Summary

## VLAN & IP Scheme
| VLAN | Department | Network | Gateway |
|------|-----------|---------|---------|
| VLAN 1 | HR | 192.168.10.0/24 | 192.168.10.1 |
| VLAN 2 | Finance | 192.168.20.0/24 | 192.168.20.1 |
| VLAN 3 | IT | 192.168.30.0/24 | 192.168.30.1 |
| VLAN 4 | Guest | 192.168.40.0/24 | 192.168.40.1 |
| VLAN 5 | IoT | 192.168.50.0/24 | 192.168.50.1 |

## Devices Used
- Access Layer: Cisco 2960 switches (HR, Finance, IT, Core)
- Core Layer: Cisco 3650 Multi-Layer Switch (inter-VLAN routing + DHCP)
- Edge Layer: 2x Cisco ISR4331 routers (HSRP primary + standby)

## Key Configurations

### HSRP (High Availability)
- Router 1: Priority 110 - Active gateway
- Router 2: Priority 100 - Standby
- Failover time: 3 seconds
- Virtual IP: 10.0.0.1

### ACL (Zero-Trust for Guest VLAN)
- Rule 1: DENY - Guest VLAN to any internal subnet
- Rule 2: PERMIT - Guest VLAN to internet (0.0.0.0/0)

### Port Security
- Mode: mac-address sticky
- Max devices per port: 1
- Violation policy: shutdown (err-disabled)

### NAT/PAT
- Type: NAT Overload on active edge router
- Translates all internal private IPs to single public ISP address

### DHCP
- Server: Core Switch (Cisco 3650)
- Leases: IP address + Subnet Mask + DNS (8.8.8.8) to all VLANs
