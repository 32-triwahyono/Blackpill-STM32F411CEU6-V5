# Blackpill STM32F411CEU6 V5 Baseboard Design

<img width="750" height="409" alt="LAYOUT PCB" src="https://github.com/user-attachments/assets/7ec1422d-31ae-4948-96fe-3c41487537c4" />


## 📌 Deskripsi Proyek
Desain modul/baseboard PCB untuk mikrokontroler STM32F411CEU6 (Blackpill Core Board) menggunakan **KiCad**. Proyek ini mencakup perancangan skematik, tata letak jalur PCB 2-layer, serta pembuatan berkas manufaktur (Gerber).

## 🛠️ Spesifikasi Hardware & PCB
- **Microcontroller:** STM32F411CEU6 (ARM Cortex-M4 32-bit)
- **PCB Layers:** 2-Layer Board (Top & Bottom Ground Plane)
- **Substrat & Ketebalan:** FR-4, 1.6 mm, 1 oz Copper
- **Antarmuka & Port:** Header ekspresi GPIO, Port SWD Debug, Power Indicator LED
- **Desain software:** KiCad EDA

## 📁 Struktur Repositori
- `*.kicad_sch` : Berkas skematik elektronik
- `*.kicad_pcb` : Berkas tata letak (layout) PCB
- `*.kicad_pro` : Berkas proyek utama KiCad
- `gerber.zip`   : Berkas manufaktur siap cetak (JLCPCB / PCBWay)

## 📸 Skematik & Layout PCB
<img width="811" height="315" alt="DEPAN BELAKANG" src="https://github.com/user-attachments/assets/4fb4c306-c56d-4b50-a84b-9e0738f14b86" />


## 🚀 Pengujian & Manufaktur
- [x] Design Rules Check (DRC) KiCad: **0 Errors / 0 Warnings**
- [x] Berkas Gerber & Drill file terverifikasi di Gerber Viewer
