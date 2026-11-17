# End-device settings

| Device | Mode | IPv4 address | Mask | Default gateway | DNS server |
|---|---|---|---|---|---|
| student_pc1 | DHCP | 192.168.10.100 (leased) | 255.255.255.0 | 192.168.10.1 | 192.168.30.100 |
| student_pc2 | DHCP | 192.168.10.101 (leased) | 255.255.255.0 | 192.168.10.1 | 192.168.30.100 |
| admin_pc | Static | 192.168.20.10 | 255.255.255.0 | 192.168.20.1 | 192.168.30.100 |
| Server0 | Static | 192.168.30.100 | 255.255.255.0 | 192.168.30.1 | 192.168.30.100 |

## Server0 services
- **DNS**: enabled, A record `www.pesuniversity.edu` -> `192.168.30.100`
- **HTTP**: enabled, serves the PES University portal page
