# Day 8 — WAN & OSPF

## Tujuan

Menghubungkan HQ, Branch 1, dan Branch 2 menggunakan koneksi WAN serial serta mengimplementasikan dynamic routing menggunakan OSPF Area 0.

## Topologi WAN

PT Konoha menggunakan topologi WAN berbentuk triangle untuk menyediakan beberapa jalur komunikasi antar-site.

| Link | Network | IP Router A | IP Router B |
|---|---|---|---|
| HQ ↔ BR1 | 10.255.0.0/30 | HQ: 10.255.0.1 | BR1: 10.255.0.2 |
| HQ ↔ BR2 | 10.255.0.4/30 | HQ: 10.255.0.5 | BR2: 10.255.0.6 |
| BR1 ↔ BR2 | 10.255.0.8/30 | BR1: 10.255.0.9 | BR2: 10.255.0.10 |

## Transit Network Edge Router ↔ MLS

Selain koneksi WAN, setiap Edge Router terhubung ke perangkat Layer 3 menggunakan dedicated transit network.

| Link | Network | Router | MLS/Core |
|---|---|---|---|
| HQ-RTR ↔ HQ-CORE1 | 10.254.0.0/30 | 10.254.0.1 | 10.254.0.2 |
| HQ-RTR ↔ HQ-CORE2 | 10.254.0.4/30 | 10.254.0.5 | 10.254.0.6 |
| BR1-RTR ↔ BR1-MLS1 | 10.254.0.8/30 | 10.254.0.9 | 10.254.0.10 |
| BR2-RTR ↔ BR2-MLS1 | 10.254.0.12/30 | 10.254.0.13 | 10.254.0.14 |

Transit network digunakan sebagai jalur Layer 3 antara Edge Router dan perangkat MLS/Core.

## OSPF Design

Routing protocol yang digunakan adalah OSPF.

- Protocol: OSPF
- Process ID: 1
- Area: 0
- HQ-RTR Router ID: 1.1.1.1
- BR1-RTR Router ID: 2.2.2.2
- BR2-RTR Router ID: 3.3.3.3
- HQ-CORE1 Router ID: 11.11.11.11
- HQ-CORE2 Router ID: 12.12.12.12
- BR1-MLS1 Router ID: 22.22.22.22
- BR2-MLS1 Router ID: 33.33.33.33

## Pembagian OSPF

### Edge Router

Edge Router mengiklankan network WAN dan transit network menuju MLS/Core.

Edge Router tidak mengiklankan network VLAN user karena gateway VLAN berada pada MLS/Core.

### MLS/Core

MLS/Core mengiklankan network VLAN yang digunakan sebagai gateway masing-masing VLAN.

#### HQ-CORE1

10.10.10.0/26
10.10.20.0/26
10.10.30.0/25
10.10.40.0/26
10.10.50.0/24
10.10.60.0/27
10.10.70.0/28
10.10.80.0/28

#### BR1-MLS

10.20.10.0/28
10.20.20.0/28
10.20.30.0/27
10.20.40.0/27
10.20.50.0/26
10.20.60.0/28

#### BR2-MLS

10.30.10.0/28
10.30.20.0/28
10.30.30.0/27
10.30.40.0/27
10.30.50.0/26
10.30.60.0/28