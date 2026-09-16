# Day 15 — HSRP Gateway Redundancy

## Overview

Pada Day 15 dilakukan implementasi HSRP (Hot Standby Router Protocol) pada multilayer switch Core PT Konoha untuk menyediakan gateway redundancy pada jaringan HQ.

HSRP digunakan untuk menyediakan satu Virtual IP (VIP) sebagai default gateway yang digunakan oleh end device. Dalam kondisi normal, KNOHA-HQ-CORE1 berfungsi sebagai Active Router, sedangkan KNOHA-HQ-CORE2 berfungsi sebagai Standby Router.

Konfigurasi HSRP menggunakan priority yang lebih tinggi pada CORE1 dan fitur preemption agar CORE1 dapat kembali menjadi Active setelah kondisi jaringan normal.

## HSRP Design

| VLAN | Network | CORE1 | CORE2 | HSRP Virtual IP |
|---|---|---|---|---|
| VLAN 10 IT | 10.10.10.0/26 | 10.10.10.1 | 10.10.10.2 | 10.10.10.62 |
| VLAN 20 HR | 10.10.20.0/26 | 10.10.20.1 | 10.10.20.2 | 10.10.20.62 |
| VLAN 30 OPERASIONAL | 10.10.30.0/25 | 10.10.30.1 | 10.10.30.2 | 10.10.30.126 |
| VLAN 40 FINANCE | 10.10.40.0/26 | 10.10.40.1 | 10.10.40.2 | 10.10.40.62 |
| VLAN 50 GUEST | 10.10.50.0/24 | 10.10.50.1 | 10.10.50.2 | 10.10.50.254 |
| VLAN 60 NET-MGMT | 10.10.60.0/27 | 10.10.60.1 | 10.10.60.2 | 10.10.60.30 |
| VLAN 70 PC-MGMT | 10.10.70.0/28 | 10.10.70.1 | 10.10.70.2 | 10.10.70.14 |
| VLAN 80 SERVER | 10.10.80.0/28 | 10.10.80.1 | 10.10.80.9 | 10.10.80.14 |

### HSRP Priority

- KNOHA-HQ-CORE1: Priority 110 — Active
- KNOHA-HQ-CORE2: Priority 100 — Standby
- HSRP Preemption: Enabled

CORE1 menggunakan priority 110 sehingga menjadi Active Router dalam kondisi normal. CORE2 menggunakan priority 100 dan berfungsi sebagai Standby Router.

## CORE1 Configuration

enable
configure terminal

interface vlan 10
 ip address 10.10.10.1 255.255.255.192
 ip helper-address 10.10.80.2
 standby 10 ip 10.10.10.62
 standby 10 priority 110
 standby 10 preempt

interface vlan 20
 ip address 10.10.20.1 255.255.255.192
 standby 20 ip 10.10.20.62
 standby 20 priority 110
 standby 20 preempt

interface vlan 30
 ip address 10.10.30.1 255.255.255.128
 standby 30 ip 10.10.30.126
 standby 30 priority 110
 standby 30 preempt

interface vlan 40
 ip address 10.10.40.1 255.255.255.192
 standby 40 ip 10.10.40.62
 standby 40 priority 110
 standby 40 preempt

interface vlan 50
 ip address 10.10.50.1 255.255.255.0
 ip helper-address 10.10.80.2
 standby 50 ip 10.10.50.254
 standby 50 priority 110
 standby 50 preempt

interface vlan 60
 ip address 10.10.60.1 255.255.255.224
 standby 60 ip 10.10.60.30
 standby 60 priority 110
 standby 60 preempt

interface vlan 70
 ip address 10.10.70.1 255.255.255.240
 standby 70 ip 10.10.70.14
 standby 70 priority 110
 standby 70 preempt

interface vlan 80
 ip address 10.10.80.1 255.255.255.240
 standby 80 ip 10.10.80.14
 standby 80 priority 110
 standby 80 preempt

end
write memory

## CORE2 Configuration
enable
configure terminal

interface vlan 10
 ip address 10.10.10.2 255.255.255.192
 ip helper-address 10.10.80.2
 standby 10 ip 10.10.10.62
 standby 10 preempt

