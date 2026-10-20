# Studi Kasus: Dari Data Berantakan ke Keputusan, Analitik 4 Tingkat pada PT Maju Bersama

Berdasarkan kuliah tamu **"Make Sense of Data with Analysis and AI"** (*Informatics Practitioner Lecture Series 2026*)
oleh **Andria Arisal** (Research Center for Data and Information Sciences, BRIN). Pesan utamanya: data di lapangan jauh lebih
berantakan daripada data latihan, sehingga **penyiapan data** adalah tahap paling krusial. AI membantu mempercepat kerja,
tetapi verifikasi kritis dan keputusan akhir tetap di tangan manusia.

**Kasus:** PT Maju Bersama (perusahaan fiktif) mencatat pendapatan naik, tetapi laba bersih turun dan piutang membengkak.
Mengapa, dan apa yang harus dilakukan? Seluruh data adalah **simulasi** (sintetis), bukan data perusahaan nyata.

## Hubungan dengan materi

| Poin materi | Implementasi |
|---|---|
| Data lapangan berantakan | `data/data_mentah.csv` sengaja berisi duplikat, 3 format tanggal, nama pelanggan tidak konsisten, harga bertipe teks, nilai kosong, dan anomali |
| Data Preparation paling krusial | Fungsi `bersihkan()` mencatat setiap temuan dan tindakan; hasilnya tampil di dashboard |
| Kerangka CRISP-DM | Skrip disusun dalam 6 tahap (lihat docstring `pipeline_analitik.py`) |
| Analisis deskriptif | Tren pendapatan, laba, piutang, DSO, dan umur piutang (*aging*) |
| Analisis diagnostik | Dekomposisi perubahan laba, margin per produk, diskon dan kedisiplinan bayar per segmen |
| Analisis prediktif | Prediksi invoice yang akan telat bayar; 4 model dibandingkan berdampingan (konsep seperti KNIME) |
| Analisis preskriptif | Prioritas penagihan, pelanggan yang perlu perhatian, simulasi kebijakan diskon dan penagihan |
| Manusia memutuskan | Anomali tidak dibuang otomatis, melainkan masuk **antrean verifikasi manusia** |

## Struktur repositori

```
.
├── .github/workflows/update.yml   # menjalankan pipeline di GitHub Actions
├── data/data_mentah.csv           # data mentah (dibuat oleh pipeline)
├── docs/
│   ├── index.html                 # dashboard (GitHub Pages)
│   ├── data_bersih.csv            # data setelah dibersihkan
│   └── prediksi_invoice.csv       # skor risiko telat per invoice terbuka
├── pipeline_analitik.py
├── requirements.txt
└── README.md
```

## Menjalankan

```bash
pip install -r requirements.txt
python pipeline_analitik.py
```

Lalu buka `docs/index.html`. Hasil selalu sama karena simulasi memakai *seed* tetap.

## Mengunggah ke GitHub

1. Buat repositori kosong di GitHub, lalu dari folder ini jalankan:
   ```bash
   git init
   git add .
   git commit -m "Studi kasus analitik PT Maju Bersama"
   git branch -M main
   git remote add origin https://github.com/<username>/<nama-repo>.git
   git push -u origin main
   ```
2. Aktifkan dashboard: **Settings → Pages → Build and deployment → Deploy from a branch → `main` / folder `/docs`**.
3. Dashboard tersedia di `https://<username>.github.io/<nama-repo>/` (tulis tautannya di sini setelah aktif).
4. Agar GitHub Actions boleh menyimpan hasil: **Settings → Actions → General → Workflow permissions → Read and write permissions**.

## Pengembangan lanjutan

Memakai data transaksi nyata dari ERP lewat API, menambah fitur prediktif (riwayat komunikasi penagihan), menyimpan riwayat skor ke basis data,
dan memantau pergeseran perilaku bayar pelanggan dari waktu ke waktu.
