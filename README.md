# Tugas: PCB Baseboard untuk WeAct STM32F411CEU6 (Black Pill)

Membuat PCB baseboard 2 layer untuk modul WeAct Black Pill STM32F411CEU6.  Skematik sudah disediakan dan tidak boleh diubah. Yang dikerjakan adalah penempatan komponen, penggambaran outline papan, routing, dan penyiapan berkas fabrikasi.



---

## 1. Berkas yang diberikan

| Berkas | Isi |
|---|---|
| `Blackpill STM32F411CEU6-V5.kicad_pro` | project KiCad 10 |
| `Blackpill STM32F411CEU6-V5.kicad_sch` | skematik lengkap — **jangan diubah** |
| `Blackpill STM32F411CEU6-V5.kicad_pcb` | 70 footprint sudah ditempatkan, belum dirouting |
| `libraries/` | symbol, footprint, dan model 3D khusus project ini |
| `fp-lib-table` | daftar library footprint khusus project |
| `sym-lib-table` | daftar library symbol khusus project |

### Library tidak perlu di-install

Project ini sudah **self-contained**. Tidak ada library yang perlu dipasang,
tidak perlu menyentuh menu Preferences. Cukup extract zip-nya lalu buka file
`.kicad_pro` — semua symbol, footprint, dan model 3D langsung terbaca.

Yang membuatnya bisa begitu adalah dua berkas `fp-lib-table` dan
`sym-lib-table` di folder project. Keduanya menunjuk ke folder `libraries/`
memakai path relatif `${KIPRJMOD}` (artinya "folder project ini"), bukan path
absolut seperti `C:\Users\...`. Jadi project tetap jalan di komputer mana pun.

Library yang dipakai terbagi dua:

| Sumber | Contoh | Keterangan |
|---|---|---|
| Bawaan KiCad | `Resistor_SMD`, `LED_SMD`, `Connector_JST`, `MountingHole` | sudah ada di setiap instalasi KiCad |
| Folder `libraries/` | `STM32_Module`, `SSD1306`, `Potentiometer_PTH` | khusus project ini, ikut di dalam zip |

**Empat hal yang membuat library hilang — hindari:**

1. Memindah, mengganti nama, atau tidak ikut mengcopy folder `libraries/`
2. Menghapus `fp-lib-table` atau `sym-lib-table` dari folder project
3. Memindahkan file `.kicad_pcb` / `.kicad_sch` keluar dari folder project
4. Mengcopy sebagian isi project saja — **pindahkan selalu satu folder utuh**

Kalau komponen terlihat hilang saat project dibuka, periksa dulu apakah kedua
berkas `*-lib-table` dan folder `libraries/` masih berada satu folder dengan
file `.kicad_pro`.

## 2. Langkah pertama — wajib

Berkas `.kicad_pcb` yang diberikan **belum memuat netlist**. Kalau langsung dirouting, tidak akan ada ratsnest sama sekali.

Buka Pcbnew → **Tools → Update PCB from Schematic** (`F8`) → centang semua → **Update PCB**. Setelah itu ratsnest (garis putih antar pad) baru muncul.

## 3. Isi rangkaian

70 komponen, 68 net. Blok fungsinya:

| Blok | Komponen | Net |
|---|---|---|
| Modul MCU | `U1` WeAct Black Pill STM32F411CEU6 | — |
| Display | `Brd1` OLED 0.96" 128x64 I2C | `SDA`, `SCL` |
| Bar LED | `D1`–`D6` | `LED1`–`LED6` |
| LED status | `D7`–`D9` merah / hijau / kuning | `LED_RED`, `LED_GREEN`, `LED_YELLOW` |
| Tombol | `SW1`–`SW4` + debounce `C1`–`C4` | `BTN1`–`BTN4` |
| Potensiometer | `RV1`, `RV2` Bourns 3386P 10K | `POT1`, `POT2` |
| Buzzer | `BZ1` (digerakkan `Q6`) | `BUZZER` |
| Level shifter / driver | `Q1`–`Q10` BSS138 | — |
| Pasif | `R1`–`R30`, `C1`–`C4` | — |
| Lubang mounting | `H1`–`H4` M3 | — |

Konektor:

| Konektor | Tipe | Fungsi | Pinout |
|---|---|---|---|
| `J1`, `J2` | JST PH 2.0 4-pin | port I2C tambahan | 1=GND, 2=+3V3, 3=SDA, 4=SCL |
| `J3` | JST PH 2.0 6-pin | port SPI | 1=GND, 2=+3V3, 3=MOSI, 4=MISO, 5=SCK, 6=SS |
| `J4` | Pin socket 1x05 2.54mm | port debug / UART | 1=GND, 2=RX2, 3=TX2, 4=NRST, 5=SWO |

