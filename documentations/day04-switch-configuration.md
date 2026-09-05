# Switch Configuration

## 1. Tujuan

Melakukan konfigurasi dasar switch pada jaringan PT Konoha serta menerapkan VLAN, access port, dan trunk sesuai dengan desain jaringan yang telah dibuat.

## 2. Konfigurasi yang Dilakukan

Konfigurasi Day 4 meliputi:

-[x] Basic switch configuration
-[x] Pembuatan VLAN
-[x] Penamaan VLAN
-[x] Konfigurasi access port
-[x] Konfigurasi trunk
-[x] Penerapan Native VLAN 999
-[x] Penentuan VLAN yang diizinkan pada trunk
-[x] Verifikasi konfigurasi

## 3. VLAN yang Digunakan

| VLAN ID | Nama VLAN | Fungsi |
|---:|---|---|
| 10 | IT | Departemen IT |
| 20 | HR | Departemen HR |
| 30 | OPERASIONAL | Departemen Operasional |
| 40 | FINANCE | Departemen Finance |
| 50 | GUEST | Jaringan Guest |
| 60 | NET-MGMT | Management perangkat jaringan |
| 70 | PC-MGMT | PC Management administrator |
| 80 | SERVER | Server internal |
| 999 | NATIVE-BLACKHOLE | Native VLAN dan unused port |

## 4. Access Port

Access port dikonfigurasi sesuai dengan kebutuhan masing-masing cluster.

Contoh pembagian:

- IT → VLAN 10
- HR → VLAN 20
- OPERASIONAL → VLAN 30
- FINANCE → VLAN 40
- Guest / Access Point → VLAN 50
- PC Management → VLAN 70
- Server → VLAN 80

Setiap access port hanya membawa satu VLAN sesuai dengan fungsi perangkat yang terhubung.

## 5. Trunk Port

Port yang digunakan sebagai koneksi antar-switch dikonfigurasi sebagai trunk menggunakan IEEE 802.1Q.

VLAN yang diizinkan pada trunk:

```text
10,20,30,40,50,60,70,80,999
```

Native VLAN yang digunakan:

```text
VLAN 999 — NATIVE-BLACKHOLE
```

Contoh konfigurasi:

```cisco
interface <trunk-interface>
 switchport mode trunk
 switchport trunk native vlan 999
 switchport trunk allowed vlan 10,20,30,40,50,60,70,80,999
```

## 6. Verifikasi

Verifikasi konfigurasi VLAN dilakukan menggunakan:

```cisco
show vlan brief
```

Verifikasi konfigurasi trunk dilakukan menggunakan:

```cisco
show interfaces trunk
```

Hasil verifikasi pada HQ-CORE1 menunjukkan:

- VLAN 10–80 berhasil dibuat.
- VLAN 999 berhasil dibuat.
- Port Fa0/1–Fa0/4 berada dalam mode trunk.
- Native VLAN telah menggunakan VLAN 999.
- VLAN 10,20,30,40,50,60,70,80,999 diizinkan pada trunk.

## 7. Catatan

Konfigurasi routing belum dilakukan pada tahap ini.

Konfigurasi berikut akan dilakukan pada tahap selanjutnya:

- EtherChannel
- STP
- Inter-VLAN Routing
- WAN Routing
- OSPF
- DHCP
- Security

## 8. Status

**Day 4 — SELESAI ✅**

Switch telah dikonfigurasi dengan VLAN, access port, dan trunk sesuai dengan desain jaringan PT Konoha.