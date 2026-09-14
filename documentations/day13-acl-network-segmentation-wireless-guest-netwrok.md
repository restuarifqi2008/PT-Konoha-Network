# Day 13 — ACL, Network Segmentation, Wireless & Guest Network

## Objective

Melakukan implementasi keamanan jaringan menggunakan Access Control List (ACL), network segmentation, serta wireless guest network pada jaringan PT Konoha.

Implementasi Day 13 bertujuan untuk:

* Membatasi akses Guest Network terhadap jaringan internal.
* Membatasi akses User VLAN terhadap Network Management VLAN.
* Memisahkan traffic Guest dari traffic internal perusahaan.
* Menyediakan konektivitas wireless untuk perangkat Guest.
* Memastikan perangkat Guest mendapatkan IP melalui DHCP.
* Memastikan akses management network hanya dapat dilakukan oleh perangkat yang diizinkan.

---

## Network Security & Wireless Design

Implementasi Day 13 terdiri dari tiga bagian utama:

1. **ACL & Guest Isolation**
2. **Management Network Segmentation**
3. **Wireless Guest Network**

Guest Network menggunakan VLAN 50 dan diterapkan pada HQ, Branch 1, dan Branch 2.

Perangkat laptop Guest terhubung melalui Access Point pada masing-masing site dan memperoleh alamat IP secara otomatis melalui DHCP.

---

## VLAN Used

| VLAN | Name             | Purpose                |
| ---- | ---------------- | ---------------------- |
| 10   | IT               | IT Department          |
| 20   | HR               | HR Department          |
| 30   | OPERASIONAL      | Operational Department |
| 40   | FINANCE          | Finance Department     |
| 50   | GUEST            | Guest Wireless Network |
| 60   | NET-MGMT         | Network Management     |
| 70   | PC-MGMT          | Management PC          |
| 80   | SERVER           | Server Network         |
| 999  | NATIVE-BLACKHOLE | Native / Unused VLAN   |

---

# 1. Guest Network

Guest Network menggunakan **VLAN 50** pada masing-masing site.

### Addressing

| Site     | Guest Network   | Gateway      |
| -------- | --------------- | ------------ |
| HQ       | `10.10.50.0/24` | `10.10.50.1` |
| Branch 1 | `10.20.50.0/26` | `10.20.50.1` |
| Branch 2 | `10.30.50.0/26` | `10.30.50.1` |

Guest client memperoleh IP address secara otomatis melalui DHCP.

---

# 2. Wireless Guest Network

Access Point digunakan pada area living room di:

* HQ
* Branch 1
* Branch 2

Area tersebut menggunakan perangkat laptop sebagai endpoint wireless Guest.

Alur koneksi Guest:

```text
Guest Laptop
     |
     | Wireless
     v
 Access Point
     |
     | VLAN 50
     v
  Access Switch
     |
     v
   MLS/Core
     |
     v
 DHCP Server
```

Guest wireless menggunakan VLAN 50 sehingga traffic wireless Guest tetap berada pada segment Guest Network.

### Wireless Validation

Pengujian dilakukan untuk memastikan:

* Laptop berhasil terhubung ke Access Point.
* Laptop memperoleh IP address melalui DHCP.
* Default gateway sesuai dengan VLAN 50.
* DNS dapat digunakan sesuai policy jaringan.
* Guest dapat mengakses koneksi yang diizinkan.
* Guest tidak dapat mengakses jaringan internal.

---

# 3. Guest Isolation

Guest traffic diisolasi dari private/internal network menggunakan extended ACL.

ACL diterapkan secara inbound pada SVI VLAN 50. Cisco mendukung penerapan named extended ACL menggunakan `ip access-group <acl> in` pada interface.

## HQ — KNOHA-HQ-CORE1

```cisco
ip access-list extended ACL-GUEST-HQ
 permit ip 10.10.50.0 0.0.0.255 host 10.10.50.1
 deny ip 10.10.50.0 0.0.0.255 10.0.0.0 0.255.255.255
 permit ip 10.10.50.0 0.0.0.255 any
exit

interface vlan 50
 ip access-group ACL-GUEST-HQ in
exit
```

