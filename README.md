# Blackpill STM32F411CEU6 V5 Baseboard Design

![3D Render KiCad](./assets/3d_render.png) <!-- Ekspor foto 3D Viewer KiCad & upload di sini -->

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
*(Tambahkan gambar tangkapan layar skematik dan layout PCB di sini)*

## 🚀 Pengujian & Manufaktur
- [x] Design Rules Check (DRC) KiCad: **0 Errors / 0 Warnings**
- [x] Berkas Gerber & Drill file terverifikasi di Gerber Viewer