interface vlan 20
 ip address 10.10.20.2 255.255.255.192
 standby 20 ip 10.10.20.62
 standby 20 preempt

interface vlan 30
 ip address 10.10.30.2 255.255.255.128
 standby 30 ip 10.10.30.126
 standby 30 preempt

interface vlan 40
 ip address 10.10.40.2 255.255.255.192
 standby 40 ip 10.10.40.62
 standby 40 preempt

interface vlan 50
 ip address 10.10.50.2 255.255.255.0
 ip helper-address 10.10.80.2
 standby 50 ip 10.10.50.254
 standby 50 preempt

interface vlan 60
 ip address 10.10.60.2 255.255.255.224
 standby 60 ip 10.10.60.30
 standby 60 preempt

interface vlan 70
 ip address 10.10.70.2 255.255.255.240
 standby 70 ip 10.10.70.14
 standby 70 preempt

interface vlan 80
 ip address 10.10.80.9 255.255.255.240
 standby 80 ip 10.10.80.14
 standby 80 preempt

end
write memory

## Server Gateway Update

Default gateway pada server HQ menggunakan HSRP Virtual IP VLAN 80 agar server tidak bergantung langsung pada physical IP salah satu Core Switch.

DHCP Server menggunakan konfigurasi:

IP Address      : 10.10.80.2
Subnet Mask     : 255.255.255.240
Default Gateway : 10.10.80.14
DNS Server      : 10.10.80.3

Dengan menggunakan Virtual IP 10.10.80.14 sebagai gateway, server tetap menggunakan gateway yang sama ketika terjadi perpindahan Active Router.

## HSRP Verification

Status HSRP diperiksa menggunakan command:

show standby brief

Pada kondisi normal, hasil yang diharapkan adalah:

CORE1 → Active
CORE2 → Standby

HSRP Virtual IP digunakan sebagai default gateway untuk masing-masing VLAN.

## Failover Testing

Pengujian failover dilakukan dengan mensimulasikan kegagalan koneksi CORE1 menuju distribution layer.

Saat CORE1 mengalami failure, CORE2 mengambil alih fungsi Active Router dan menggunakan HSRP Virtual IP sebagai gateway.

Hasil pengujian menunjukkan:

CORE1 menjadi Active Router dalam kondisi normal.
CORE2 menjadi Standby Router dalam kondisi normal.
CORE2 berhasil mengambil alih fungsi gateway ketika CORE1 mengalami failure.
HSRP Virtual IP tetap dapat digunakan sebagai gateway.
Guest Network tetap dapat memperoleh alamat IP melalui DHCP ketika CORE2 menjadi Active.
DHCP Server tetap dapat dijangkau melalui CORE2.
Setelah CORE1 kembali normal, fitur preemption membuat CORE1 kembali menjadi Active Router.
CORE2 kembali menjadi Standby Router.

## IP SLA / Tracking Limitation

IP SLA dan object tracking tidak diterapkan pada implementasi ini karena IOS/device Cisco Packet Tracer yang digunakan tidak menyediakan command:

ip sla
track

Oleh karena itu, gateway redundancy pada project ini menggunakan HSRP dengan priority dan preemption sebagai mekanisme failover.

## Evidence

Evidence Day 15 disimpan pada:

screenshots/day 15/

Evidence yang digunakan:

hsrp-status-core1.png
hsrp-status-core2.png

hsrp-status-core1.png menunjukkan status HSRP pada CORE1 dengan CORE1 sebagai Active Router.

hsrp-status-core2.png menunjukkan status HSRP pada CORE2 dengan CORE2 sebagai Standby Router.

Command yang digunakan untuk menghasilkan evidence:

show standby brief

## Result

Day 15 berhasil mengimplementasikan HSRP Gateway Redundancy pada jaringan HQ PT Konoha.

HSRP berhasil menyediakan gateway redundancy antara KNOHA-HQ-CORE1 dan KNOHA-HQ-CORE2. Failover telah diuji dengan mensimulasikan kegagalan CORE1, dan CORE2 berhasil mengambil alih fungsi gateway. Setelah CORE1 kembali normal, HSRP preemption berhasil mengembalikan CORE1 sebagai Active Router.

Pengujian juga membuktikan bahwa DHCP Guest tetap berjalan ketika CORE2 mengambil alih gateway.