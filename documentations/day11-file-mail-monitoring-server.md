# Day 11 — File, Mail & Monitoring Server

## Objective

Mengimplementasikan server layanan tambahan pada jaringan PT Konoha, yaitu:

- File Server
- Mail Server
- Monitoring Server

Seluruh server ditempatkan pada **VLAN 80 — SERVER** di HQ.

---

## Server Infrastructure

| Server | IP Address | Function |
|---|---|---|
| File Server | `10.10.80.5` | FTP / File Sharing |
| Mail Server | `10.10.80.6` | Internal Email |
| Monitoring Server | `10.10.80.7` | Network Monitoring |

Gateway Server VLAN:

`10.10.80.1/28`

---

## File Server

File Server menggunakan IP:

`10.10.80.5`

FTP Service diaktifkan untuk menyediakan layanan transfer file.

### FTP Account

| Username | Password | Permission |
|---|---|---|
| admin | `konoha123` | Read, Write, Delete, Rename, List |

### FTP Testing

Pengujian dilakukan dari PC:

ftp 10.10.80.5

Hasil pengujian:

FTP server berhasil diakses.
Login menggunakan user admin berhasil.
Perintah dir berhasil menampilkan isi FTP directory.

## Mail Server

Mail Server menggunakan IP:

10.10.80.6

Email Service diaktifkan untuk menyediakan komunikasi email internal.

### Mail Account
Username	Email
it	it@konoha.com
hr	hr@konoha.com

### Mail Server Configuration
Incoming Mail Server : 10.10.80.6
Outgoing Mail Server : 10.10.80.6

### Email Testing

Pengujian dilakukan dengan mengirim email dari:

it@konoha.com

ke:

hr@konoha.com

Subject:

test

Hasil pengujian:

Email berhasil dikirim.
Email berhasil diterima oleh user tujuan.
Mail Server berhasil digunakan untuk komunikasi internal.

## Monitoring Server

Monitoring Server menggunakan IP:

10.10.80.7

Server digunakan sebagai bagian dari infrastruktur monitoring jaringan PT Konoha.

Konfigurasi monitoring lanjutan seperti SNMP, Syslog, dan NTP akan dikembangkan pada Day 18.

### Connectivity Testing

Dari client dilakukan pengujian:

ping 10.10.80.5
ping 10.10.80.6
ping 10.10.80.7

Hasil:

File Server dapat dijangkau.
Mail Server dapat dijangkau.
Monitoring Server dapat dijangkau.

## Testing Result
Test	Result
FTP Connection	✅ Success
FTP Login	✅ Success
FTP Directory Listing	✅ Success
Mail Sending	✅ Success
Mail Receiving	✅ Success
Monitoring Server Connectivity	✅ Success

## Kesimpulan

Day 11 berhasil mengimplementasikan File Server, Mail Server, dan Monitoring Server pada jaringan PT Konoha.

FTP berhasil digunakan untuk transfer file, Mail Server berhasil digunakan untuk komunikasi email internal, dan Monitoring Server berhasil terhubung dengan jaringan.