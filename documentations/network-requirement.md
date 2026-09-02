# PT Konoha Enterprise Network

## Network Requirements

### 1. Project Overview

PT Konoha merupakan perusahaan skala menengah yang memiliki satu Headquarters (HQ) dan dua Branch Office. Perusahaan memiliki empat divisi utama, yaitu IT, HR, Operasional, dan Finance.

HQ berfungsi sebagai pusat jaringan perusahaan dan memiliki Server Cluster yang menyediakan berbagai layanan jaringan dan layanan internal perusahaan. Branch 1 dan Branch 2 terhubung ke HQ melalui jaringan WAN.

Proyek ini bertujuan untuk membangun simulasi jaringan enterprise PT Konoha menggunakan Cisco Packet Tracer dengan menerapkan konsep network segmentation, routing, network services, network security, redundancy, monitoring, testing, dan troubleshooting.

### 2. Project Objectives

Tujuan pembangunan simulasi jaringan ini adalah:

* Membuat desain jaringan enterprise yang terstruktur dan scalable.
* Memisahkan jaringan berdasarkan divisi menggunakan VLAN.
* Menerapkan Inter-VLAN Routing menggunakan Multilayer Switch.
* Menghubungkan HQ, Branch 1, dan Branch 2 menggunakan routing OSPF.
* Menyediakan layanan jaringan seperti DHCP dan DNS.
* Menyediakan server internal perusahaan.
* Menerapkan network security menggunakan ACL, SSH, Port Security, dan basic device hardening.
* Menyediakan network management dan PC management network.
* Menyediakan Guest Network yang terisolasi dari jaringan internal.
* Melakukan pengujian konektivitas, routing, security, dan troubleshooting.
* Menghasilkan dokumentasi proyek yang dapat digunakan sebagai portfolio jaringan enterprise.

### 3. Site Structure

PT Konoha memiliki tiga lokasi utama:

| Site     | Keterangan                                 |
| -------- | ------------------------------------------ |
| HQ       | Headquarters dan pusat jaringan perusahaan |
| Branch 1 | Cabang pertama                             |
| Branch 2 | Cabang kedua                               |

HQ juga memiliki Server Cluster yang ditempatkan dalam lokasi HQ dan digunakan untuk menyediakan layanan jaringan serta layanan internal perusahaan.

### 4. Organizational Structure

Setiap site memiliki empat divisi utama:

| Division    | Description                                  |
| ----------- | -------------------------------------------- |
| IT          | Infrastruktur, jaringan, dan dukungan teknis |
| HR          | Pengelolaan sumber daya manusia              |
| Operasional | Aktivitas operasional perusahaan             |
| Finance     | Pengelolaan keuangan perusahaan              |

### 5. User Estimation

Jumlah user yang digunakan dalam simulasi ditentukan berdasarkan skenario perusahaan skala menengah.

#### HQ

| Division    | Estimated Users |
| ----------- | --------------: |
| IT          |              15 |
| HR          |              15 |
| Operasional |              35 |
| Finance     |              20 |
| **Total**   |          **85** |

#### Branch 1

| Division    | Estimated Users |
| ----------- | --------------: |
| IT          |               5 |
| HR          |               5 |
| Operasional |              15 |
| Finance     |              10 |
| **Total**   |          **35** |

#### Branch 2

| Division    | Estimated Users |
| ----------- | --------------: |
| IT          |               5 |
| HR          |               5 |
| Operasional |              15 |
| Finance     |              10 |
| **Total**   |          **35** |

Total estimasi user pada seluruh perusahaan adalah **155 user**.

### 6. VLAN Requirements

Jaringan PT Konoha menggunakan segmentasi VLAN untuk memisahkan traffic berdasarkan fungsi dan kebutuhan keamanan.

| VLAN ID | VLAN Name        | Function                                          |
| ------: | ---------------- | ------------------------------------------------- |
|      10 | IT               | User IT                                           |
|      20 | HR               | User HR                                           |
|      30 | OPERASIONAL      | User Operasional                                  |
|      40 | FINANCE          | User Finance                                      |
|      50 | GUEST            | Guest network                                     |
|      60 | NET-MGMT         | Management perangkat jaringan                     |
|      70 | PC-MGMT          | PC administrator/management                       |
|      80 | SERVER           | Server network                                    |
|     999 | NATIVE-BLACKHOLE | Native VLAN dan isolasi port yang tidak digunakan |

Setiap VLAN akan menggunakan subnet IP yang berbeda untuk memastikan segmentasi jaringan dan mempermudah pengelolaan serta penerapan security policy.

### 7. Network Architecture

Arsitektur jaringan menggunakan konsep hierarchical network design.

#### HQ

HQ menggunakan tiga layer utama:

**Core Layer**

* 2 Core Switch

**Distribution Layer**

* 2 Distribution Switch untuk jaringan user HQ
* 2 Distribution Switch untuk Server Cluster

**Access Layer**

* 2 Access Switch untuk user HQ
* 2 Access Switch untuk Server Cluster

Server Cluster ditempatkan di HQ dan dipisahkan dari jaringan user untuk meningkatkan struktur dan keamanan jaringan.

#### Branch 1

Branch 1 menggunakan:

* 1 Multilayer Switch
* 1 Access Switch

Multilayer Switch berfungsi sebagai perangkat Layer 3 untuk Inter-VLAN Routing dan koneksi routing menuju WAN.

