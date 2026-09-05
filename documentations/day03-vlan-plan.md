# VLAN Plan — PT Konoha

## Tujuan

VLAN digunakan untuk melakukan segmentasi jaringan berdasarkan fungsi dan kebutuhan pengguna pada PT Konoha. Setiap VLAN memiliki broadcast domain tersendiri dan nantinya akan dikontrol menggunakan inter-VLAN routing serta ACL.

## Daftar VLAN

| VLAN ID | Nama VLAN | Fungsi | HQ | BR1 | BR2 |
|---:|---|---|:---:|:---:|:---:|
| 10 | IT | User departemen IT | ✅ | ✅ | ✅ |
| 20 | HR | User departemen HR | ✅ | ✅ | ✅ |
| 30 | OPERASIONAL | User departemen Operasional | ✅ | ✅ | ✅ |
| 40 | FINANCE | User departemen Finance | ✅ | ✅ | ✅ |
| 50 | GUEST | Guest dan perangkat wireless | ✅ | ✅ | ✅ |
| 60 | NET-MGMT | Management perangkat jaringan | ✅ | ✅ | ✅ |
| 70 | PC-MGMT | Workstation administrator jaringan | ✅ | ❌ | ❌ |
| 80 | SERVER | Server internal perusahaan | ✅ | ❌ | ❌ |
| 999 | NATIVE-BLACKHOLE | Native VLAN dan port tidak digunakan | ✅ | ✅ | ✅ |

## Penjelasan VLAN

### VLAN 10 — IT

Digunakan untuk perangkat user pada departemen IT.

### VLAN 20 — HR

Digunakan untuk perangkat user pada departemen HR.

### VLAN 30 — OPERASIONAL

Digunakan untuk perangkat user pada departemen Operasional.

### VLAN 40 — FINANCE

Digunakan untuk perangkat user pada departemen Finance.

### VLAN 50 — GUEST

Digunakan untuk perangkat tamu dan perangkat wireless.

VLAN ini akan diberikan pembatasan akses menggunakan ACL agar tidak dapat mengakses jaringan internal perusahaan secara bebas.

### VLAN 60 — NET-MGMT

Digunakan khusus untuk management perangkat jaringan seperti:

- Core Switch
- Distribution Switch
- Access Switch
- Server Access Switch
- Multilayer Switch Branch
- Router

### VLAN 70 — PC-MGMT

Digunakan untuk workstation administrator jaringan.

Pada project ini digunakan satu PC Management utama di HQ:

`PC-MGMT-HQ`

PC tersebut akan digunakan untuk melakukan remote management perangkat menggunakan SSH.

### VLAN 80 — SERVER

Digunakan untuk seluruh server internal perusahaan yang berada di HQ.

Server yang termasuk dalam VLAN ini:

- DHCP Server
- DNS Server
- Web Server
- File Server
- Mail Server
- Monitoring Server

### VLAN 999 — NATIVE-BLACKHOLE

Digunakan sebagai Native VLAN pada trunk dan untuk port yang tidak digunakan.

VLAN ini tidak diberikan alamat IP dan tidak digunakan untuk komunikasi user.

## Prinsip Implementasi

- VLAN dibuat sesuai fungsi perangkat.
- VLAN user dipisahkan antar departemen.
- VLAN Management dipisahkan dari jaringan user.
- VLAN Server dipisahkan dari jaringan user.
- VLAN Guest akan dibatasi menggunakan ACL.
- VLAN 70 hanya digunakan di HQ.
- VLAN 80 hanya digunakan di HQ.
- VLAN 999 digunakan sebagai Native VLAN pada trunk.