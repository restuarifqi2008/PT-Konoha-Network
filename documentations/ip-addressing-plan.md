# IP Addressing Plan — PT Konoha

## 1. Pembagian IP Address

IP addressing PT Konoha menggunakan pembagian subnet berdasarkan lokasi dan kebutuhan masing-masing VLAN.

Struktur network utama:

- **HQ** → `10.10.0.0/16`
- **Branch 1** → `10.20.0.0/16`
- **Branch 2** → `10.30.0.0/16`
- **WAN** → `10.255.0.0/24`

---

## 2. IP Addressing HQ

| VLAN ID | Nama VLAN | Jumlah User | Network | Subnet Mask | Gateway |
|---:|---|---:|---|---|---|
| 10 | IT | 15 | `10.10.10.0/26` | `255.255.255.192` | `10.10.10.1` |
| 20 | HR | 15 | `10.10.20.0/26` | `255.255.255.192` | `10.10.20.1` |
| 30 | OPERASIONAL | 35 | `10.10.30.0/25` | `255.255.255.128` | `10.10.30.1` |
| 40 | FINANCE | 20 | `10.10.40.0/26` | `255.255.255.192` | `10.10.40.1` |
| 50 | GUEST | - | `10.10.50.0/24` | `255.255.255.0` | `10.10.50.1` |
| 60 | NET-MGMT | - | `10.10.60.0/27` | `255.255.255.224` | `10.10.60.1` |
| 70 | PC-MGMT | 1 | `10.10.70.0/28` | `255.255.255.240` | `10.10.70.1` |
| 80 | SERVER | 6 | `10.10.80.0/28` | `255.255.255.240` | `10.10.80.1` |
| 999 | NATIVE-BLACKHOLE | - | Tidak menggunakan IP | - | - |

### Keterangan

- Jumlah user pada VLAN 10, 20, 30, dan 40 mengikuti estimasi pengguna HQ.
- VLAN 50 digunakan untuk jaringan Guest.
- VLAN 60 digunakan untuk management perangkat jaringan.
- VLAN 70 digunakan untuk PC Management administrator.
- VLAN 80 digunakan untuk Server Cluster.
- VLAN 999 tidak diberikan alamat IP.

---

## 3. IP Addressing Branch 1

| VLAN ID | Nama VLAN | Jumlah User | Network | Subnet Mask | Gateway |
|---:|---|---:|---|---|---|
| 10 | IT | 5 | `10.20.10.0/28` | `255.255.255.240` | `10.20.10.1` |
| 20 | HR | 5 | `10.20.20.0/28` | `255.255.255.240` | `10.20.20.1` |
| 30 | OPERASIONAL | 15 | `10.20.30.0/27` | `255.255.255.224` | `10.20.30.1` |
| 40 | FINANCE | 10 | `10.20.40.0/27` | `255.255.255.224` | `10.20.40.1` |
| 50 | GUEST | - | `10.20.50.0/26` | `255.255.255.192` | `10.20.50.1` |
| 60 | NET-MGMT | - | `10.20.60.0/28` | `255.255.255.240` | `10.20.60.1` |
| 999 | NATIVE-BLACKHOLE | - | Tidak menggunakan IP | - | - |

### Keterangan

Branch 1 tidak menggunakan:

- VLAN 70 — PC-MGMT
- VLAN 80 — SERVER

Management dilakukan secara terpusat dari HQ dan seluruh server berada di HQ.

---

## 4. IP Addressing Branch 2

| VLAN ID | Nama VLAN | Jumlah User | Network | Subnet Mask | Gateway |
|---:|---|---:|---|---|---|
| 10 | IT | 5 | `10.30.10.0/28` | `255.255.255.240` | `10.30.10.1` |
| 20 | HR | 5 | `10.30.20.0/28` | `255.255.255.240` | `10.30.20.1` |
| 30 | OPERASIONAL | 15 | `10.30.30.0/27` | `255.255.255.224` | `10.30.30.1` |
| 40 | FINANCE | 10 | `10.30.40.0/27` | `255.255.255.224` | `10.30.40.1` |
| 50 | GUEST | - | `10.30.50.0/26` | `255.255.255.192` | `10.30.50.1` |
| 60 | NET-MGMT | - | `10.30.60.0/28` | `255.255.255.240` | `10.30.60.1` |
| 999 | NATIVE-BLACKHOLE | - | Tidak menggunakan IP | - | - |

### Keterangan

Branch 2 tidak menggunakan:

- VLAN 70 — PC-MGMT
- VLAN 80 — SERVER

Management dilakukan secara terpusat dari HQ dan seluruh server berada di HQ.

---

## 5. IP Addressing Server

Seluruh server ditempatkan pada VLAN 80 di HQ dan menggunakan IP statis.

| Server | IP Address | Gateway |
|---|---|---|
| DHCP Server | `10.10.80.2/28` | `10.10.80.1` |
| DNS Server | `10.10.80.3/28` | `10.10.80.1` |
| Web Server | `10.10.80.4/28` | `10.10.80.1` |
| File Server | `10.10.80.5/28` | `10.10.80.1` |
| Mail Server | `10.10.80.6/28` | `10.10.80.1` |
| Monitoring Server | `10.10.80.7/28` | `10.10.80.1` |

---

## 6. IP Addressing Management

### HQ

| Device | Management IP |
|---|---|
| HQ-CORE1 | `10.10.60.2/27` |
| HQ-CORE2 | `10.10.60.3/27` |
| HQ-DIST1 | `10.10.60.4/27` |
| HQ-DIST2 | `10.10.60.5/27` |
| HQ-ACC1 | `10.10.60.6/27` |
| HQ-ACC2 | `10.10.60.7/27` |
| HQ-SACC1 | `10.10.60.8/27` |
| HQ-SACC2 | `10.10.60.9/27` |
| HQ-RTR | `10.10.60.10/27` |

### Branch 1

| Device | Management IP |
|---|---|
| BR1-MLS1 | `10.20.60.2/28` |
| BR1-ACC1 | `10.20.60.3/28` |
| BR1-RTR | `10.20.60.4/28` |

### Branch 2

| Device | Management IP |
|---|---|
| BR2-MLS1 | `10.30.60.2/28` |
| BR2-ACC1 | `10.30.60.3/28` |
| BR2-RTR | `10.30.60.4/28` |

### PC Management

| Device | IP Address | Gateway |
|---|---|---|
| PC-MGMT-HQ | `10.10.70.2/28` | `10.10.70.1` |

---

## 7. IP Addressing WAN

Koneksi antar Edge Router menggunakan jaringan point-to-point dengan subnet `/30`.

| Link | Network | IP Router 1 | IP Router 2 |
|---|---|---|---|
| HQ ↔ BR1 | `10.255.0.0/30` | HQ `10.255.0.1` | BR1 `10.255.0.2` |
| HQ ↔ BR2 | `10.255.0.4/30` | HQ `10.255.0.5` | BR2 `10.255.0.6` |
| BR1 ↔ BR2 | `10.255.0.8/30` | BR1 `10.255.0.9` | BR2 `10.255.0.10` |

---

## 8. Ringkasan

| Lokasi | Network Utama |
|---|---|
| HQ | `10.10.0.0/16` |
| Branch 1 | `10.20.0.0/16` |
| Branch 2 | `10.30.0.0/16` |
| WAN | `10.255.0.0/24` |

Pembagian alamat IP menggunakan subnet yang disesuaikan dengan kebutuhan pengguna pada masing-masing lokasi serta menyediakan ruang untuk pengembangan jaringan di masa mendatang.