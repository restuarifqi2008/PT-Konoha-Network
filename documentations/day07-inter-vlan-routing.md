# Day 7 — Inter-VLAN Routing

## Tujuan

Mengaktifkan routing antar-VLAN menggunakan SVI pada multilayer switch di HQ, Branch 1, dan Branch 2.

## Konfigurasi

### HQ

Device: `KNOHA-HQ-CORE1`

- `ip routing` enabled
- SVI VLAN 10–80 configured
- VLAN 999 tidak diberikan IP address

### Branch 1

Device: `KNOHA-BR1-MLS1`

- `ip routing` enabled
- SVI VLAN 10–60 configured
- VLAN 70 dan VLAN 80 tidak digunakan di branch

### Branch 2

Device: `KNOHA-BR2-MLS1`

- `ip routing` enabled
- SVI VLAN 10–60 configured
- VLAN 70 dan VLAN 80 tidak digunakan di branch

## Gateway

### HQ

| VLAN | Gateway |
|---|---|
| 10 | 10.10.10.1 |
| 20 | 10.10.20.1 |
| 30 | 10.10.30.1 |
| 40 | 10.10.40.1 |
| 50 | 10.10.50.1 |
| 60 | 10.10.60.1 |
| 70 | 10.10.70.1 |
| 80 | 10.10.80.1 |

### Branch 1

| VLAN | Gateway |
|---|---|
| 10 | 10.20.10.1 |
| 20 | 10.20.20.1 |
| 30 | 10.20.30.1 |
| 40 | 10.20.40.1 |
| 50 | 10.20.50.1 |
| 60 | 10.20.60.1 |

### Branch 2

| VLAN | Gateway |
|---|---|
| 10 | 10.30.10.1 |
| 20 | 10.30.20.1 |
| 30 | 10.30.30.1 |
| 40 | 10.30.40.1 |
| 50 | 10.30.50.1 |
| 60 | 10.30.60.1 |

## Testing

Inter-VLAN connectivity berhasil diuji menggunakan ICMP/Ping.

Testing dilakukan pada:

- HQ: VLAN 10 → VLAN 20
- Branch 1: VLAN 10 → VLAN 20
- Branch 2: VLAN 10 → VLAN 20

Seluruh pengujian berhasil tanpa packet loss.

## Kesimpulan

Inter-VLAN Routing berhasil diterapkan pada seluruh site. Multilayer switch berfungsi sebagai Layer 3 gateway untuk masing-masing VLAN.

Routing antar-site belum dilakukan pada tahap ini dan akan dikonfigurasi menggunakan OSPF pada tahap WAN.