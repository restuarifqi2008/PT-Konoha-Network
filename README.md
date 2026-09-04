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
    
