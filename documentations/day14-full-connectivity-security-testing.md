# Day 14 — Full Connectivity & Security Testing

## Objective

Melakukan pengujian menyeluruh terhadap konektivitas dan keamanan jaringan PT Konoha setelah seluruh konfigurasi Day 1–13 selesai.

## Testing

### 1. Inter-VLAN Connectivity

Pengujian dilakukan untuk memastikan komunikasi antar VLAN melalui Multilayer Switch berjalan normal.

Hasil:
- IT → HR: berhasil
- IT → Operasional: berhasil
- IT → Finance: berhasil
- User → Server Gateway: berhasil

**Status: PASS ✅**

### 2. Inter-Site Connectivity

Pengujian komunikasi antar lokasi:

- HQ → BR1: berhasil
- HQ → BR2: berhasil
- BR1 → BR2: berhasil

**Status: PASS ✅**

### 3. Server Connectivity

Seluruh server pada VLAN 80 berhasil diakses dari jaringan internal:

| Server | IP Address | Status |
|---|---|---|
| DHCP | 10.10.80.2 | PASS |
| DNS | 10.10.80.3 | PASS |
| Web | 10.10.80.4 | PASS |
| File | 10.10.80.5 | PASS |
| Mail | 10.10.80.6 | PASS |
| Monitoring | 10.10.80.7 | PASS |

**Status: PASS ✅**

### 4. DNS & HTTPS

Pengujian akses Web Server menggunakan DNS:

- `konoha.local`
- `www.konoha.local`
- `portal.konoha.local`

Pengujian dilakukan dari HQ, BR1, dan BR2.

Seluruh domain berhasil di-resolve dan Web Server dapat diakses melalui HTTPS.

**Status: PASS ✅**

### 5. Guest Network Security

Pengujian dilakukan dari Guest Network VLAN 50.

Hasil:
- Guest → Gateway: berhasil
- Guest → Internal Network: diblokir
- Guest → Server: diblokir
- Guest → Management Network: diblokir

ACL Guest berhasil menerapkan isolasi terhadap jaringan internal.

**Status: PASS ✅**

### 6. Management Network Security

Pengujian dilakukan untuk memastikan akses menuju VLAN 60 hanya tersedia untuk perangkat yang memiliki hak management.

Hasil:
- User VLAN → Management Network: diblokir
- PC Management VLAN 70 → Management Network: berhasil
- PC Management → SSH perangkat jaringan: berhasil

**Status: PASS ✅**

### 7. OSPF Verification

Verifikasi dilakukan menggunakan:

show ip ospf neighbor
show ip route

Seluruh adjacency OSPF pada HQ-RTR berada pada status FULL.

Router-ID:

HQ-RTR: 1.1.1.1
HQ-CORE1: 11.11.11.11
HQ-CORE2: 12.12.12.12
BR1-RTR: 2.2.2.2
BR2-RTR: 3.3.3.3

Routing antar-site berjalan normal.

Status: PASS ✅

### 8. ACL Verification

Verifikasi dilakukan menggunakan:

show access-lists

ACL Guest dan Management menunjukkan match counter pada rule deny, yang membuktikan traffic yang seharusnya diblokir berhasil ditolak.

Status: PASS ✅

### 9. Trunk & EtherChannel Verification

Verifikasi dilakukan menggunakan:

show interfaces trunk
show etherchannel summary

Hasil:

Port-channel aktif dan bundled.
Member EtherChannel berstatus P.
Native VLAN menggunakan VLAN 999.
VLAN yang diperlukan tersedia pada trunk.
Po5 pada HQ-ACC1 berstatus SU.

Status: PASS ✅

## Testing Summary
Test	Result
Inter-VLAN Connectivity	PASS ✅
Inter-Site Connectivity	PASS ✅
Server Connectivity	PASS ✅
DNS & HTTPS	PASS ✅
Guest Network Security	PASS ✅
Management Security	PASS ✅
OSPF	PASS ✅
ACL	PASS ✅
Trunk & EtherChannel	PASS ✅

## Result

Seluruh pengujian konektivitas, routing, server, DNS, HTTPS, ACL, network segmentation, trunk, dan EtherChannel berhasil dilakukan.

Tidak ditemukan masalah konektivitas setelah troubleshooting OSPF Router-ID dan pemasangan ACL pada SVI yang sebelumnya belum terpasang.

## Kesimpulan

Day 14 berhasil memvalidasi bahwa jaringan PT Konoha berjalan sesuai rancangan dan security policy yang telah diterapkan.

Network siap dilanjutkan ke tahap High Availability dengan HSRP dan IP SLA/Tracking pada Day 15.