Net daya: `GND` (36 pin), `+3.3V` (13 pin), `+5V` (11 pin).

## 4. Spesifikasi papan

- 2 layer (double layer)
- Warna PCB hitam
- Semua komponen memakai footprint yang sudah ditentukan di skematik —
  jangan diganti
- Outline papan (`Edge.Cuts`) digambar sendiri, ukuran menyesuaikan tata
  letak. Beri sudut membulat (rounded corner) agar rapi
- 4 lubang spacer M3 di keempat pojok papan — gunakan `H1`–`H4`
- 4 lubang spacer M2 untuk modul OLED (lihat bagian 4.1)
- Ikuti ketentuan fabrikasi JLCPCB (lebar jalur, clearance, annular ring,
  jarak ke tepi papan)

### 4.1 Lubang spacer M3 di pojok papan

`H1`–`H4` sudah ada di skematik dengan footprint `MountingHole:MountingHole_3mm`.
Tempatkan keempatnya di **pojok papan**, jaga jaraknya seragam dari tepi
(saran 4–5 mm dari tiap sisi) sehingga membentuk persegi panjang yang rapi.

### 4.2 Lubang spacer M2 untuk modul OLED

Footprint `SSD1306:128x64OLED` hanya berisi 4 pad header, **tanpa lubang
mounting**. Kalau tidak ditambahkan, modul OLED hanya bertumpu pada 4 pin
header saja dan gampang goyang atau patah. Tambahkan sendiri 4 lubang
spacer-nya.

Posisi lubang relatif terhadap titik nol footprint `Brd1`:

| Lubang | X (mm) | Y (mm) |
|---|---|---|
| kiri atas | −10.81 | −10.10 |
| kiri bawah | −10.81 | +11.90 |
| kanan atas | +11.19 | −10.10 |
| kanan bawah | +11.19 | +11.90 |

Pola lubangnya **22.0 x 22.0 mm**, titik tengahnya di (0.19, 0.90) dari titik
nol `Brd1`. Pakai spacer **M2**, jadi lubangnya Ø2.2 mm.

Cara menambahkannya: di Pcbnew, tempatkan 4 footprint
`MountingHole:MountingHole_2.2mm_M2` lewat **Place → Add Footprint**, lalu isi
koordinatnya lewat Properties (`E`) sesuai tabel di atas setelah `Brd1`
diletakkan pada posisi finalnya.

> **Perhatian:** lubang ini tidak ada di skematik. Saat menjalankan
> **Update PCB from Schematic**, **jangan centang** opsi
> *"Delete footprints with no symbols"* — kalau tercentang, keempat lubang ini
> akan terhapus.

## 5. Aturan penempatan

Penempatan dinilai, bukan sekadar "yang penting nyambung".

- Area di depan dan di belakang modul Black Pill harus kosong. Sisi depan untuk kabel USB, sisi belakang untuk kabel jumper ke ST-Link. Jangan
  letakkan komponen apa pun di kedua area itu. 
- Konektor USB modul harus di pinggir papan supaya kabel bisa ditancapkan.
- Port debug `J4` diletakkan di samping (kiri atau kanan) posisi SWD debugger.
- `D1`–`D6` harus sejajar lurus berurutan LED1 → LED6 sebagai bar LED.
- `D7`, `D8`, `D9` (merah, hijau, kuning) dikelompokkan dan diletakkan berdekatan.
- Tempatkan `C1`–`C4` sedekat mungkin dengan tombol pasangannya (fungsinya debounce).
- `RV1`, `RV2` dan `J1`–`J3` diletakkan di tepi papan agar mudah diakses.

## 6. Routing

- Selesaikan **seluruh net** — tidak boleh ada ratsnest tersisa.
- **Jalur daya (`+3.3V`, `+5V`) dibuat lebih lebar** dari jalur sinyal.
  Sinyal minimal 0.3 mm, jalur daya 0.5 mm atau lebih.
- `SDA` dan `SCL` dirutekan berdampingan dan sependek mungkin.
- `MOSI`, `MISO`, `SCK`, `SS` diusahakan panjangnya seimbang.
- Hindari sudut 90° pada jalur — gunakan 45°.
- Beri via secukupnya; jangan menaruh via di atas pad SMD.

### 6.1 Copper pour GND — wajib

