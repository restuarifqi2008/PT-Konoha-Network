# Day 9 — DHCP & DHCP Relay

## Tujuan

Mengimplementasikan layanan DHCP terpusat pada jaringan PT Konoha agar client di HQ, Branch 1, dan Branch 2 dapat memperoleh konfigurasi IP secara otomatis.

Selain itu, DHCP Relay digunakan agar DHCP request dari jaringan Branch 1 dan Branch 2 dapat diteruskan menuju DHCP Server yang berada di HQ.

---

## DHCP Design

PT Konoha menggunakan centralized DHCP Server yang ditempatkan pada Server VLAN di HQ.

| Server | IP Address | Subnet Mask | Default Gateway | VLAN |
|---|---|---|---|---|
| DHCP Server | 10.10.80.2 | 255.255.255.240 | 10.10.80.1 | VLAN 80 |

DNS Server yang akan digunakan pada konfigurasi DHCP:

## DHCP Pool

DHCP pool dibuat untuk VLAN user pada seluruh site.

HQ
VLAN	Department	Network	Gateway
10	IT	10.10.10.0/26	10.10.10.1
20	HR	10.10.20.0/26	10.10.20.1
30	OPERASIONAL	10.10.30.0/25	10.10.30.1
40	FINANCE	10.10.40.0/26	10.10.40.1
50	GUEST	10.10.50.0/24	10.10.50.1

Branch 1
VLAN	Department	Network	Gateway
10	IT	10.20.10.0/28	10.20.10.1
20	HR	10.20.20.0/28	10.20.20.1
30	OPERASIONAL	10.20.30.0/27	10.20.30.1
40	FINANCE	10.20.40.0/27	10.20.40.1
50	GUEST	10.20.50.0/26	10.20.50.1

Branch 2
VLAN	Department	Network	Gateway
10	IT	10.30.10.0/28	10.30.10.1
20	HR	10.30.20.0/28	10.30.20.1
30	OPERASIONAL	10.30.30.0/27	10.30.30.1
40	FINANCE	10.30.40.0/27	10.30.40.1
50	GUEST	10.30.50.0/26	10.30.50.1

## DHCP Relay

DHCP Server berada di HQ pada jaringan:

10.10.80.2

Karena DHCP request menggunakan broadcast dan tidak dapat melewati perangkat Layer 3 secara langsung, DHCP Relay digunakan untuk meneruskan request dari setiap VLAN menuju DHCP Server.

DHCP Relay dikonfigurasi menggunakan:

ip helper-address 10.10.80.2

Konfigurasi ip helper-address diterapkan pada SVI VLAN user yang membutuhkan layanan DHCP.

HQ

DHCP Relay diterapkan pada:

VLAN 10 — IT
VLAN 20 — HR
VLAN 30 — OPERASIONAL
VLAN 40 — FINANCE
VLAN 50 — GUEST

Branch 1
DHCP Relay diterapkan pada:

VLAN 10 — IT
VLAN 20 — HR
VLAN 30 — OPERASIONAL
VLAN 40 — FINANCE
VLAN 50 — GUEST

Branch 2
DHCP Relay diterapkan pada:

VLAN 10 — IT
VLAN 20 — HR
VLAN 30 — OPERASIONAL
VLAN 40 — FINANCE
VLAN 50 — GUEST

VLAN yang digunakan untuk management dan server tetap menggunakan konfigurasi IP sesuai kebutuhan jaringan dan tidak menjadi bagian dari DHCP pool user.

## DHCP Verification

Pengujian dilakukan dengan mengubah konfigurasi IP pada PC menjadi DHCP.

HQ IT Client

Hasil konfigurasi DHCP:

IP Address      : 10.10.10.10
Subnet Mask     : 255.255.255.192
Default Gateway : 10.10.10.1

Client HQ berhasil memperoleh IP address secara otomatis sesuai dengan subnet VLAN 10 HQ.

Branch 1 IT Client

Hasil konfigurasi DHCP:

IP Address      : 10.20.10.10
Subnet Mask     : 255.255.255.240
Default Gateway : 10.20.10.1

Client Branch 1 berhasil memperoleh IP address secara otomatis melalui DHCP Relay menuju DHCP Server di HQ.

Branch 2 IT Client

Hasil konfigurasi DHCP:

IP Address      : 10.30.10.11
Subnet Mask     : 255.255.255.240
Default Gateway : 10.30.10.1

Client Branch 2 berhasil memperoleh IP address secara otomatis melalui DHCP Relay menuju DHCP Server di HQ.

## Connectivity Testing

Setelah seluruh client berhasil memperoleh IP address melalui DHCP, dilakukan pengujian konektivitas antar-site.

Hasil pengujian:

Client HQ berhasil memperoleh IP address secara otomatis.
Client Branch 1 berhasil memperoleh IP address secara otomatis.
Client Branch 2 berhasil memperoleh IP address secara otomatis.
IP address yang diperoleh sesuai dengan subnet VLAN masing-masing.
Default gateway yang diperoleh sesuai dengan gateway VLAN masing-masing.
DHCP Relay berhasil meneruskan request DHCP dari Branch 1 dan Branch 2 menuju DHCP Server di HQ.
Client antar-site berhasil saling melakukan ping.

## Screenshot Evidence

Screenshot hasil pengujian disimpan pada folder:

screenshots/day 09/

File bukti utama:

dhcp-hq-client.png
dhcp-br1-client.png
dhcp-br2-client.png

Screenshot menunjukkan bahwa client pada HQ, Branch 1, dan Branch 2 berhasil memperoleh IP address, subnet mask, dan default gateway secara otomatis melalui DHCP.

## Hasil

Implementasi centralized DHCP berhasil dilakukan.

DHCP Server di HQ dapat memberikan layanan DHCP kepada client pada:

HQ
Branch 1
Branch 2

DHCP Relay berhasil meneruskan request DHCP dari jaringan yang berbeda menuju DHCP Server pusat di HQ.

Seluruh client yang diuji berhasil memperoleh konfigurasi IP secara otomatis dan tetap dapat berkomunikasi dengan perangkat pada site lain.

## Kesimpulan

Day 9 berhasil menyelesaikan implementasi DHCP dan DHCP Relay pada jaringan PT Konoha.

Centralized DHCP mempermudah pengelolaan alamat IP karena layanan DHCP dikelola dari satu lokasi, yaitu HQ.

DHCP Relay memungkinkan client pada Branch 1 dan Branch 2 tetap menggunakan DHCP Server yang berada di HQ meskipun berada pada jaringan yang berbeda.

Seluruh pengujian DHCP dan konektivitas antar-site berhasil dilakukan.