## Branch 1 — KNOHA-BR1-MLS1

```cisco
ip access-list extended ACL-GUEST-BR1
 permit ip 10.20.50.0 0.0.0.63 host 10.20.50.1
 deny ip 10.20.50.0 0.0.0.63 10.0.0.0 0.255.255.255
 permit ip 10.20.50.0 0.0.0.63 any
exit

interface vlan 50
 ip access-group ACL-GUEST-BR1 in
exit
```

## Branch 2 — KNOHA-BR2-MLS1

```cisco
ip access-list extended ACL-GUEST-BR2
 permit ip 10.30.50.0 0.0.0.63 host 10.30.50.1
 deny ip 10.30.50.0 0.0.0.63 10.0.0.0 0.255.255.255
 permit ip 10.30.50.0 0.0.0.63 any
exit

interface vlan 50
 ip access-group ACL-GUEST-BR2 in
exit
```

### Guest Isolation Logic

ACL Guest menggunakan tiga rule utama:

1. Mengizinkan Guest mengakses gateway VLAN 50.
2. Memblokir akses Guest menuju private network `10.0.0.0/8`.
3. Mengizinkan traffic lainnya.

Rule permit menuju gateway ditempatkan sebelum rule deny agar Guest tetap dapat berkomunikasi dengan default gateway.

---

# 4. Management Network Segmentation

Network Management menggunakan VLAN 60.

User VLAN tidak diperbolehkan mengakses Network Management VLAN untuk mencegah user biasa melakukan akses langsung terhadap perangkat jaringan.

User VLAN yang dibatasi:

* VLAN 10 — IT
* VLAN 20 — HR
* VLAN 30 — OPERASIONAL
* VLAN 40 — FINANCE

Sedangkan PC Management pada VLAN 70 diperbolehkan mengakses Network Management VLAN.

### Management Network

| Site     | Network         | Gateway      |
| -------- | --------------- | ------------ |
| HQ       | `10.10.60.0/27` | `10.10.60.1` |
| Branch 1 | `10.20.60.0/28` | `10.20.60.1` |
| Branch 2 | `10.30.60.0/28` | `10.30.60.1` |

---

# 5. Management ACL — HQ

Device: `KNOHA-HQ-CORE1`

```cisco
ip access-list extended ACL-HQ-USER-TO-MGMT
 remark BLOCK HQ USER VLAN TO ALL MANAGEMENT VLAN

 deny ip 10.10.10.0 0.0.0.63 10.10.60.0 0.0.0.31
 deny ip 10.10.10.0 0.0.0.63 10.20.60.0 0.0.0.15
 deny ip 10.10.10.0 0.0.0.63 10.30.60.0 0.0.0.15

 deny ip 10.10.20.0 0.0.0.63 10.10.60.0 0.0.0.31
 deny ip 10.10.20.0 0.0.0.63 10.20.60.0 0.0.0.15
 deny ip 10.10.20.0 0.0.0.63 10.30.60.0 0.0.0.15

 deny ip 10.10.30.0 0.0.0.127 10.10.60.0 0.0.0.31
 deny ip 10.10.30.0 0.0.0.127 10.20.60.0 0.0.0.15
 deny ip 10.10.30.0 0.0.0.127 10.30.60.0 0.0.0.15

 deny ip 10.10.40.0 0.0.0.63 10.10.60.0 0.0.0.31
 deny ip 10.10.40.0 0.0.0.63 10.20.60.0 0.0.0.15
 deny ip 10.10.40.0 0.0.0.63 10.30.60.0 0.0.0.15

 permit ip any any
exit

interface vlan 10
 ip access-group ACL-HQ-USER-TO-MGMT in
exit

interface vlan 20
 ip access-group ACL-HQ-USER-TO-MGMT in
exit

interface vlan 30
 ip access-group ACL-HQ-USER-TO-MGMT in
exit

interface vlan 40
 ip access-group ACL-HQ-USER-TO-MGMT in
exit
```

---

# 6. Management ACL — Branch 1

Device: `KNOHA-BR1-MLS1`

