# College Computer Lab Network

A secured college network built in Cisco Packet Tracer for the CCN (Computer Communication Networks) Level 2 project. Student, Admin and Server devices are separated into VLANs, connected through one router, given addresses by DHCP, and protected by an extended ACL. A DNS server and a web server let students open the college portal by name.

![Network topology](docs/screenshots/01_topology.png)

## Objectives

- Design a structured network for a college computer lab.
- Separate students, administration and server devices using VLANs.
- Configure automatic IP address allocation using DHCP, including the DNS server address.
- Enable inter-VLAN communication through the router, with one router interface per VLAN.
- Implement network security using an extended ACL.
- Host a college learning portal using an HTTP server.
- Configure a DNS server so the portal opens by domain name (`www.pesuniversity.edu`) instead of an IP address.

## Technologies used

- VLANs (10 STUDENT, 20 ADMIN, 30 SERVER)
- IPv4 addressing and subnetting (three /24 networks)
- Inter-VLAN routing using one router interface per VLAN
- DHCP (router as DHCP server)
- DNS and HTTP services on Server0
- Extended access control list (ACL)

## Addressing

| Network | VLAN | Network ID | Gateway | DHCP range |
|---|---|---|---|---|
| Student | 10 | 192.168.10.0/24 | 192.168.10.1 | .100 - .254 |
| Admin | 20 | 192.168.20.0/24 | 192.168.20.1 | not used |
| Server | 30 | 192.168.30.0/24 | 192.168.30.1 | not used |

| Device | IP address | Assignment |
|---|---|---|
| COLLEGE-ROUTER Gi0/0, Gi0/1, Gi0/2 | 192.168.10.1, 192.168.20.1, 192.168.30.1 | Static |
| student_pc1, student_pc2 | 192.168.10.100, 192.168.10.101 | DHCP |
| admin_pc | 192.168.20.10 | Static |
| Server0 (DNS + web) | 192.168.30.100 | Static |

## DNS

Server0 (192.168.30.100) runs the DNS service with one A record:

| Name | Type | Address |
|---|---|---|
| www.pesuniversity.edu | A | 192.168.30.100 |

The router's DHCP pools hand out `192.168.30.100` as the DNS server, so students can open `http://www.pesuniversity.edu` in the Web Browser of student_pc1.

## Security (ACL)

The extended ACL `STUDENT_SECURITY` is applied inbound on Gi0/0 (student network). Students can ping the Admin PC, but every other kind of traffic to it is blocked. Access to the rest of the network is not affected.

## How to run

1. Open `College_Lab_Network.pkt` in Cisco Packet Tracer 9.0.1 or newer.
2. On `student_pc1`, open Desktop > Command Prompt and run `ipconfig /all` to see the DHCP lease.
3. Run the tests below.

| From | Action | Expected |
|---|---|---|
| student_pc1 | `ping 192.168.10.1` | replies |
| student_pc1 | `ping 192.168.20.10` | replies (ICMP allowed) |
| student_pc1 | `ping 192.168.30.100` | replies |
| student_pc1 | `tracert 192.168.30.100` | hop 1 = 192.168.10.1, hop 2 = 192.168.30.100 |
| student_pc1 | Web Browser: `http://www.pesuniversity.edu` | portal page loads |
| student_pc1 | Web Browser: `http://192.168.20.10` | blocked by the ACL |

Screenshots of every test are in [`docs/screenshots`](docs/screenshots).

## Repository contents

```
College_Lab_Network.pkt   Packet Tracer project file
configs/                  router and switch configs, end-device settings
docs/                     full project report (Word) and screenshots
LICENSE
```

Full write-up: [`docs/CCN_Level2_Project_Report.docx`](docs/CCN_Level2_Project_Report.docx)

## Known limitations

Discussed in Section 9.2 of the report:

- The ACL protects only the single host 192.168.20.10, not the whole Admin subnet.
- One ACL line (`permit tcp ... host 192.168.10.0 established`) matches no traffic.
- Server0's address 192.168.30.100 is inside the unused SERVER_POOL range.
- No device passwords or SSH, and no redundancy.

## Authors

| Name | USN | Class |
|---|---|---|
| Pooja P | PES1UG24EC145 | 5D |
| Prateek T | PES1UG24EC151 | 5D |

## License

[MIT](LICENSE). The "PES University" name on the demo web page is used only as a mock portal for coursework and implies no affiliation or endorsement.
