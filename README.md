# Jarkom Subnetting Routing

| Nama                    | NRP          |
|-------------------------|--------------|
| Mohammad Abyan Ranuaji   | 5027241106   |

## 1. Desain Topologi

Di bawah ini adalah topologi jaringan yang dirancang menggunakan Cisco Packet Tracer, menunjukkan 2 router (Pusat & Cabang) dan 6 jaringan LAN yang terhubung.

<img width="1109" height="673" alt="image" src="https://github.com/user-attachments/assets/0ce00d9a-cbd4-452c-a28a-04986ef823c0" />


## 2. Tabel Perhitungan VLSM

Tabel berikut merinci alokasi IP untuk setiap jaringan LAN dan WAN, diurutkan dari kebutuhan host terbesar ke terkecil.

<img width="922" height="482" alt="image" src="https://github.com/user-attachments/assets/6a528297-44dc-48d6-8f76-5cbc3bcee6d9" />


## 3. Tabel Agregasi Rute (CIDR / Supernetting)

Untuk efisiensi routing, rute statis dikonfigurasi menggunakan agregasi rute (supernet) untuk semua LAN di Kantor Pusat[cite: 42].

* Rute Agregasi Kantor Pusat: `10.146.0.0/22` (Mencakup `10.146.0.0` s/d `10.146.3.255`)
* Rute LAN Kantor Cabang: `10.146.3.192/27`

<img width="1324" height="1136" alt="image" src="https://github.com/user-attachments/assets/7bf0f119-85fe-46fb-b11a-686d617fe253" />

## 4. Konfigurasi Kunci

Berikut adalah beberapa cuplikan konfigurasi penting dari proyek ini.

### Konfigurasi SVI (Interface VLAN) di Router Pusat
Ini digunakan untuk membuat gateway bagi LAN yang terhubung ke modul switch NIM-ES2-4.

```
--- Konfigurasi Gateway LAN Kurikulum ---

interface Vlan10
description Gateway-LAN-Kurikulum
ip address 10.146.2.1 255.255.255.0
no shutdown

interface GigabitEthernet0/1/0
description To-Switch-Kurikulum
switchport mode access
switchport access vlan 10
no shutdown
```

Konfigurasi Static Routing:

```
--- Di Router Pusat ---
Rute ke LAN Pengawas di Cabang
ip route 10.146.3.192 255.255.255.224 10.146.3.234

--- Di Router Cabang ---
Rute Supernet ke semua jaringan di Kantor Pusat
ip route 10.146.0.0 255.255.252.0 10.146.3.233
```

## 5. Hasil Pengujian (Verifikasi)
Pengujian konektivitas dilakukan dengan ping dari satu PC di jaringan Kantor Pusat (Sekretariat) ke PC di jaringan lain (Kurikulum) dan ke PC di Kantor Cabang (Pengawas Sekolah).

Hasilnya menunjukkan konektivitas penuh antar jaringan.

<img width="384" height="391" alt="Screenshot_155" src="https://github.com/user-attachments/assets/03131393-24a5-450b-a3c9-f6ff5d5cebc5" />