```cisco
ip access-list extended ACL-BR1-USER-TO-MGMT
 remark BLOCK BR1 USER VLAN TO ALL MANAGEMENT VLAN

 deny ip 10.20.10.0 0.0.0.15 10.10.60.0 0.0.0.31
 deny ip 10.20.10.0 0.0.0.15 10.20.60.0 0.0.0.15
 deny ip 10.20.10.0 0.0.0.15 10.30.60.0 0.0.0.15

 deny ip 10.20.20.0 0.0.0.15 10.10.60.0 0.0.0.31
 deny ip 10.20.20.0 0.0.0.15 10.20.60.0 0.0.0.15
 deny ip 10.20.20.0 0.0.0.15 10.30.60.0 0.0.0.15

 deny ip 10.20.30.0 0.0.0.31 10.10.60.0 0.0.0.31
 deny ip 10.20.30.0 0.0.0.31 10.20.60.0 0.0.0.15
 deny ip 10.20.30.0 0.0.0.31 10.30.60.0 0.0.0.15

 deny ip 10.20.40.0 0.0.0.31 10.10.60.0 0.0.0.31
 deny ip 10.20.40.0 0.0.0.31 10.20.60.0 0.0.0.15
 deny ip 10.20.40.0 0.0.0.31 10.30.60.0 0.0.0.15

 permit ip any any
exit

interface vlan 10
 ip access-group ACL-BR1-USER-TO-MGMT in
exit

interface vlan 20
 ip access-group ACL-BR1-USER-TO-MGMT in
exit

interface vlan 30
 ip access-group ACL-BR1-USER-TO-MGMT in
exit

interface vlan 40
 ip access-group ACL-BR1-USER-TO-MGMT in
exit
```

---

# 7. Management ACL — Branch 2

Device: `KNOHA-BR2-MLS1`

```cisco
ip access-list extended ACL-BR2-USER-TO-MGMT
 remark BLOCK BR2 USER VLAN TO ALL MANAGEMENT VLAN

 deny ip 10.30.10.0 0.0.0.15 10.10.60.0 0.0.0.31
 deny ip 10.30.10.0 0.0.0.15 10.20.60.0 0.0.0.15
 deny ip 10.30.10.0 0.0.0.15 10.30.60.0 0.0.0.15

 deny ip 10.30.20.0 0.0.0.15 10.10.60.0 0.0.0.31
 deny ip 10.30.20.0 0.0.0.15 10.20.60.0 0.0.0.15
 deny ip 10.30.20.0 0.0.0.15 10.30.60.0 0.0.0.15

 deny ip 10.30.30.0 0.0.0.31 10.10.60.0 0.0.0.31
 deny ip 10.30.30.0 0.0.0.31 10.20.60.0 0.0.0.15
 deny ip 10.30.30.0 0.0.0.31 10.30.60.0 0.0.0.15

 deny ip 10.30.40.0 0.0.0.31 10.10.60.0 0.0.0.31
 deny ip 10.30.40.0 0.0.0.31 10.20.60.0 0.0.0.15
 deny ip 10.30.40.0 0.0.0.31 10.30.60.0 0.0.0.15

 permit ip any any
exit

interface vlan 10
 ip access-group ACL-BR2-USER-TO-MGMT in
exit

interface vlan 20
 ip access-group ACL-BR2-USER-TO-MGMT in
exit

interface vlan 30
 ip access-group ACL-BR2-USER-TO-MGMT in
exit

interface vlan 40
 ip access-group ACL-BR2-USER-TO-MGMT in
exit
```

---

# 8. Security Policy

Setelah implementasi Day 13, policy jaringan menjadi:

| Source       | Destination      | Result                     |
| ------------ | ---------------- | -------------------------- |
| Guest VLAN   | Guest Gateway    | ✅ Allowed                  |
| Guest VLAN   | Internal Network | ❌ Blocked                  |
| Guest VLAN   | Server VLAN      | ❌ Blocked                  |
| Guest VLAN   | Management VLAN  | ❌ Blocked                  |
| User VLAN    | Management VLAN  | ❌ Blocked                  |
| PC-MGMT VLAN | Management VLAN  | ✅ Allowed                  |
| PC-MGMT VLAN | Network Devices  | ✅ Allowed                  |
| User VLAN    | Server Network   | ✅ Allowed sesuai kebutuhan |
| User VLAN    | User VLAN lain   | ✅ Allowed sesuai kebutuhan |

