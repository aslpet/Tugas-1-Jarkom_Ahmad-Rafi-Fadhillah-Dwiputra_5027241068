# Tugas-1-Jarkom_Ahmad-Rafi-Fadhillah-Dwiputra_5027241068
tugas1/2 (?) Jarkom

# Laporan Tugas Subnetting & Routing
**Ahmad Rafi Fadhillah Dwiputra - 5027241068**

Berikut adalah dokumentasi perhitungan Variable Length Subnet Masking (VLSM) dan Classless Inter-Domain Routing (CIDR) untuk topologi jaringan yang diusulkan.
<img width="872" height="628" alt="Topologi" src="https://github.com/user-attachments/assets/159354a3-3782-4957-9090-7229a06f1e17" />

mod: 108
NRP : 5027241068
BaseNw: 10.108.0.0

## 1. Analisis Kebutuhan Host (Subnetting)

Tabel ini merangkum kebutuhan host untuk setiap unit kerja, yang menjadi dasar untuk alokasi subnet.
<img width="971" height="727" alt="Subnetting" src="https://github.com/user-attachments/assets/6ec4e013-d694-4d6f-93c0-c95fa93d2145" />

| Kebutuhan Ruang | Jml Host (Termasuk Gateway) | Prefix | Block Size |
| :--- | :--- | :--- | :--- |
| Sekretariat | 381 | /23 | 512 |
| Bidang Kurikulum | 221 | /24 | 256 |
| Bidang Guru & Tendik | 96 | /25 | 128 |
| Bidang Sarana Prasarana | 46 | /26 | 64 |
| Bidang Pengawas Sekolah | 19 | /27 | 32 |
| Server & Admin | 7 | /28 | 16 |
| Interlink Kantor Pusat | 6 | /29 | 8 |
| Link WAN (P2P) | 2 | /30 | 4 |
| **TOTAL HOSTS** | **778** | **/22** | **1024** |

## 2. Perhitungan VLSM

Tabel ini menunjukkan alokasi VLSM berdasarkan kebutuhan host, diurutkan dari yang terbesar ke terkecil.
![VLSM](https://github.com/user-attachments/assets/cb88fdd8-17b7-44f0-b97e-9fff30305614)

| Kebutuhan Ruang | Jml Host (Termasuk Gateway) | Block Size | Prefix | Network Address | Subnet Mask | Usable IP Range | Broadcast | Gateway (Contoh) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Sekretariat | 381 | 512 | /23 | 10.108.2.0 | 255.255.254.0 | 10.108.2.1 - 10.108.3.254 | 10.108.3.255 | 10.108.2.1 |
| Bidang Kurikulum | 221 | 256 | /24 | 10.108.1.0 | 255.255.255.0 | 10.108.1.1 - 10.108.1.254 | 10.108.1.255 | 10.108.1.1 |
| Bidang Guru & Tendik | 96 | 128 | /25 | 10.108.0.128 | 255.255.255.128 | 10.108.0.129 - 10.108.0.254 | 10.108.0.255 | 10.108.0.129 |
| Bidang Sarana Prasarana | 46 | 64 | /26 | 10.108.0.64 | 255.255.255.192 | 10.108.0.65 - 10.108.0.126 | 10.108.0.127 | 10.108.0.65 |
| Bidang Pengawas Sekolah | 19 | 32 | /27 | 10.108.0.32 | 255.255.255.224 | 10.108.0.33 - 10.108.0.62 | 10.108.0.63 | 10.108.0.33 |
| Server & Admin | 7 | 16 | /28 | 10.108.0.16 | 255.255.255.240 | 10.108.0.17 - 10.108.0.30 | 10.108.0.31 | 10.108.0.17 |
| Interlink Kantor Pusat | 6 | 8 | /29 | 10.108.0.8 | 255.255.255.248 | 10.108.0.9 - 10.108.0.14 | 10.108.0.15 | 10.108.0.9 |
| Link WAN (P2P) | 2 | 4 | /30 | 10.108.0.0 | 255.255.255.252 | 10.108.0.1 - 10.108.0.2 | 10.108.0.3 | 10.108.0.1 |

## 3. Desain CIDR & Supernetting

Perhitungan berikut dirancang untuk alokasi CIDR yang *contiguous* (berurutan) agar memungkinkan *route aggregation* (supernetting) yang efisien.

### 3.1. Tabel Alokasi Subnet (CIDR)
<img width="805" height="570" alt="CIDR" src="https://github.com/user-attachments/assets/3bc2f9d0-20eb-445d-a790-3d208949ae1d" />

| Subnet | Nama Subnet | Network / Prefix | Network Address | Broadcast | Range Host (usable) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| A1 | Sekretariat | 10.108.0.0/23 | 10.108.0.0 | 10.108.1.255 | 10.108.0.1 – 10.108.1.254 |
| A2 | Bidang Kurikulum | 10.108.2.0/24 | 10.108.2.0 | 10.108.2.255 | 10.108.2.1 – 10.108.2.254 |
| A3 | Bidang Guru & Tendik | 10.108.3.0/25 | 10.108.3.0 | 10.108.3.127 | 10.108.3.1 – 10.108.3.126 |
| A4 | Bidang Sarana Prasarana | 10.108.3.128/26 | 10.108.3.128 | 10.108.3.191 | 10.108.3.129 – 10.108.3.190 |
| A5 | Server & Admin | 10.108.3.192/28 | 10.108.3.192 | 10.108.3.207 | 10.108.3.193 – 10.108.3.206 |
| A7 | Interlink Kantor Pusat | 10.108.3.208/29 | 10.108.3.208 | 10.108.3.215 | 10.108.3.209 – 10.108.3.214 |
| A6 | Bidang Pengawas (Cabang) | 10.108.4.0/27 | 10.108.4.0 | 10.108.4.31 | 10.108.4.1 – 10.108.4.30 |
| A8 | Link WAN (P2P) | 10.108.4.32/30 | 10.108.4.32 | 10.108.4.35 | 10.108.4.33 – 10.108.4.34 |

### 3.2. Tabel Agregasi Rute (Supernetting)

Tabel ini menunjukkan proses penggabungan (agregasi) rute secara hierarkis, dari alokasi subnet individual hingga ke satu rute supernet tunggal.
![GrafikCIDR](https://github.com/user-attachments/assets/db7a521c-efcc-43e6-8e4b-750093664388)

| Level | Keterangan Penggabungan | Hasil Gabungan | Prefix | Netmask | Network Address | Broadcast Address |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Level 1 | Gabungkan subnet-subnet di 10.108.3.x (A3, A4, A5, A7) | 10.108.3.0/24 | /24 | 255.255.255.0 | 10.108.3.0 | 10.108.3.255 |
| Level 2 | Gabungkan hasil Level 1 (...3.0/24) dengan A2 (...2.0/24) | 10.108.2.0/23 | /23 | 255.255.254.0 | 10.108.2.0 | 10.108.3.255 |
| Level 3 (Blok B1) | Gabungkan hasil Level 2 (...2.0/23) dengan A1 (...0.0/23) | 10.108.0.0/22 | /22 | 255.255.252.0 | 10.108.0.0 | 10.108.3.255 |
| Level 4 (Blok B2) | Gabungkan semua subnet Cabang (A6, A8) di 10.108.4.x | 10.108.4.0/26 | /26 | 255.255.255.192 | 10.108.4.0 | 10.108.4.63 |
| Level 5 (Blok C1) | Gabungkan Blok B1 (...0.0/22) dan Blok B2 (...4.0/26) | 10.108.0.0/21 | /21 | 255.255.248.0 | 10.108.0.0 | 10.108.7.255 |
