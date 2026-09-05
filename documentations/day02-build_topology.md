# Day 2 — Pembangunan Physical Network Topology

## Tujuan

Membangun physical network topology PT Konoha menggunakan Cisco Packet Tracer berdasarkan network requirements yang telah ditentukan pada Day 1.

Pada tahap ini fokus utama adalah membangun struktur fisik jaringan. Konfigurasi VLAN, IP Addressing, OSPF, DHCP, ACL, SSH, dan fitur keamanan lainnya belum dilakukan.

---

## 1. Topologi HQ

HQ menggunakan arsitektur jaringan berlapis yang terdiri dari Core, Distribution, dan Access Layer.

Perangkat yang digunakan:

- 2 Core Multilayer Switch
- 2 Distribution Multilayer Switch
- 2 User Access Switch
- 2 Server Access Switch
- 1 Edge Router
- 6 Server
- Representative end devices
- 1 Access Point untuk jaringan Guest
- 1 PC Management

### Pembagian User Access Switch

**KNOHA-HQ-ACC1**
- IT
- HR
- Guest Wireless
- PC Management

**KNOHA-HQ-ACC2**
- Operasional
- Finance

User endpoint yang digunakan merupakan representative devices dan tidak merepresentasikan seluruh jumlah pengguna perusahaan.

---

## 2. Server Cluster

Server Cluster ditempatkan di HQ.

Server dibagi menjadi dua Server Access Switch.

### KNOHA-HQ-SACC1

Menangani:

- DHCP Server
- DNS Server
- Web Server

### KNOHA-HQ-SACC2

Menangani:

- File Server
- Mail Server
- Monitoring Server

Masing-masing Server Access Switch menangani tiga server.

Kedua Server Access Switch memiliki uplink redundant menuju Distribution Layer untuk meningkatkan availability dan menyediakan jalur alternatif apabila terjadi kegagalan koneksi.

Tidak digunakan Server Distribution Switch khusus.

---

## 3. PC Management

Satu PC Management ditempatkan di HQ sebagai workstation administrator jaringan.

Perangkat:

- `PC-MGMT-HQ`

PC Management akan digunakan pada tahap konfigurasi untuk melakukan administrasi dan pengujian perangkat jaringan melalui SSH.

PC Management menggunakan:

**VLAN 70 — PC-MGMT**

Konsep yang digunakan adalah centralized network management, sehingga administrator dari HQ dapat melakukan management terhadap perangkat jaringan di HQ maupun branch melalui jaringan yang telah dirouting.

---

## 4. Jaringan Guest dan Wireless

Jaringan Guest disediakan untuk perangkat tamu dan perangkat wireless.

Perangkat yang digunakan:

- Access Point
- Laptop representative
- Smartphone akan ditambahkan pada tahap berikutnya jika diperlukan

Jaringan Guest akan menggunakan:

**VLAN 50 — GUEST**

Jaringan Guest nantinya akan diberikan pembatasan akses menggunakan ACL agar tidak dapat mengakses jaringan internal perusahaan secara bebas.

---

## 5. Topologi Branch 1

Branch 1 menggunakan desain sederhana tanpa Distribution Layer.

Perangkat yang digunakan:

- 1 Multilayer Switch
- 1 Access Switch
- 1 Edge Router
- Representative end devices
- 1 Access Point

### Struktur

```text
KNOHA-BR1-RTR
      |
KNOHA-BR1-MLS1
      |
KNOHA-BR1-ACC1
      |
  End Devices