---

# 9. Testing & Verification

## Guest Wireless Testing

Pengujian dilakukan dari laptop yang terhubung melalui Access Point.

### DHCP

Guest laptop berhasil memperoleh IP address dari DHCP Server sesuai subnet VLAN 50.

### Gateway

```text
ping <Guest Gateway>
```

Result:

```text
Success
```

### Internal Network

```text
ping 10.10.80.4
```

Result:

```text
Blocked
```

Guest tidak dapat mengakses Web Server pada VLAN 80 karena server berada pada jaringan internal.

---

## Management Segmentation Testing

### User VLAN → Management

```text
ping 10.10.60.1
```

Result:

```text
Blocked
```

User VLAN tidak dapat mengakses Network Management VLAN.

### PC Management → Management

```text
ping 10.10.60.1
ping 10.10.60.3
```

Result:

```text
Success
```

PC Management tetap dapat mengakses perangkat pada Management Network.

---

# 10. ACL Verification

Untuk melihat ACL dan hit counter:

```cisco
show access-lists
```

Hit counter digunakan untuk memastikan traffic dari hasil pengujian telah diproses oleh rule ACL yang sesuai.

Cisco juga menyediakan `show ip access-list` untuk melihat isi IP ACL yang sedang aktif.

---

# 11. Configuration Backup

Setelah konfigurasi dan pengujian selesai:

```cisco
write memory
```

Konfigurasi disimpan pada perangkat untuk memastikan konfigurasi tetap tersedia setelah perangkat direstart.

---

# 12. Evidence

Screenshot yang digunakan sebagai bukti implementasi:

### ACL Configuration

* `acl-hq.png`
* `acl-br1.png`
* `acl-br2.png`

### Guest & Wireless Testing

* `guest-isolation-hq.png`
* `guest-isolation-br1.png`
* `guest-isolation-br2.png`
* `guest-dhcp-wireless.png`
* `wireless-guest-connectivity.png`

### Management Segmentation

* `management-segmentation.png`

Screenshot tersebut menunjukkan konfigurasi ACL, koneksi wireless Guest, DHCP Guest, Guest isolation, serta pembatasan akses menuju Management Network.

---

# 13. Result

Hasil implementasi Day 13:

| Feature                          | Status      |
| -------------------------------- | ----------- |
| Guest VLAN                       | ✅ Completed |
| Guest Wireless                   | ✅ Completed |
| Guest DHCP                       | ✅ Completed |
| Guest Isolation HQ               | ✅ Passed    |
| Guest Isolation Branch 1         | ✅ Passed    |
| Guest Isolation Branch 2         | ✅ Passed    |
| Management Segmentation HQ       | ✅ Passed    |
| Management Segmentation Branch 1 | ✅ Passed    |
| Management Segmentation Branch 2 | ✅ Passed    |
| PC Management Access             | ✅ Passed    |
| ACL Verification                 | ✅ Passed    |

---

# Conclusion

Day 13 berhasil mengimplementasikan **ACL, Network Segmentation, Wireless Guest Network, dan Guest Isolation** pada jaringan PT Konoha.

Guest Network pada HQ, Branch 1, dan Branch 2 berhasil dipisahkan dari jaringan internal menggunakan VLAN 50 dan ACL. Perangkat laptop Guest dapat terhubung melalui Access Point dan memperoleh alamat IP secara otomatis melalui DHCP.

Selain itu, akses dari User VLAN menuju Network Management VLAN berhasil dibatasi. Hanya perangkat yang berada pada PC Management VLAN yang diperbolehkan mengakses Network Management Network untuk kebutuhan administrasi dan remote management.

Dengan implementasi ini, segmentasi jaringan PT Konoha menjadi lebih aman dan terkontrol, sementara konektivitas wireless Guest tetap dapat disediakan tanpa membuka akses ke jaringan internal perusahaan.
