# Day 5 — EtherChannel LACP

## Tujuan

Mengimplementasikan EtherChannel menggunakan LACP pada koneksi antar-switch untuk meningkatkan bandwidth dan menyediakan redundancy pada jaringan PT Konoha.

## Protocol

- EtherChannel Protocol: LACP
- Mode: `active`
- Standard: IEEE 802.3ad

## EtherChannel yang Berhasil

| Port-Channel | Koneksi | Protocol | Status |
|---|---|---|---|
| Po1 | HQ-CORE1 ↔ HQ-DIST1 | LACP | Berhasil |
| Po2 | HQ-CORE1 ↔ HQ-DIST2 | LACP | Berhasil |
| Po3 | HQ-CORE2 ↔ HQ-DIST1 | LACP | Berhasil |
| Po4 | HQ-CORE2 ↔ HQ-DIST2 | LACP | Berhasil |
| Po5 | HQ-DIST1 ↔ HQ-ACC1 | LACP | Berhasil |
| Po6 | HQ-DIST1 ↔ HQ-ACC2 | LACP | Berhasil |

## Verifikasi

EtherChannel diverifikasi menggunakan:

```text
show etherchannel summary