**Seluruh area tembaga yang tidak terpakai wajib diisi copper pour yang
terhubung ke net `GND`**, di kedua layer. Tidak boleh ada area tembaga yang
dibiarkan kosong begitu saja, dan tidak boleh ada tembaga mengambang (tidak
terhubung ke net mana pun).

Alasannya:

- `GND` adalah net terbesar di rangkaian ini (**36 pin**). Menariknya satu per
  satu sebagai jalur biasa tidak praktis dan hasilnya buruk.
- Arus balik selalu mengalir di jalur berimpedansi terendah, yaitu tepat di
  bawah jalur sinyalnya. Dengan bidang ground utuh, loop arusnya kecil
  sehingga noise dan EMI rendah.
- Distribusi tembaga yang merata membuat proses etsa lebih seragam. Area
  kosong yang luas menyebabkan jalur di sekitarnya ter-etsa berlebih dan
  menipis.
- Tembaga yang seimbang antara layer atas dan bawah mencegah papan melengkung.

Tembaga **mengambang lebih berbahaya daripada tidak ada tembaga** — ia
berperilaku seperti antena. Karena itu opsi *remove islands* wajib aktif.

**Cara membuatnya:** pilih layer, klik **Add Filled Zone**, gambar area
mengikuti outline papan, lalu pada Zone Properties isi net **`GND`** dan
setelan berikut:

| Setelan | Nilai |
|---|---|
| Clearance | 0.3 mm |
| Minimum width | 0.25 mm |
| Pad connection | **Thermal relief** |
| Thermal relief gap | 0.3 mm |
| Thermal spoke width | 0.5 mm |
| Remove islands | **Yes** |

Ulangi untuk layer satunya. Setelah selesai tekan **`B`** untuk mengisi semua
zone, lalu jalankan DRC lagi.

> **Pad connection wajib Thermal relief, jangan Solid.** Kalau pad tersambung
> penuh ke bidang tembaga, panas solder langsung terserap bidang dan pad
> menjadi sangat sulit disolder. Board ini banyak komponen THT — buzzer,
> potensiometer, JST, pin header, OLED — jadi setelan ini menentukan apakah
> papan bisa dirakit atau tidak.

### 6.2 Pembagian layer

Jadikan **`B.Cu` sebagai bidang ground yang seutuh mungkin**, dan rutekan
sinyal sebanyak mungkin di `F.Cu`.

Setiap jalur sinyal yang ditarik di layer bawah akan memotong bidang ground
dan memaksa arus balik memutar — persis yang ingin dihindari. Kalau terpaksa
menyeberang di layer bawah, buat sependek mungkin dan jangan sampai memotong
bidang ground menjadi terbelah.

`F.Cu` tetap diberi pour `GND` juga untuk mengisi sela-sela antar jalur.

## 7. Aturan desain (DRC)

Nilai yang sudah diatur di project:

| Parameter | Nilai |
|---|---|
| Clearance minimum | 0.3 mm |
| Lebar jalur minimum | 0.3 mm |
| Diameter lubang minimum | 0.3 mm |
| Diameter via minimum | 0.5 mm |
| Annular ring via minimum | 0.2 mm |
| Jarak lubang ke lubang | 0.45 mm |
| Tinggi teks silkscreen | 1.0 mm |
| Tebal garis teks silkscreen | 0.15 mm |

Netclass `Default` sudah disetel sesuai batas minimum di atas:

| Parameter | Nilai |
|---|---|
| Lebar jalur | 0.3 mm |
| Clearance | 0.3 mm |
| Diameter via | 0.8 mm |
| Drill via | 0.4 mm |

Nilai ini boleh dinaikkan (misalnya jalur daya dilebarkan lewat netclass
sendiri), tapi **jangan diturunkan** — di bawah nilai tersebut DRC akan
menolak.

**DRC harus bersih (0 error)** sebelum dikumpulkan.

## 8. Yang dikumpulkan

Dikumpulkan dalam **satu file zip** dengan nama `NIM_Nama.zip`.

### 8.1 File project KiCad lengkap

Cara paling aman: **zip seluruh folder project apa adanya**, jangan memilih
file satu per satu. Yang wajib ada:

| File | Keterangan |
|---|---|
| `*.kicad_pro` | file project |
| `*.kicad_sch` | skematik |
| `*.kicad_pcb` | layout PCB hasil pekerjaan |
| `fp-lib-table` | daftar library footprint project |
| `sym-lib-table` | daftar library symbol project |
| `libraries/` | seluruh folder, beserta isinya |