#### Branch 2

Branch 2 menggunakan:

* 1 Multilayer Switch
* 1 Access Switch

Multilayer Switch berfungsi sebagai perangkat Layer 3 untuk Inter-VLAN Routing dan koneksi routing menuju WAN.

### 8. Routing Requirements

Routing antar-site menggunakan **OSPF (Open Shortest Path First)**.

OSPF digunakan untuk menghubungkan:

* HQ
* Branch 1
* Branch 2

Inter-VLAN Routing menggunakan **Multilayer Switch** sehingga setiap VLAN dapat berkomunikasi melalui Layer 3 berdasarkan security policy yang diterapkan.

### 9. WAN Requirements

HQ, Branch 1, dan Branch 2 akan terhubung melalui simulasi jaringan WAN menggunakan perangkat yang tersedia pada Cisco Packet Tracer.

Topologi WAN secara konseptual:

```text
              HQ
               |
           WAN Cloud
           /       \
          /         \
    Branch 1       Branch 2
```

### 10. Server Requirements

Server Cluster ditempatkan di HQ.

Server yang digunakan dalam simulasi:

| Server            | Function                                            |
| ----------------- | --------------------------------------------------- |
| DHCP Server       | Memberikan IP address secara otomatis kepada client |
| DNS Server        | Name resolution                                     |
| Web Server        | Menyediakan internal web service                    |
| File Server       | Penyimpanan dan pertukaran file                     |
| Mail Server       | Simulasi layanan email perusahaan                   |
| Monitoring Server | Monitoring perangkat dan network service            |

Server akan ditempatkan pada **VLAN 80 (SERVER)**.

### 11. Network Security Requirements

Jaringan PT Konoha akan menerapkan beberapa mekanisme keamanan:

* Access Control List (ACL)
* SSH untuk remote management
* Port Security
* Disable unused ports
* Management VLAN
* PC Management VLAN
* Guest Network isolation
* Native VLAN khusus
* Basic device hardening
* Password protection
* Login banner

Security policy akan dikembangkan lebih detail pada tahap implementasi ACL dan security configuration.

### 12. High Availability and Redundancy

Untuk meningkatkan reliability, beberapa komponen jaringan HQ akan menggunakan redundant devices dan links.

Redundancy yang direncanakan meliputi:

* 2 Core Switch
* 2 Distribution Switch pada jaringan user HQ
* 2 Distribution Switch pada Server Cluster
* Redundant links
* EtherChannel
* Spanning Tree Protocol

Implementasi redundancy akan disesuaikan dengan kemampuan perangkat Cisco Packet Tracer yang digunakan.

### 13. Network Management

Network management akan menggunakan:

**VLAN 60 - NET-MGMT**

VLAN ini digunakan untuk management perangkat jaringan seperti:

* Core Switch
* Distribution Switch
* Multilayer Switch
* Access Switch
* perangkat jaringan lainnya

Remote management akan menggunakan **SSH**.

### 14. PC Management

**VLAN 70 - PC-MGMT** digunakan untuk PC administrator atau workstation khusus yang digunakan untuk melakukan management terhadap perangkat jaringan dan sistem internal tertentu.

Akses terhadap VLAN ini akan dibatasi menggunakan security policy dan ACL.

### 15. Guest Network

**VLAN 50 - GUEST** digunakan untuk perangkat tamu.

Guest Network harus memiliki akses yang terbatas dan tidak diperbolehkan mengakses jaringan internal perusahaan secara bebas.

Konsep policy:

```text
Guest Network
     |
     +----> Allowed Services
     |
     X
Internal Network
```

Detail policy Guest Network akan diimplementasikan menggunakan ACL.

### 16. Required Network Features

Proyek ini direncanakan memiliki fitur berikut:

* VLAN
* Inter-VLAN Routing
* Trunking
* EtherChannel
* Spanning Tree Protocol
* OSPF
* DHCP
* DNS
* Web Server
* File Server
* Mail Server
* Monitoring Server
* Management VLAN
* PC Management VLAN
* Guest VLAN
* ACL
* SSH
* Port Security
* Basic Device Hardening
* Connectivity Testing
* Routing Testing
* Security Testing
* Troubleshooting

### 17. Project Constraints

Proyek dibangun menggunakan **Cisco Packet Tracer**, sehingga pemilihan perangkat dan fitur akan disesuaikan dengan fitur yang tersedia pada simulator.

Desain juga harus tetap manageable untuk diselesaikan dalam periode pengembangan proyek selama 20 hari.

### 18. Expected Final Result

Pada akhir proyek diharapkan terbentuk simulasi jaringan enterprise PT Konoha yang:

1. Memiliki segmentasi jaringan berdasarkan divisi dan fungsi.
2. Memiliki konektivitas HQ, Branch 1, dan Branch 2.
3. Menggunakan OSPF untuk routing antar-site.
4. Menggunakan Multilayer Switch untuk Inter-VLAN Routing.
5. Memiliki Server Cluster di HQ.
6. Memiliki network management dan PC management network.
7. Memiliki Guest Network yang terisolasi dari jaringan internal.
8. Memiliki mekanisme security dasar.
9. Dapat diuji dan dibuktikan menggunakan connectivity test dan troubleshooting.
10. Memiliki dokumentasi teknis yang lengkap untuk kebutuhan portfolio.
