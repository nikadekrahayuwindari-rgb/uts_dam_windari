# Proyek Ujian Tengah Semester (UTS) — Data Acquisition & Management
### Analisis Integrasi Data Sanggar Seni Tradisional, Iklim Historis, dan Wilayah Administratif di Kabupaten Badung & Provinsi Bali

---

## Identitas Mahasiswa
| Keterangan | Detail |
| :--- | :--- |
| **Nama Lengkap** | Ni Kadek Rahayu Windari |
| **NIM** | 2501010176 |
| **Program Studi / Kelas** | Informatika — Kelas F (2026) |
| **Institusi** | Institut Bisnis dan Teknologi Indonesia (INSTIKI) |
| **Mata Kuliah** | Data Acquisition & Management |
| **Email Akademik** | `2501010176@instiki.ac.id` |

---

## Latar Belakang & Tujuan Proyek
Pelestarian kesenian tradisional Bali—khususnya di Kabupaten Badung dan kabupaten/kota lainnya di Provinsi Bali—membutuhkan dukungan data yang terstruktur, mutakhir, dan terintegrasi lintas sumber. Proyek ini membangun *pipeline* akuisisi data otomatis menggunakan Python yang menggabungkan **tiga metode akuisisi utama** (unduhan dataset terbuka, konsumsi REST API, dan *web scraping* statis) dengan menerapkan tata kelola etika data (*data ethics*), validasi integritas berkas (*MD5 checksum*), serta pelacakan silsilah data (*data provenance*).[cite: 5]

---

## Arsitektur & Struktur Direktori
- `uts_dam_2501010176/README.md` — Dokumentasi utama proyek[cite: 5]
- `uts_dam_2501010176/data/raw/` — Data mentah asli tanpa modifikasi (`data_sanggar_seni_bali.csv`, `robots_wikipedia.txt`, `api_cuaca_*.json`, `wikipedia_kabupaten_badung.html`)[cite: 5]
- `uts_dam_2501010176/data/processed/` — Data bersih hasil normalisasi & integrasi (`sanggar_seni_bersih.csv`, `api_cuaca_bersih.csv`, `scraping_wikipedia_badung.csv`, `dataset_terintegrasi_sanggar_seni.csv`)[cite: 5]
- `uts_dam_2501010176/docs/` — Dokumentasi tata kelola & audit teknis (`data_ethics_checklist.csv`, `provenance.csv`, `endpoint_log.csv`)[cite: 5]

---

## Ringkasan Pipeline Akuisisi Data (Soal 1 – Soal 5)

| Tahap | Metode Akuisisi | Sumber Data & Lisensi | Volume & Hasil Utama |
| :--- | :--- | :--- | :--- |
| **Soal 1** | Unduhan Dataset Terstruktur (CSV) | Data Sanggar Seni & Kesenian Tradisional Bali (*Open Data / CC BY 4.0*) | **108 baris x 4 kolom** mencakup 6 jenis kesenian tradisional di 9 kabupaten/kota (2024–2025), lengkap dengan perhitungan estimasi penyimpanan tahunan (+30% cadangan). |
| **Soal 2** | Tata Kelola & Etika Data | `robots.txt` Wikimedia Foundation & Audit Privasi (PII) | Verifikasi otomatis `RobotFileParser.can_fetch()` menghasilkan status **`True`**, penerapan `User-Agent` resmi, jeda sopan `1.5 detik`, bebas PII, serta tersimpan dalam **9 butir checklist etika**. |
| **Soal 3** | REST API Ingestion | Open-Meteo Historical Weather Archive API (*CC BY 4.0*) | **531 baris observasi** suhu rata-rata harian dan curah hujan untuk 9 kabupaten/kota di Bali (Januari–Februari 2025) beserta penyimpanan seluruh respons JSON mentah. |
| **Soal 4** | Static Web Scraping | Wikipedia Bahasa Indonesia — *Kabupaten Badung* (*CC BY-SA 4.0*) | Ekstraksi tabel administratif dan wilayah menggunakan `BeautifulSoup` dari HTML statis dengan pembersihan referensi catatan kaki. |
| **Soal 5** | Integrasi Data & Provenance | Penggabungan Lintas Sumber (`pd.merge`) & Hashing MD5 | Penggabungan menggunakan kunci normalisasi `kunci_wilayah` (`validate="many_to_one"`) tanpa duplikasi baris, dilengkapi verifikasi integritas *hash* MD5 untuk seluruh berkas di `data/raw/`. |

---

## Jaminan Kualitas & Tata Kelola Data
- **Kepatuhan Etika & Sopan Santun Server:** Setiap permintaan HTTP menyertakan header `User-Agent` beridentitas mahasiswa, *timeout* 15 detik, jeda 1,5 detik antar-permintaan, serta mekanisme *exponential backoff* otomatis.
- **Integritas Data Mentah (*Raw Immutability*):** Seluruh berkas asli disimpan apa adanya di `data/raw/` dan dikunci keasliannya menggunakan tanda tangan digital **MD5 Checksum** pada `docs/provenance.csv`.
- **Validasi Skema Relasional:** Proses penggabungan akhir menjamin konsistensi 108 baris data utama tanpa kehilangan maupun penggandaan entitas wilayah.

---

## Cara Menjalankan Proyek
1. Buka berkas **`UTS_DAM_2501010176.ipynb`** di Google Colab atau Jupyter Notebook.
2. Pastikan pustaka `requests`, `pandas`, dan `beautifulsoup4` tersedia (sudah terpasang secara bawaan di Google Colab).
3. Jalankan seluruh sel secara berurutan (**Runtime -> Run all**) untuk mereproduksi folder `uts_dam_2501010176/` beserta seluruh berkas keluaran dan arsip ZIP secara otomatis.