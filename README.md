# GNS3 Enterprise LAN with VLANs and OSPF

A GNS3 network simulation of a segmented enterprise LAN using VLANs, Router-on-a-Stick inter-VLAN routing, DHCP, and single-area OSPF.

## Topology

- Two Cisco routers: distribution router `R1` and OSPF peer `R2`
- Two Layer 2 access switches
- Four VPCS clients
- VLAN 10 for staff
- VLAN 20 for students
- VLAN 30 for servers
- An 802.1Q trunk between the access layer and `R1`
- A routed `/30` link between `R1` and `R2`

## Addressing plan

| Segment | Network | Gateway or endpoint |
|---|---|---|
| VLAN 10 | `192.168.10.0/24` | `192.168.10.1` |
| VLAN 20 | `192.168.20.0/24` | `192.168.20.1` |
| VLAN 30 | `192.168.30.0/24` | `192.168.30.1` |
| R1-R2 link | `10.0.0.0/30` | `10.0.0.1` / `10.0.0.2` |
| R2 loopback | `172.16.1.0/24` | `172.16.1.1` |

## Implemented concepts

- 802.1Q VLAN trunking
- Router-on-a-Stick subinterfaces
- Per-VLAN DHCP pools
- OSPFv2 Area 0 route exchange
- Cross-VLAN and routed-connectivity testing

## Repository contents

- `configs/R1.cfg`: distribution-router startup configuration
- `configs/R2.cfg`: OSPF-peer startup configuration
- `configs/vpcs/`: VPCS startup files

## Open the lab

1. Install GNS3 and import compatible router appliances locally.
2. Create two routers, two Ethernet switches, and four VPCS nodes.
3. Connect the devices according to the topology and addressing plan above.
4. Apply `configs/R1.cfg` and `configs/R2.cfg` to the routers.
5. Apply the startup files from `configs/vpcs/` to the corresponding clients.
6. Start the nodes and run the verification checks below.

Cisco IOS images and the local GNS3 runtime archive are not included. Appliance images must be supplied separately under the applicable license.

## Verification targets

- Clients obtain addresses from the correct VLAN DHCP pool.
- Hosts communicate across VLANs through `R1`.
- `R1` and `R2` reach a full OSPF adjacency.
- VLAN clients reach the `172.16.1.1` loopback through OSPF-learned routes.