Kedua berkas `*-lib-table` dan folder `libraries/` **harus ikut disertakan**.
Tanpa salah satunya, footprint Black Pill, OLED, dan trimpot tidak akan
ditemukan dan layout terbuka dengan komponen hilang — pekerjaan yang sudah
selesai pun akan terlihat rusak saat diperiksa.

Jangan mengirim file `.kicad_prl`, `.lck`, atau folder backup — tidak perlu.

### 8.2 Gerber dan drill file siap kirim ke JLCPCB

Bukan sekadar hasil plot, tapi **berkas yang benar-benar siap diunggah** ke
JLCPCB dan sudah dibuktikan lolos.

**a. Export gerber** — Pcbnew → **File → Plot**, format **Gerber**, layer yang
dicentang:

`F.Cu`, `B.Cu`, `F.Mask`, `B.Mask`, `F.Silkscreen`, `B.Silkscreen`, `Edge.Cuts`

**b. Export drill** — pada dialog yang sama klik **Generate Drill Files**:

| Setelan | Nilai |
|---|---|
| Format | Excellon |
| Satuan | Milimeter |
| Drill origin | Absolute |
| Zeros format | Decimal format |

**c. Zip** seluruh hasilnya menjadi `gerber.zip`. Isinya file gerber + file
drill langsung di root zip, jangan dibungkus folder lagi.

