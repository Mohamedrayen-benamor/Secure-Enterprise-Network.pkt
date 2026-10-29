
# Secure Enterprise Network Lab

A Cisco Packet Tracer project that explores network segmentation, inter-VLAN routing, and basic network security for a fictional company.

> **Project status:** Update this README to reflect your actual configuration and test results before publishing. Do not include passwords or other sensitive information in a public repository.

## 1. Goal and topology

The goal is to build a small enterprise network that separates departments and restricts access between them. The lab uses:

- One Cisco 2911 router for inter-VLAN routing and DHCP.
- One Cisco 2960 switch for VLANs, trunking, and port security.
- Four PCs representing IT, HR, Sales, and Guest users.
- One server representing a public website.

```text
IT PC ───────┐
HR PC ───────┤
Sales PC ────┤── SW1 ── 802.1Q trunk ── R1 ── Public server
Guest PC ────┘
```

The router uses **router-on-a-stick**: one physical router interface connects to the switch, with a separate subinterface for each VLAN. The public server is connected to another router interface. It is a simulated public service, not a real internet connection.

## 2. VLANs and IP addressing

| Department | VLAN | Network | Default gateway | Switch port |
|---|---:|---|---|---|
| IT | 10 | 10.10.10.0/24 | 10.10.10.1 | Fa0/1 |
| HR | 20 | 10.10.20.0/24 | 10.10.20.1 | Fa0/2 |
| Sales | 30 | 10.10.30.0/24 | 10.10.30.1 | Fa0/3 |
| Guest | 40 | 10.10.40.0/24 | 10.10.40.1 | Fa0/4 |

| Device/interface | Address | Purpose |
|---|---|---|
| SW1 VLAN 10 | 10.10.10.2/24 | Switch management |
| R1 GigabitEthernet0/1 | 203.0.113.1/24 | Server-facing gateway |
| Public server | 203.0.113.10/24 | Simulated public web server |

The subnet mask for each `/24` network is `255.255.255.0`. The switch-to-router connection is intended to be an 802.1Q trunk carrying VLANs 10, 20, 30, and 40. Device port names may differ depending on the Packet Tracer model and cabling.

## 3. Security policies

The intended lab policies are:

- **Segmentation:** Each department has its own VLAN.
- **HR and Sales isolation:** ACLs block IP traffic between the HR and Sales subnets in both directions.
- **Guest isolation:** Guest traffic is blocked from the internal `10.10.0.0/16` range, while access to the simulated public server is permitted. DHCP requests are permitted separately.
- **Secure management:** SSH is used instead of Telnet, and management access is restricted to the IT subnet using a VTY access list.
- **Port security:** Access ports are configured to learn a maximum of one MAC address using sticky learning and to restrict violations.

These are intended policies; record which ones you actually configured and verified in the test table below.

## 4. Configuration highlights

- Created VLANs and assigned switch access ports to departments.
- Configured an 802.1Q trunk between the switch and router.
- Created router subinterfaces and gateway addresses for inter-VLAN routing.
- Set up DHCP pools for client addressing.
- Applied inbound extended ACLs to the HR, Sales, and Guest subinterfaces.
- Configured SSH and restricted VTY access to IT.
- Configured switch port security on the PC-facing ports.

Useful verification commands:

```cisco
show ip interface brief
show ip dhcp pool
show ip dhcp binding
show access-lists
show vlan brief
show interfaces trunk
show port-security
```

## 5. Test results and screenshots

Replace **Not yet recorded** with your actual result after performing each test. Include the date and any troubleshooting notes if useful.

| Test | Expected result | Actual result |
|---|---|---|
| IT PC obtains an address by DHCP | Address in 10.10.10.0/24 | Not yet recorded |
| HR, Sales, and Guest PCs obtain addresses by DHCP | Addresses in their respective subnets | Not yet recorded |
| IT PC pings HR PC | Allowed | Not yet recorded |
| Sales PC pings HR PC | Blocked | Not yet recorded |
| HR PC pings Sales PC | Blocked | Not yet recorded |
| Guest PC pings IT PC | Blocked | Not yet recorded |
| Guest PC opens `http://203.0.113.10` | Allowed | Not yet recorded |
| IT PC connects to R1 and SW1 using SSH | Allowed | Not yet recorded |
| Non-IT PC attempts SSH management | Blocked | Not yet recorded |
| A second MAC address uses a secured switch port | Restricted; violation counter increases | Not yet recorded |

Suggested screenshots to add under a `screenshots/` folder:

- Network topology.
- `show vlan brief` and `show interfaces trunk`.
- DHCP addressing on a PC.
- Successful and blocked connectivity tests.
- SSH and port-security verification.

After adding a screenshot, you can display it in this README using:

```markdown
Capture d'écran 2026-10-29 114041.png

```

## 6. Learning and limitations

### Learning goals

- Understand how VLANs separate broadcast domains and how trunk links carry multiple VLANs.
- Configure router-on-a-stick and DHCP for multiple subnets.
- Practice translating access requirements into ACL rules.
- Verify security controls and troubleshoot DHCP, trunks, and connectivity.

### Limitations

- This is a **simulation**, not a production deployment or a real internet connection.
- The public server is directly connected to the lab router; it does not represent an internet service provider.
- Basic ACLs and port security are not substitutes for a stateful firewall, intrusion detection, centralized authentication, or endpoint security.
- The Guest policy blocks the defined internal address range; additional rules would be needed for other internal ranges or more restrictive access.
- Packet Tracer device behavior and available commands can differ from physical Cisco hardware.

## Repository contents

```text
secure-enterprise-network/
├── README.md
├── Secure-Enterprise-Network.pkt
└── screenshots/
```


