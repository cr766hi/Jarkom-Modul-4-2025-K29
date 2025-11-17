# LAPORAN RESMI MODUL 4 KOMUNIKASI DATA DAN JARINGAN KOMPUTER

## Jarkom-Modul-4-IT29-2024

### Praktikum Jaringan Komputer Modul 4: Subnetting dan Routing

---

## 👥 Kelompok IT29

| Nama | NRP |
|------|-----|
| Christiano Ronaldo Silalahi | 5027241025 |
| Jofanka Al Kautsar P A | 5027241103 |

---

## 📋 Daftar Isi
- [Topologi](#topologi)
- [Prefix IP](#prefix-ip)
- [Rute dan Subnet](#rute-dan-subnet)
- [Tree CIDR](#tree-cidr)
- [Pembagian IP Address](#pembagian-ip-address)
- [Konfigurasi GNS3](#konfigurasi-gns3)
- [Routing](#routing)
- [Testing](#testing)

---

## 🗺️ Topologi

Praktikum ini menggunakan metode **CIDR (Classless Inter-Domain Routing)** pada **GNS3**. dan **VLSM** pada **Cisco Packet Tracer**

### Topologi GNS3
<img width="1543" height="982" alt="image" src="https://github.com/user-attachments/assets/b359cf02-9098-4502-8055-fb60c4394300" />

### Topologi CPT
<img width="2821" height="1441" alt="Topologi CPT Revisi 2" src="https://github.com/user-attachments/assets/f7df832f-c59d-4884-af8b-845df7e2d5b7" />


### Struktur Topologi
```
Cloud0 (NAT)
    |
Amonsul (Router)
    |
    ├── Minastir (Router)
    │   ├── Amroth (Router)
    │   │   └── Switch
    │   │       ├── Morgoth (Router)
    │   │       │   └── Switch
    │   │       │       ├── Erendis (Client)
    │   │       │       └── Elrond (Client)
    │   │       └── Throne (Router)
    │   │           └── Erebor (Client)
    │   └── Anor (Router)
    │       ├── Beacon (Client)
    │       └── Silmarils (Client)
    │
    ├── Eregion (Router)
    │   ├── Numenor (Router)
    │   │   ├── Mordor (Router)
    │   │   │   └── Erain (Router)
    │   │   │       ├── Switch
    │   │   │       │   ├── Balrog (Client)
    │   │   │       │   ├── Gothmog (Client)
    │   │   │       │   └── Thranduil (Client)
    │   │   │       └── Switch
    │   │   │           ├── Khazad (Client)
    │   │   │           └── Melkor (Client)
    │   │   ├── Switch
    │   │   │   ├── Mirdain (Client)
    │   │   │   └── Arthedain (Client)
    │   │   └── Gudur (Router)
    │   │       ├── Switch
    │   │       │   ├── Hobbiton (Client)
    │   │       │   ├── Grond (Client)
    │   │       │   └── IronCrown (Client)
    │   │       └── Switch
    │   │           ├── Edhil (Client)
    │   │           └── Palantir (Client)
    │   └── Switch
    │       ├── Morgul (Client)
    │       └── Mirkwood (Client)
    │
    └── Fornost (Router)
        └── Switch
            ├── Valmar (Router)
            │   ├── Switch
            │   │   ├── Gwaith (Client)
            │   │   ├── Utumno (Client)
            │   │   └── Imrahil (Client)
            │   └── Switch
            │       ├── Arnor (Client)
            │       └── Doriath (Client)
            └── Valinor (Router)
                └── Switch
                    ├── Lindon (Client)
                    ├── Anarion (Client)
                    └── Shadow (Client)
```

---

## 🌐 Prefix IP

Prefix IP yang digunakan: **10.78.0.0**

---

## 📊 Rute dan Subnet

### Pembagian Rute Subnet A1-A23

| Subnet | Rute | Jumlah IP | Netmask |
|--------|------|-----------|---------|
| A1 | Numenor > Switch > Mirdain, Elrond | 511 | /23 |
| A2 | Throne > Erebor | 7 | /29 |
| A3 | Anor > (Beacon, Silmarils) | 1023 | /22 |
| A4 | (Amroth, Morgoth, Throne) > Switch | 7 | /29 |
| A5 | Minastir > Anor | 3 | /30 |
| A6 | Minastir > Amroth | 3 | /30 |
| A7 | Amonsul > Minastir | 3 | /30 |
| A8 | Erain > Switch > (Balrog, Gothmog, Thranduil) | 511 | /23 |
| A9 | Erain > Switch > (Khazad, Melkor) | 511 | /23 |
| A10 | Numenor > Switch > (Mirdain, Arthedain) | 1023 | /22 |
| A11 | Mordor > Erain | 3 | /30 |
| A12 | Numenor > Mordor | 3 | /30 |
| A13 | Eregion > Numenor | 3 | /30 |
| A14 | Gudur > Switch > (Hobbiton, Grond, IronCrown) | 15 | /28 |
| A15 | Gudur > Switch > (Edhil, Palantir) | 127 | /25 |
| A16 | Numenor > Gudur | 3 | /30 |
| A17 | Eregion > Switch > (Morgul, Mirkwood) | 127 | /25 |
| A18 | Amonsul > Eregion | 3 | /30 |
| A19 | Valmar > Switch > (Gwaith, Utumno, Imrahil) | 63 | /26 |
| A20 | Valmar > Switch > (Arnor, Doriath) | 31 | /27 |
| A21 | Valinor > Switch > (Lindon, Anarion, Shadow) | 511 | /23 |
| A22 | (Fornost, Valmar, Valinor) > Switch | 7 | /29 |
| A23 | Amonsul > Fornost | 3 | /30 |

![Pembagian Rute](./screenshots/pembagian-rute.png)
> *Screenshot tabel pembagian rute subnet*

---

## 🌳 Tree CIDR

### Pengelompokan Subnet dengan CIDR

#### Level I - Penggabungan Awal
![1](https://github.com/user-attachments/assets/44c6a4ff-d342-4825-ac04-0978216be954)

| Subnet | Gabungan dari | Netmask Akhir |
|--------|---------------|---------------|
| B1 | A1 (/23) + A2 (/29) | /22 |
| B2 | A9 (/23) + A8 (/23) | /22 |

#### Level II
![2](https://github.com/user-attachments/assets/b0377d48-d856-41fa-9d44-5a7d64681b9f)

| Subnet | Gabungan dari | Netmask Akhir |
|--------|---------------|---------------|
| C1 | B2 (/22) + A11 (/30) | /21 |
| C2 | A20 (/27) + A19 (/26) | /25 |
| C3 | A15 (/25) + A14 (/28) | /24 |

#### Level III
![3](https://github.com/user-attachments/assets/c2a132df-ed7a-4162-9131-33d29e5113d1)

| Subnet | Gabungan dari | Netmask Akhir |
|--------|---------------|---------------|
| D1 | B1 (/22) + A4 (/29) | /21 |
| D2 | A16 (/30) + C3 (/24) | /23 |
| D3 | A12 (/30) + C1 (/21) | /20 |
| D4 | A21 (/23) + C2 (/25) | /22 |

#### Level IV
![4](https://github.com/user-attachments/assets/b20d3dfb-a897-4a4a-9380-55661ef83dce)

| Subnet | Gabungan dari | Netmask Akhir |
|--------|---------------|---------------|
| E1 | D4 (/22) + A22 (/29) | /21 |
| E2 | D2 (/23) + A10 (/22) | /21 |
| E3 | D1 (/21) + A6 (/30) | /20 |
| E4 | A5 (/30) + A3 (/22) | /21 |

#### Level V
![5](https://github.com/user-attachments/assets/49ab61d8-1be7-42ce-a81c-a0d681856bbd)

| Subnet | Gabungan dari | Netmask Akhir |
|--------|---------------|---------------|
| F1 | E2 (/21) + D3 (/20) | /19 |

#### Level VI
![6](https://github.com/user-attachments/assets/506ad002-a692-47a6-b75b-11dc1cb9b612)

| Subnet | Gabungan dari | Netmask Akhir |
|--------|---------------|---------------|
| G1 | A17 (/25) + F1 (/19) | /18 |
| G2 | E3 (/20) + E4 (/21) | /19 |

#### Level VII
![7](https://github.com/user-attachments/assets/498411a5-fc1d-4984-9824-65f63c690f47)

| Subnet | Gabungan dari | Netmask Akhir |
|--------|---------------|---------------|
| H1 | E1 (/21) + A23 (/30) | /20 |
| H2 | A18 (/30) + G1 (/18) | /17 |
| H3 | G2 (/19) + A7 (/30) | /18 |

#### Level VIII
![8](https://github.com/user-attachments/assets/f20baa0e-2836-4242-8ade-6124504574d2)

| Subnet | Gabungan dari | Netmask Akhir |
|--------|---------------|---------------|
| I1 | H3 (/18) + H2 (/17) | /16 |

#### Level IX
![9](https://github.com/user-attachments/assets/2df5bd28-4802-4ae9-aac7-e124cdc15b07)


| Subnet | Gabungan dari | Netmask Akhir |
|--------|---------------|---------------|
| J1 | H1 (/20) + I1 (/16) | /15 |

### Tree CIDR Lengkap
![Tree CIDR](./screenshots/tree-cidr.png)
> *Screenshot tree CIDR dari level 1 hingga level 9*

---

## 📍 Pembagian IP Address

Berikut adalah pembagian IP address berdasarkan prefix **10.78.0.0** dengan menggunakan tree CIDR yang telah dibuat:

| Subnet | Rute Topologi | Network ID | Netmask | Gateway | Broadcast | Range IP Host | Jumlah Host |
|--------|---------------|------------|---------|---------|-----------|---------------|-------------|
| A1 | Numenor > Switch > Mirdain, Elrond | 10.78.8.0 | /23 | 10.78.8.1 | 10.78.9.255 | 10.78.8.2 - 10.78.9.254 | 510 |
| A2 | Throne > Erebor | 10.78.17.112 | /29 | 10.78.17.113 | 10.78.17.119 | 10.78.17.114 - 10.78.17.118 | 6 |
| A3 | Anor > (Beacon, Silmarils) | 10.78.0.0 | /22 | 10.78.0.1 | 10.78.3.255 | 10.78.0.2 - 10.78.3.254 | 1022 |
| A4 | (Amroth, Morgoth, Throne) > Switch | 10.78.17.120 | /29 | 10.78.17.121 | 10.78.17.127 | 10.78.17.122 - 10.78.17.126 | 6 |
| A5 | Minastir > Anor | 10.78.17.136 | /30 | 10.78.17.137 | 10.78.17.139 | 10.78.17.138 - 10.78.17.138 | 2 |
| A6 | Minastir > Amroth | 10.78.17.140 | /30 | 10.78.17.141 | 10.78.17.143 | 10.78.17.142 - 10.78.17.142 | 2 |
| A7 | Amonsul > Minastir | 10.78.17.144 | /30 | 10.78.17.145 | 10.78.17.147 | 10.78.17.146 - 10.78.17.146 | 2 |
| A8 | Erain > Switch > (Balrog, Gothmog, Thranduil) | 10.78.10.0 | /23 | 10.78.10.1 | 10.78.11.255 | 10.78.10.2 - 10.78.11.254 | 510 |
| A9 | Erain > Switch > (Khazad, Melkor) | 10.78.12.0 | /23 | 10.78.12.1 | 10.78.13.255 | 10.78.12.2 - 10.78.13.254 | 510 |
| A10 | Numenor > Switch > (Mirdain, Arthedain) | 10.78.4.0 | /22 | 10.78.4.1 | 10.78.7.255 | 10.78.4.2 - 10.78.7.254 | 1022 |
| A11 | Mordor > Erain | 10.78.17.148 | /30 | 10.78.17.149 | 10.78.17.151 | 10.78.17.150 - 10.78.17.150 | 2 |
| A12 | Numenor > Mordor | 10.78.17.152 | /30 | 10.78.17.153 | 10.78.17.155 | 10.78.17.154 - 10.78.17.154 | 2 |
| A13 | Eregion > Numenor | 10.78.17.156 | /30 | 10.78.17.157 | 10.78.17.159 | 10.78.17.158 - 10.78.17.158 | 2 |
| A14 | Gudur > Switch > (Hobbiton, Grond, IronCrown) | 10.78.17.96 | /28 | 10.78.17.97 | 10.78.17.111 | 10.78.17.98 - 10.78.17.110 | 14 |
| A15 | Gudur > Switch > (Edhil, Palantir) | 10.78.16.0 | /25 | 10.78.16.1 | 10.78.16.127 | 10.78.16.2 - 10.78.16.126 | 126 |
| A16 | Numenor > Gudur | 10.78.17.160 | /30 | 10.78.17.161 | 10.78.17.163 | 10.78.17.162 - 10.78.17.162 | 2 |
| A17 | Eregion > Switch > (Morgul, Mirkwood) | 10.78.16.128 | /25 | 10.78.16.129 | 10.78.16.255 | 10.78.16.130 - 10.78.16.254 | 126 |
| A18 | Amonsul > Eregion | 10.78.17.164 | /30 | 10.78.17.165 | 10.78.17.167 | 10.78.17.166 - 10.78.17.166 | 2 |
| A19 | Valmar > Switch > (Gwaith, Utumno, Imrahil) | 10.78.17.0 | /26 | 10.78.17.1 | 10.78.17.63 | 10.78.17.2 - 10.78.17.62 | 62 |
| A20 | Valmar > Switch > (Arnor, Doriath) | 10.78.17.64 | /27 | 10.78.17.65 | 10.78.17.95 | 10.78.17.66 - 10.78.17.94 | 30 |
| A21 | Valinor > Switch > (Lindon, Anarion, Shadow) | 10.78.14.0 | /23 | 10.78.14.1 | 10.78.15.255 | 10.78.14.2 - 10.78.15.254 | 510 |
| A22 | (Fornost, Valmar, Valinor) > Switch | 10.78.17.128 | /29 | 10.78.17.129 | 10.78.17.135 | 10.78.17.130 - 10.78.17.134 | 6 |
| A23 | Amonsul > Fornost | 10.78.17.168 | /30 | 10.78.17.169 | 10.78.17.171 | 10.78.17.170 - 10.78.17.170 | 2 |

![Tabel Pembagian IP](./screenshots/tabel-ip.png)
> *Screenshot tabel pembagian IP lengkap*

---

## ⚙️ Konfigurasi GNS3

### Konfigurasi Router

#### 1. Amonsul (Router Utama)
![Config Amonsul](./screenshots/config-amonsul.png)
```bash
# Interface ke Cloud (Internet)
auto eth0
iface eth0 inet dhcp

# Interface ke Minastir (A7)
auto eth1
iface eth1 inet static
    address 10.78.17.145
    netmask 255.255.255.252

# Interface ke Eregion (A18)
auto eth2
iface eth2 inet static
    address 10.78.17.165
    netmask 255.255.255.252

# Interface ke Fornost (A23)
auto eth3
iface eth3 inet static
    address 10.78.17.169
    netmask 255.255.255.252
```

#### 2. Minastir
![Config Minastir](./screenshots/config-minastir.png)
```bash
# Interface ke Amonsul (A7)
auto eth0
iface eth0 inet static
    address 10.78.17.146
    netmask 255.255.255.252
    gateway 10.78.17.145

# Interface ke Anor (A5)
auto eth1
iface eth1 inet static
    address 10.78.17.137
    netmask 255.255.255.252

# Interface ke Amroth (A6)
auto eth2
iface eth2 inet static
    address 10.78.17.141
    netmask 255.255.255.252
```

#### 3. Anor
![Config Anor](./screenshots/config-anor.png)
```bash
# Interface ke Minastir (A5)
auto eth0
iface eth0 inet static
    address 10.78.17.138
    netmask 255.255.255.252
    gateway 10.78.17.137

# Interface ke Switch (A3 - Beacon, Silmarils)
auto eth1
iface eth1 inet static
    address 10.78.0.1
    netmask 255.255.252.0
```

#### 4. Amroth
![Config Amroth](./screenshots/config-amroth.png)
```bash
# Interface ke Minastir (A6)
auto eth0
iface eth0 inet static
    address 10.78.17.142
    netmask 255.255.255.252
    gateway 10.78.17.141

# Interface ke Switch (A4 - Morgoth, Throne)
auto eth1
iface eth1 inet static
    address 10.78.17.121
    netmask 255.255.255.248
```

#### 5. Throne
![Config Throne](./screenshots/config-throne.png)
```bash
# Interface ke Switch A4
auto eth0
iface eth0 inet static
    address 10.78.17.122
    netmask 255.255.255.248
    gateway 10.78.17.121

# Interface ke Erebor (A2)
auto eth1
iface eth1 inet static
    address 10.78.17.113
    netmask 255.255.255.248
```

#### 6. Morgoth
![Config Morgoth](./screenshots/config-morgoth.png)
```bash
# Interface ke Switch A4
auto eth0
iface eth0 inet static
    address 10.78.17.123
    netmask 255.255.255.248
    gateway 10.78.17.121

# Interface ke Switch (A1 - Erendis, Elrond)
auto eth1
iface eth1 inet static
    address 10.78.8.1
    netmask 255.255.254.0
```

#### 7. Eregion
![Config Eregion](./screenshots/config-eregion.png)
```bash
# Interface ke Amonsul (A18)
auto eth0
iface eth0 inet static
    address 10.78.17.166
    netmask 255.255.255.252
    gateway 10.78.17.165

# Interface ke Numenor (A13)
auto eth1
iface eth1 inet static
    address 10.78.17.157
    netmask 255.255.255.252

# Interface ke Switch (A17 - Morgul, Mirkwood)
auto eth2
iface eth2 inet static
    address 10.78.16.129
    netmask 255.255.255.128
```

#### 8. Numenor
![Config Numenor](./screenshots/config-numenor.png)
```bash
# Interface ke Eregion (A13)
auto eth0
iface eth0 inet static
    address 10.78.17.158
    netmask 255.255.255.252
    gateway 10.78.17.157

# Interface ke Mordor (A12)
auto eth1
iface eth1 inet static
    address 10.78.17.153
    netmask 255.255.255.252

# Interface ke Switch (A10 - Mirdain, Arthedain)
auto eth2
iface eth2 inet static
    address 10.78.4.1
    netmask 255.255.252.0

# Interface ke Gudur (A16)
auto eth3
iface eth3 inet static
    address 10.78.17.161
    netmask 255.255.255.252
```

#### 9. Mordor
![Config Mordor](./screenshots/config-mordor.png)
```bash
# Interface ke Numenor (A12)
auto eth0
iface eth0 inet static
    address 10.78.17.154
    netmask 255.255.255.252
    gateway 10.78.17.153

# Interface ke Erain (A11)
auto eth1
iface eth1 inet static
    address 10.78.17.149
    netmask 255.255.255.252
```

#### 10. Erain
![Config Erain](./screenshots/config-erain.png)
```bash
# Interface ke Mordor (A11)
auto eth0
iface eth0 inet static
    address 10.78.17.150
    netmask 255.255.255.252
    gateway 10.78.17.149

# Interface ke Switch (A8 - Balrog, Gothmog, Thranduil)
auto eth1
iface eth1 inet static
    address 10.78.10.1
    netmask 255.255.254.0

# Interface ke Switch (A9 - Khazad, Melkor)
auto eth2
iface eth2 inet static
    address 10.78.12.1
    netmask 255.255.254.0
```

#### 11. Gudur
![Config Gudur](./screenshots/config-gudur.png)
```bash
# Interface ke Numenor (A16)
auto eth0
iface eth0 inet static
    address 10.78.17.162
    netmask 255.255.255.252
    gateway 10.78.17.161

# Interface ke Switch (A14 - Hobbiton, Grond, IronCrown)
auto eth1
iface eth1 inet static
    address 10.78.17.97
    netmask 255.255.255.240

# Interface ke Switch (A15 - Edhil, Palantir)
auto eth2
iface eth2 inet static
    address 10.78.16.1
    netmask 255.255.255.128
```

#### 12. Fornost
![Config Fornost](./screenshots/config-fornost.png)
```bash
# Interface ke Amonsul (A23)
auto eth0
iface eth0 inet static
    address 10.78.17.170
    netmask 255.255.255.252
    gateway 10.78.17.169

# Interface ke Switch (A22 - Valmar, Valinor)
auto eth1
iface eth1 inet static
    address 10.78.17.129
    netmask 255.255.255.248
```

#### 13. Valmar
![Config Valmar](./screenshots/config-valmar.png)
```bash
# Interface ke Switch A22
auto eth0
iface eth0 inet static
    address 10.78.17.130
    netmask 255.255.255.248
    gateway 10.78.17.129

# Interface ke Switch (A19 - Gwaith, Utumno, Imrahil)
auto eth1
iface eth1 inet static
    address 10.78.17.1
    netmask 255.255.255.192

# Interface ke Switch (A20 - Arnor, Doriath)
auto eth2
iface eth2 inet static
    address 10.78.17.65
    netmask 255.255.255.224
```

#### 14. Valinor
![Config Valinor](./screenshots/config-valinor.png)
```bash
# Interface ke Switch A22
auto eth0
iface eth0 inet static
    address 10.78.17.131
    netmask 255.255.255.248
    gateway 10.78.17.129

# Interface ke Switch (A21 - Lindon, Anarion, Shadow)
auto eth1
iface eth1 inet static
    address 10.78.14.1
    netmask 255.255.254.0
```

### Konfigurasi Client

#### Contoh: Beacon (Client di A3)
![Config Beacon](./screenshots/config-beacon.png)
```bash
auto eth0
iface eth0 inet static
    address 10.78.0.2
    netmask 255.255.252.0
    gateway 10.78.0.1
```

#### Contoh: Erebor (Client di A2)
![Config Erebor](./screenshots/config-erebor.png)
```bash
auto eth0
iface eth0 inet static
    address 10.78.17.114
    netmask 255.255.255.248
    gateway 10.78.17.113
```

#### Contoh: Erendis (Client di A1)
![Config Erendis](./screenshots/config-erendis.png)
```bash
auto eth0
iface eth0 inet static
    address 10.78.8.2
    netmask 255.255.254.0
    gateway 10.78.8.1
```

> *Konfigurasi client lainnya mengikuti pola yang sama sesuai dengan subnet dan gateway masing-masing*

![Config All Clients](./screenshots/config-all-clients.png)
> *Screenshot konfigurasi semua client*

---

## 🔀 Routing

### Routing pada Amonsul (Router Utama)
![Routing Amonsul](./screenshots/routing-amonsul.png)
```bash
# Routing ke jaringan Minastir dan cabangnya
route add -net 10.78.0.0 netmask 255.255.252.0 gw 10.78.17.146  # A3 via Minastir
route add -net 10.78.17.136 netmask 255.255.255.252 gw 10.78.17.146  # A5 via Minastir
route add -net 10.78.17.140 netmask 255.255.255.252 gw 10.78.17.146  # A6 via Minastir
route add -net 10.78.17.120 netmask 255.255.255.248 gw 10.78.17.146  # A4 via Minastir
route add -net 10.78.17.112 netmask 255.255.255.248 gw 10.78.17.146  # A2 via Minastir
route add -net 10.78.8.0 netmask 255.255.254.0 gw 10.78.17.146  # A1 via Minastir

# Routing ke jaringan Eregion dan cabangnya
route add -net 10.78.17.156 netmask 255.255.255.252 gw 10.78.17.166  # A13 via Eregion
route add -net 10.78.16.128 netmask 255.255.255.128 gw 10.78.17.166  # A17 via Eregion
route add -net 10.78.4.0 netmask 255.255.252.0 gw 10.78
```

## VLSM
Untuk metode VLSM, tabel routing yang digunakan kurang lebih sama dengan yang di CIDR.

### Routing CPT

#### Client
Pada client, kita dapat melakukan config seperti ini.

![WhatsApp Image 2025-11-17 at 23 37 27](https://github.com/user-attachments/assets/5f346a36-fcb5-4e2d-afec-afa94e3fb605)

Dengan ketentuan sebagai berikut.
```
<IP Client>
<Netmask>
<Gateway>
```

### Switch
Jika topologi tersebut terdapat switch, maka dapat melakukan konfigurasi seperti ini.

![WhatsApp Image 2025-11-17 at 23 37 49](https://github.com/user-attachments/assets/5620cc90-928a-4e61-bae1-5a65807a05ab)

Diaman untuk interface yang langsung mengarah ke router diseeting modeny ke trunk.

### Router
Pada router yang memiliki client, dapat melakukan konfigurasi seperti ini.

![WhatsApp Image 2025-11-17 at 23 38 16](https://github.com/user-attachments/assets/b7620a2a-065f-4d52-b468-f3939177645e)

Dengan format:
```
<Gateway>
<Netmask>
```


### Cara Routing

Cara biar router ke router bisa connect, misalnya terdapat topologi seperti ini.
```
router1 <-> router2
```

a = yang terhubung dengan router itu sendiri

Kita dapat melakukan konfigurasi pada masing-masing router.

Di Router1: 
```
ip route <NID Router 2 a> <netmask a> <IP Router 2>
```
Di Router 2:
```
ip route <NID Router 1 a> <netmask a> <IP Router 1>
```

agar kedua router dapat terhubung ke client, diperlukan ip routing, baik router1 maupun ke router2. Setiap router perlu membuat ip routing ke masing-masing client. Untuk kodenya seperti ini.

di router1: 
```
ip route <NID Client> <netmask client> <IP Router 2 a>
```





