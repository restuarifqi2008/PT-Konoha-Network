# Day 12 — SSH, Network Management & Hardening

## Objective

Melakukan konfigurasi remote management menggunakan SSH pada perangkat jaringan PT Konoha serta menerapkan konfigurasi dasar untuk meningkatkan keamanan akses perangkat.

## SSH Configuration

SSH dikonfigurasi pada perangkat:

- HQ Core Switch
- HQ Distribution Switch
- HQ Access Switch
- HQ Server Access Switch
- Branch MLS
- Branch Access Switch
- HQ Router
- Branch Router

Konfigurasi dasar SSH yang digunakan:

cisco
ip domain-name konoha.local
username admin privilege 15 secret KonohaSSH123
crypto key generate rsa
ip ssh version 2

line vty 0 15
 login local
 transport input ssh

service password-encryption

## Network Management Addressing

Management menggunakan VLAN 60 (NET-MGMT).

### HQ
Device	Management IP
CORE1	10.10.60.1
CORE2	10.10.60.3
DIST1	10.10.60.4
DIST2	10.10.60.5
ACC1	10.10.60.6
ACC2	10.10.60.7
SACC1	10.10.60.8
SACC2	10.10.60.9

### Branch 1
Device	Management IP
BR1-MLS1	10.20.60.1
BR1-ACC1	10.20.60.3

### Branch 2
Device	Management IP
BR2-MLS1	10.30.60.1
BR2-ACC1	10.30.60.3

Router menggunakan IP interface routed/transit untuk remote management:

Device	SSH IP
HQ-RTR	10.254.0.1
BR1-RTR	10.254.0.9
BR2-RTR	10.254.0.13

## Security Hardening

Beberapa konfigurasi dasar hardening diterapkan:

Menggunakan enable secret
Menggunakan local user authentication
Mengaktifkan SSH version 2
Menonaktifkan akses Telnet pada VTY
Mengaktifkan password encryption
Menggunakan privilege level 15 untuk administrator
SSH Testing

Pengujian SSH dilakukan dari PC menggunakan format:

ssh -l admin <management-ip>

Hasil pengujian:

Router HQ dan Branch berhasil diakses menggunakan SSH.
Core Switch berhasil dikonfigurasi dan diuji menggunakan SSH.
Distribution Switch berhasil diakses menggunakan SSH.
Access Switch berhasil diakses menggunakan SSH.
Branch MLS berhasil diakses menggunakan SSH.
Sebagian besar perangkat berhasil melakukan remote management melalui SSH.

CORE2 masih memerlukan troubleshooting lanjutan terkait akses SSH dari PC menuju management IP 10.10.60.3. Konfigurasi SSH pada CORE2 sudah aktif, namun konektivitas dari PC menuju IP tersebut masih perlu diperiksa lebih lanjut.

## STP Consideration

STP tetap menggunakan desain:

CORE1 sebagai Root Primary.
CORE2 sebagai Root Secondary.

Port yang berada pada status Alternate/Blocking tetap dipertahankan karena merupakan mekanisme normal STP untuk mencegah network loop.

Tidak dilakukan perubahan STP hanya untuk mengatasi masalah akses SSH CORE2.

## Testing Result
Test	Result
SSH Router	PASS
SSH Core Switch	PASS
SSH Distribution Switch	PASS
SSH Access Switch	PASS
SSH Branch MLS	PASS
SSH CORE2	PENDING
SSH menggunakan Telnet	BLOCKED
Conclusion

Day 12 berhasil menerapkan SSH sebagai metode remote management utama pada jaringan PT Konoha. Akses Telnet dinonaktifkan dan local authentication digunakan untuk meningkatkan keamanan administrasi perangkat.

Sebagian besar perangkat jaringan berhasil diakses melalui SSH. Troubleshooting akses SSH menuju CORE2 akan dilanjutkan pada tahap berikutnya tanpa mengubah desain STP Root Primary dan Root Secondary yang telah diterapkan.