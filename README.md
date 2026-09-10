# PT Konoha Network

> Simulasi jaringan enterprise multi-site menggunakan Cisco Packet Tracer.

## 📌 Tentang Project

PT Konoha Network adalah project simulasi jaringan enterprise yang dirancang untuk menggambarkan infrastruktur jaringan perusahaan dengan satu Head Office (HQ) dan dua Branch Office.

Project ini dibuat menggunakan **Cisco Packet Tracer** dengan menerapkan konsep:

- VLAN Segmentation
- Hierarchical Network Design
- Multilayer Switching
- Inter-VLAN Routing
- EtherChannel
- Spanning Tree Protocol (STP)
- OSPF
- DHCP
- DNS
- Web Server
- File Server
- Mail Server
- Monitoring
- SSH
- ACL
- Port Security
- Guest Wireless Network
- Network Management

Project dikembangkan secara bertahap dengan pendekatan implementasi dan troubleshooting seperti pada lingkungan jaringan enterprise.

---

## 🏢 Arsitektur Jaringan

Project terdiri dari tiga lokasi:

```text
                    PT KONOHA NETWORK

                        ┌─────────┐
                        │   HQ    │
                        └────┬────┘
                             │
                  ┌──────────┴──────────┐
                  │                     │
             ┌────▼────┐           ┌────▼────┐
             │  BR1    │           │  BR2    │
             └─────────┘           └─────────┘
    
## Progress

- [x] Day 1 — Network Requirements
- [x] Day 2 — Physical Topology
- [x] Day 3 — VLAN & IP Addressing Plan
- [x] Day 4 — Basic Switch Configuration
- [x] Day 5 — EtherChannel (LACP)
- [x] Day 6 — STP
- [x] Day 7 — Inter-VLAN Routing
- [x] Day 8 — WAN & OSPF
- [x] Day 9 — DHCP & DHCP Relay
- [ ] Day 10 — DNS & Web Server
- [ ] Day 11 — File, Mail & Monitoring Server
- [ ] Day 12 — SSH & Network Management
- [ ] Day 13 — ACL & Network Segmentation
- [ ] Day 14 — Wireless & Guest Network
- [ ] Day 15 — Full Connectivity & Security Testing

### Advanced Enterprise Enhancement

- [ ] Day 16 — HSRP Gateway Redundancy
- [ ] Day 17 — Firewall & DMZ Security
- [ ] Day 18 — Network Monitoring, Syslog & SNMP
- [ ] Day 19 — QoS & Traffic Prioritization
- [ ] Day 20 — High Availability & Final Enterprise Optimization