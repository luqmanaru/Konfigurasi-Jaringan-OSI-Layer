# Konfigurasi-Jaringan-OSI-Layer

![Cisco](https://img.shields.io/badge/Cisco-Networking-blue?style=for-the-badge&logo=cisco&logoColor=white)
![VLAN](https://img.shields.io/badge/VLAN-Segmentation-green?style=for-the-badge)
![OSI](https://img.shields.io/badge/OSI-Layer-orange?style=for-the-badge)

---

## 📝 Deskripsi
Repository ini berisi **solusi lengkap UAS Jaringan Komputer Terapan 1** meliputi:
- Implementasi VLAN untuk 2 Auditorium
- Topologi jaringan dengan 2 segmentasi subnet (/24 dan /25)
- Analisis keamanan jaringan (Inbound/Outbound)
- Penjelasan mendalam tentang OSI Layer

---

## 📋 Soal UAS & Solusi
| No | Soal | Teknologi | Status |
|----|------|-----------|--------|
| 1 | Keamanan Jaringan | Inbound/Outbound | ✅ Selesai |
| 2 | VLAN 2 Auditorium | VLAN, Router-on-a-Stick | ✅ Selesai |
| 3 | OSI Layer | Model Referensi Jaringan | ✅ Selesai |
| 4 | Subnetting /24 dan /25 | IP Subnetting | ✅ Selesai |

---

## 🔧 Solusi Soal 1: Keamanan Jaringan
### Peran Inbound dan Outbound
| Jenis | Fungsi | Contoh Implementasi |
|-------|--------|---------------------|
| **Inbound** | Mengatur paket data yang masuk ke jaringan | - Memblokir IP mencurigakan<br>- Membatasi port yang bisa diakses<br>- Mencegah serangan hacking/malware |
| **Outbound** | Mengontrol paket data yang keluar dari jaringan | - Memblokir akses ke situs berbahaya<br>- Mencegah kebocoran data sensitif<br>- Menghindari aktivitas tidak sah |

**Inti**: Inbound menjaga dari ancaman luar, Outbound melindungi data internal.

---

## 🔧 Solusi Soal 2: VLAN 2 Auditorium
### Topologi Jaringan
| Perangkat | VLAN | IP Address | Subnet | Peran |
|-----------|------|------------|--------|-------|
| **SW-1** | 10, 20 | - | - | Switch Access |
| **R1** | - | Fa0/0.10: 192.168.10.1/24<br>Fa0/0.20: 192.168.20.1/24 | /24 | Router-on-a-Stick |
| **Auditorium A** | 10 | DHCP | 192.168.10.0/24 | User VLAN |
| **Auditorium B** | 20 | DHCP | 192.168.20.0/24 | User VLAN |

### Konfigurasi Switch
```cisco
enable
configure terminal
hostname SW-1
vlan 10
name AUDITORIUM_A
exit
vlan 20
name AUDITORIUM_B
exit
interface range fa0/2-10
switchport mode access
switchport access vlan 10
exit
interface range fa0/11-24
switchport mode access
switchport access vlan 20
exit
interface fa0/1
switchport mode trunk
end
```

### Konfigurasi Router
```cisco
enable
configure terminal
hostname R1
interface fa0/0
no shutdown
interface fa0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
exit
interface fa0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
end
```

---

## 🔧 Solusi Soal 3: OSI Layer
### Proses Transaksi Data Client-Server
| Layer | Fungsi | Contoh Implementasi |
|-------|--------|---------------------|
| **7. Application** | Antarmuka user aplikasi | HTTP, FTP, DNS |
| **6. Presentation** | Enkripsi, kompresi, format data | SSL/TLS, JPEG |
| **5. Session** | Manajemen sesi komunikasi | API, NetBIOS |
| **4. Transport** | Pengiriman data end-to-end | TCP, UDP |
| **3. Network** | Routing dan pengalamatan IP | IP, Router |
| **2. Data Link** | Pengiriman frame antar node | MAC Address, Switch |
| **1. Physical** | Transmisi bit fisik | Kabel, Wi-Fi |

**Alur Transaksi**:
1. Client (Layer 7) → Permintaan data
2. Layer 6-1 → Enkripsi, segmentasi, pengalamatan
3. Jaringan fisik → Transmisi bit
4. Server → Proses kebalikan (Layer 1-7)
5. Server → Respons ke client

---

## 🔧 Solusi Soal 4: Subnetting /24 dan /25
### Topologi Jaringan
| Perangkat | Jaringan | IP Address | Subnet | Host |
|-----------|----------|------------|--------|------|
| **R-1841** | A & B | Fa0/0: 192.168.1.1<br>Fa0/1: 192.168.2.1 | /24<br>/25 | - |
| **SW1-2950** | A | - | /24 | 5 PC |
| **SW2-2950** | B | - | /25 | 5 PC |
| **PC A1-A5** | A | DHCP | 192.168.1.0/24 | 254 host |
| **PC B1-B5** | B | DHCP | 192.168.2.0/25 | 126 host |

### Konfigurasi Router
```cisco
enable
configure terminal
hostname R-1841
interface FastEthernet0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
interface FastEthernet0/1
ip address 192.168.2.1 255.255.255.128
no shutdown
end
```

### Konfigurasi Switch 1
```cisco
enable
configure terminal
hostname SW1-2950
interface range fa0/1-24
no shutdown
exit
```

### Konfigurasi Switch 2
```cisco
enable
configure terminal
hostname SW2-2950
interface range fa0/1-24
no shutdown
exit
```

---

## 📊 Output Konfigurasi
### Verifikasi VLAN (Soal 2)
```
SW-1# show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1
10   AUDITORIUM_A                     active    Fa0/2, Fa0/3, Fa0/4, Fa0/5
                                                Fa0/6, Fa0/7, Fa0/8, Fa0/9
                                                Fa0/10
20   AUDITORIUM_B                     active    Fa0/11, Fa0/12, Fa0/13, Fa0/14
                                                Fa0/15, Fa0/16, Fa0/17, Fa0/18
                                                Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                Fa0/23, Fa0/24
```

### Verifikasi IP Router (Soal 4)
```
R-1841# show ip interface brief

Interface              IP-Address      OK? Method Status                Protocol
FastEthernet0/0        192.168.1.1     YES manual up                    up
FastEthernet0/1        192.168.2.1     YES manual up                    up
```

## Topologi
<img width="827" height="299" alt="image" src="https://github.com/user-attachments/assets/0d48e294-4e40-495a-9f28-511e602436d7" />

---
**luqmanaru**  