**d. Verifikasi di JLCPCB** — unggah `gerber.zip` ke
**Gerber Viewer JLCPCB** (<https://jlcpcb.com/gerber-viewer>) atau halaman
order-nya. Pastikan:

- semua layer terbaca, tidak ada yang hilang atau tertukar
- bentuk papan terbaca benar dari `Edge.Cuts` (tidak terpotong / tidak kosong)
- seluruh lubang muncul di posisinya, termasuk 4 lubang M3 dan 4 lubang M2 OLED
- ukuran papan yang terdeteksi sesuai dengan outline yang digambar

**e. Screenshot hasil verifikasi** dengan nama `jlc-gerber.png`, harus terlihat
tampilan layer papan beserta ukuran yang terdeteksi.

#### Kesesuaian dengan aturan JLCPCB

Aturan DRC project ini sudah disetel **lebih ketat** dari batas minimum
JLCPCB, jadi selama DRC bersih, hasilnya aman difabrikasi:

| Parameter | Batas JLCPCB (1–2 layer, 1 oz) | Aturan tugas ini |
|---|---|---|
| Lebar jalur minimum | 0.10 mm | 0.3 mm |
| Clearance minimum | 0.10 mm | 0.3 mm |
| Diameter bor minimum | 0.15 mm | 0.3 mm |
| Diameter via (pad) minimum | 0.25 mm | 0.5 mm |
| Annular ring minimum | 0.18 mm (disarankan ≥0.25) | 0.2 mm |
| Jarak lubang ke lubang | 0.45 mm | 0.45 mm |
| Tinggi teks silkscreen | 1.0 mm | 1.0 mm |
| Tebal garis teks silkscreen | 0.15 mm | 0.15 mm |

Sumber: [JLCPCB PCB Capabilities](https://jlcpcb.com/capabilities/pcb-capabilities).

Hal berikut tetap harus diperhatikan sendiri karena tidak sepenuhnya dicek DRC:

- **Jarak tembaga ke tepi papan** minimal 0.2 mm menurut JLCPCB; pakai
  0.5 mm supaya aman
- **Warna solder mask hitam** menuntut jarak antar pad minimal 0.13 mm — lebih
  ketat dari warna hijau. Pastikan tidak ada pad yang berdempetan.
- **Silkscreen putih di atas hitam**: pastikan semua designator masih terbaca
  dan tidak menimpa pad. Warning `silk_over_copper` dan `silk_overlap` di DRC
  memang bukan error, tapi rapikan sebisanya dengan menggeser designator.
- **Ukuran papan** memengaruhi biaya. Usahakan tata letak seringkas mungkin —
  jangan ada area kosong yang tidak terpakai — supaya papannya tidak mahal.
  Tetap jaga kerapian dan aturan penempatan di bagian 5, jangan sampai
  komponen dipaksa berdesakan.

### 8.3 Gambar 3D Viewer dari berbagai sisi

Untuk verifikasi penempatan komponen dan tinggi modul. Minimal **6 gambar**:

| File | Tampilan |
|---|---|
| `3d-top.png` | atas |
| `3d-bottom.png` | bawah |
| `3d-front.png` | depan |
| `3d-back.png` | belakang |
| `3d-left.png` | kiri |
| `3d-right.png` | kanan |
| `3d-iso.png` | isometrik / menyudut (opsional tapi dianjurkan) |

Cara mengambilnya: buka **3D Viewer** (`Alt+3`) → pilih sudut lewat menu
**View → Top / Bottom / Front / Back / Left / Right** → simpan lewat
**File → Export Current View as PNG…**

Pastikan seluruh papan masuk ke dalam gambar dan komponen tampil dengan model
3D-nya, bukan kotak kosong.

### 8.4 Bukti DRC bersih

Screenshot jendela DRC yang menunjukkan **0 error**, beri nama `drc.png`.

### 8.5 Laporan dalam bentuk PPT

File presentasi (`.pptx` atau `.pdf`) dengan nama `NIM_Nama_Laporan.pptx`.
Isinya menjelaskan proses pengerjaan, bukan sekadar kumpulan screenshot.

Susunan minimal:

| Slide | Isi |
|---|---|
| 1 | Judul, nama, NIM, kelas |
| 2 | Gambaran rangkaian: fungsi tiap blok (MCU, OLED, LED, tombol, potensiometer, buzzer, konektor) |
| 3 | Strategi penempatan — **alasan** kenapa komponen ditaruh di posisi itu, kaitkan dengan aturan di bagian 5 |
| 4 | Strategi routing — pembagian layer, lebar jalur daya vs sinyal, penggunaan ground pour |
| 5 | Hasil layout 2D (tampak F.Cu dan B.Cu) |
| 6 | Hasil 3D dari beberapa sisi |
| 7 | Bukti DRC 0 error dan hasil verifikasi gerber di JLCPCB |
| 8 | Kendala yang dihadapi dan cara mengatasinya |
| 9 | Kesimpulan |

Yang dinilai terutama **slide 3, 4, dan 8** — di situ terlihat apakah
mahasiswa memahami pekerjaannya atau sekadar menarik jalur sampai tersambung.
Uraikan dengan kalimat sendiri.

## 9. Penilaian

| Aspek | Bobot |
|---|---|
| Routing selesai seluruhnya, DRC bersih | 25% |
| Kepatuhan aturan penempatan (bagian 5) | 20% |
| Kualitas routing: lebar jalur daya, ground pour, kerapian | 20% |
| Outline papan, lubang mounting, gerber lolos verifikasi JLCPCB | 15% |
| Laporan PPT (terutama slide penjelasan strategi dan kendala) | 15% |
| Kelengkapan berkas yang dikumpulkan | 5% |

## 10. Referensi board versi lama

![Board versi lama](./assets/image-20260920171551141.png)

> **Gunakan foto ini sebatas acuan letak konektor dan port saja — jangan
> ditiru tata letaknya.** Desain baru sudah banyak berubah.

Yang boleh diambil dari foto ini hanya **idenya**: semua konektor dan port menghadap ke tepi papan sehingga kabel bisa ditancapkan tanpa terhalang,
konektor dikelompokkan di satu sisi, dan area di sekeliling konektor USB modul dibiarkan kosong.

**Jangan menyalin** posisi komponen, ukuran papan, bentuk outline, maupun susunan LED dan tombolnya. Kerjakan penempatan sendiri mengikuti aturan di bagian 5.

Beberapa perbedaan yang perlu diperhatikan:

| | Board lama (foto) | Board baru (tugas ini) |
|---|---|---|
| Konektor | empat konektor JST dikelompokkan di sisi kiri | `J1`, `J2` JST PH 4-pin + `J3` JST PH 6-pin + `J4` pin socket 1x05 |
| Port debug | menyatu dengan kelompok konektor | `J4` terpisah, diletakkan di samping posisi SWD debugger |
| Bar LED | kolom vertikal di sisi kanan | bebas, asalkan `D1`–`D6` sejajar lurus berurutan |
| Outline | persegi panjang | digambar sendiri, disarankan bersudut membulat |

Yang **tidak** berubah: penetapan pin STM32-nya sama persis dengan yang tertulis di silkscreen board lama — bar LED di `PA9`, `PA8`, `PB15`, `PB14`,
`PB13`, `PB12`; LED status di `PA15` (merah), `PA10` (kuning), `PB10` (hijau); tombol di `PB4`, `PB5`, `PB8`, `PB9`; potensiometer di `PA1` dan `PB0`; buzzer di `PB2`. Jadi foto ini tetap berguna untuk memahami fungsi tiap bagian, bukan untuk menyalin tata letaknya.
