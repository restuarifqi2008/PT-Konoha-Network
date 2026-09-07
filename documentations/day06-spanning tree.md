# Day 6 — Spanning Tree Protocol

## Tujuan

Mengatur Spanning Tree Protocol (STP) pada jaringan PT Konoha untuk mencegah loop Layer 2 dan menentukan jalur utama serta jalur backup pada topologi redundant.

## STP Design

- Protocol: IEEE 802.1D STP
- Root Primary: HQ-CORE1
- Root Secondary: HQ-CORE2
- VLAN: 10, 20, 30, 40, 50, 60, 70, 80, 999

## Konfigurasi

### HQ-CORE1

```cisco
spanning-tree vlan 10,20,30,40,50,60,70,80,999 root primary