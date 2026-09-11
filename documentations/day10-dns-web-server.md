# Day 10 — DNS + Web Server

## Tujuan

Mengimplementasikan layanan DNS dan Web Server pada jaringan PT Konoha agar client dapat mengakses layanan web menggunakan hostname/domain internal.

Selain itu, HTTPS diaktifkan untuk menyediakan akses web melalui koneksi yang terenkripsi dalam simulasi jaringan.

---

## DNS Design

DNS Server ditempatkan pada Server VLAN di HQ.

| Server | IP Address | Subnet Mask | Default Gateway | VLAN |
|---|---|---|---|---|
| DNS Server | 10.10.80.3 | 255.255.255.240 | 10.10.80.1 | VLAN 80 |

DNS Server digunakan untuk melakukan name resolution terhadap layanan internal PT Konoha.

---

## DNS Records

DNS record dibuat untuk mengarahkan hostname internal menuju Web Server.

### A Record

konoha.local       → 10.10.80.4
www.konoha.local   → 10.10.80.4

### CNAME Record

Alias dibuat untuk menyediakan hostname alternatif menuju Web Server:

portal.konoha.local
        ↓
www.konoha.local
        ↓
10.10.80.4

Dengan konfigurasi tersebut, client dapat mengakses Web Server menggunakan:

https://portal.konoha.local

## Web Server Design

Web Server ditempatkan pada Server VLAN di HQ.

Server	IP Address	Subnet Mask	Default Gateway	VLAN
Web Server	10.10.80.4	255.255.255.240	10.10.80.1	VLAN 80

Web Server digunakan untuk menyediakan internal website PT Konoha yang dapat diakses oleh client dari seluruh site.

## HTTP & HTTPS

Service HTTP dan HTTPS diaktifkan pada Web Server.

HTTP  : ON
HTTPS : ON

Akses utama yang digunakan untuk pengujian:

https://konoha.local

dan:

https://portal.konoha.local

HTTPS digunakan sebagai simulasi secure web access pada jaringan internal PT Konoha.

Catatan: Implementasi HTTPS pada Packet Tracer merupakan simulasi layanan HTTPS dan tidak merepresentasikan implementasi sertifikat TLS/PKI production-grade.

## Website

Halaman website dibuat sebagai internal website PT Konoha.

Konten utama website:

PT Konoha
Enterprise Network Infrastructure

Welcome to PT Konoha

Website juga menampilkan informasi mengenai:

Enterprise Network Infrastructure
Network Infrastructure
DHCP
DNS
Web Server
OSPF
Enterprise Sites

Website digunakan sebagai representasi layanan internal perusahaan.

## DNS Verification

Pengujian dilakukan dari client pada jaringan HQ, Branch 1, dan Branch 2.

Hostname yang diuji:

konoha.local
portal.konoha.local

DNS berhasil melakukan name resolution menuju Web Server:

10.10.80.4

## Web Access Testing

Pengujian akses website dilakukan dari seluruh site.

### HQ

Client HQ berhasil mengakses:

https://portal.konoha.local

Website berhasil ditampilkan dan menampilkan halaman PT Konoha.

### Branch 1

Client Branch 1 berhasil mengakses:

https://portal.konoha.local

Website berhasil ditampilkan melalui konektivitas jaringan antar-site.

### Branch 2

Client Branch 2 berhasil mengakses:

https://portal.konoha.local

Website berhasil ditampilkan melalui konektivitas jaringan antar-site.

## End-to-End Service Flow

Alur akses layanan:

Client
   |
   | HTTPS Request
   v
DNS Server
10.10.80.3
   |
   | DNS Resolution
   v
portal.konoha.local
   |
   | CNAME
   v
www.konoha.local
   |
   | A Record
   v
10.10.80.4
   |
   v
Web Server
   |
   v
PT Konoha Website

Service dapat diakses dari:

HQ
Branch 1
Branch 2

## Connectivity Testing

Hasil pengujian:

DNS Server berhasil melakukan name resolution.
konoha.local berhasil diarahkan ke Web Server.
www.konoha.local berhasil diarahkan ke Web Server.
CNAME portal.konoha.local berhasil digunakan sebagai alias.
Web Server berhasil melayani request HTTPS.
Client HQ berhasil mengakses website.
Client Branch 1 berhasil mengakses website.
Client Branch 2 berhasil mengakses website.
Website menampilkan halaman PT Konoha.
Layanan dapat diakses lintas-site melalui jaringan OSPF.

## Screenshot Evidence

Screenshot hasil konfigurasi dan pengujian disimpan pada:

screenshots/day 10/

Bukti yang digunakan:

dns-record.png
web-server-config.png
https-web-access-hq.png
https-web-access-br1.png
https-web-access-br2.png

Screenshot menunjukkan konfigurasi DNS, Web Server, serta keberhasilan akses website melalui HTTPS dari HQ, Branch 1, dan Branch 2.

## Hasil

Implementasi DNS dan Web Server berhasil dilakukan pada jaringan PT Konoha.

DNS Server berhasil menyediakan name resolution untuk layanan internal perusahaan.

Web Server berhasil menyediakan website internal PT Konoha dan dapat diakses menggunakan hostname melalui HTTPS.

Pengujian dari HQ, Branch 1, dan Branch 2 berhasil dilakukan sehingga layanan web dapat digunakan secara lintas-site.

## Kesimpulan

Day 10 berhasil diselesaikan.

Jaringan PT Konoha sekarang memiliki layanan DNS dan Web Server yang terintegrasi dengan jaringan enterprise.

DNS memungkinkan client mengakses layanan menggunakan hostname, sedangkan Web Server menyediakan layanan website internal yang dapat diakses melalui HTTPS.

Implementasi ini menjadi dasar untuk pengembangan network services berikutnya pada PT Konoha.