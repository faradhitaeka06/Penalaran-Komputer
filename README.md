# 🧠 Penalaran Komputer: Pidana Klasifikasi TUN di Pengadilan PTUN BANDUNG

Proyek ini mengembangkan sistem **penalaran berbasis kasus (Case-Based Reasoning)** untuk mendukung pencarian dan analisis putusan hukum dalam ranah **Pidana Klasifikasi TUN di Pengadilan PTUN BANDUNG**. Sistem ini memungkinkan pengguna mencari kasus serupa berdasarkan ringkasan fakta dan mengevaluasi performa retrieval.

---

## ⚙️ Teknologi yang Digunakan

* Python 3.10+
* pandas, scikit-learn
* BeautifulSoup, pdfminer / pdftotext
* Sentence-Transformers (BERT Embedding)
* TF-IDF + SVM
* Google Colab / Jupyter Notebook

---

## Langkah-Langkah Proyek

### ✅ Tahap 1: Membangun Case Base

* **Seleksi & Unduh Dokumen:** mendapatkan 140 Data dari Direktori MA RI.
* **Konversi & Ekstraksi Teks:** PDF ➔ teks biasa menggunakan pdfminer atau HTML dengan BeautifulSoup.
* **Pembersihan:** Hapus header, footer, nomor halaman, dan normalisasi teks.
* **Output:** /data/raw/*.txt dan cleaning.log 

### ✅ Tahap 2: Case Representation

* **Ekstraksi Metadata:** Nomor Perkara, Tanggal, Pasal, Pihak, dll.
* **Ekstraksi Konten Kunci:** Ringkasan Fakta dan Argumen Hukum.
* **Feature Engineering:** Panjang teks (jumlah kata), Bag-of-Words (kata kunci), Estimasi QA-pairs.
* **Output:** /data/processed/cases.csv

### ✅ Tahap 3: Case Retrieval

* **Representasi Vektor:**
  * TF-IDF menggunakan TfidfVectorizer
  * BERT Embedding menggunakan paraphrase-multilingual-MiniLM-L12-v2
* **Splitting Dataset:** Train/Test dengan rasio 80:20.
* **Model Retrieval:**
  * TF-IDF + SVM
  * BERT Similarity
* **Pengujian Awal:** Menyiapkan 10 query uji beserta ground-truth case_id. 
* **Output:** Simpan 10 query uji di /data/eval/queries.json

### ✅ Tahap 4: Solution Reuse

* **Ekstrak Solusi:** Ambil ringkasan_fakta dari retrieved_case_id berdasarkan hasil Tahap 3.
* **Algoritma Prediksi:**
  * BERT: Ambil solusi dengan Majority Vote dari Top-k retrieval.
  * SVM: Gunakan solusi dari 1 kasus dengan skor tertinggi (Top-1).
* **Demo Manual:** Verifikasi hasil reuse dengan membandingkan solusi_prediksi terhadap solusi_asli untuk beberapa contoh query, dengan menggunkan 10 contoh/ query
* **Output:** Simpan hasil ke /data/results/predictions.csv dalam format: query_id, predicted_solution , top_5_case_id

### ✅ Tahap 5: Model Evaluation

* **Evaluasi Retrieval:** Accuracy, Precision, Recall, F1-score.
* **Visualisasi:**
  * Tabel Metrik per Model (TF-IDF + SVM vs. BERT)
  * Plot Bar Chart Performance
  * Diskusi Kasus Gagal Prediksi (Error Analysis)

---

## 🚀 Petunjuk Instalasi & Eksekusi

### 1. Clone Repository

bash
git clone https://github.com/faradhitaeka06/Penalaran-Komputer.git


### 2. Install Dependensi

Buat dan jalankan `requirements.txt`:

```bash
pip install -r requirements.txt
```

Isi `requirements.txt`:

```txt
pandas
scikit-learn
beautifulsoup4
pdfminer.six
sentence-transformers
```

Gunakan Google Colab **(disarankan)** atau Jupyter Notebook:

python
!pip install pandas scikit-learn beautifulsoup4 sentence-transformers


### 3. Jalankan End-to-End Pipeline

Buka dan jalankan notebook berikut secara berurutan:

1. `Tahap 1 Membangun Case Base.ipynb` : Konversi PDF ke teks dan pembersihan data mentah.
2. `Tahap 2 Case Representation.ipynb` : Ekstraksi metadata, fitur, dan representasi kasus.
3. `Tahap 3 Case Retrieval.ipynb` : Vektorisasi (TF-IDF/BERT) dan pemanggilan kasus mirip.
4. `Tahap 4 Solution Reuse.ipynb` : Prediksi solusi dengan mengambil ringkasan dari kasus baru.
5. `Tahap 5 Model Evaluation.ipynb` : Evaluasi hasil prediksi dan visualisasi hasil.

---

## 📄 Hasil Akhir

| Model          | Accuracy | Precision | Recall  | F1-Score |
| -------------- | -------- | --------- | ------- | -------- |
| TF-IDF + SVM   | 80.00%   | 68.18%    | 72.73%  | 69.70%   |
| BERT Embedding | 80.00%  | 66.67%   | 66.67% | 66.67%  |

---

## 📄 Struktur Folder

Penalaran Komputer/

├── CSV/
│
│   ├── putusan_ma__2025-06-05.csv        ← Dataset hasil ekstraksi (CSV).
│

├── PDF/

│   ├── zaeb5100c5d0d2b2af02323235335332.pdf

│   ├── zaeb5100cbe5b2da80523232355335432.pdf  ← File putusan hukum asli (PDF) dengan nama acak/terenkripsi.

│   ├── ...

│   └── zaeb5589c4b3361c9fcf313732363136.pdf
│   

├── data/

│   ├── eval/                             ← Hasil eval.

│   ├── processed/                        ← Data yang sudah diproses.

│   ├── raw/                              ← Data mentah hasil konversi PDF/HTML

│   └── results/                          ← Output dari proses retrieval, prediksi, dsb
│

├── data_clean/

│   └── raw/                              ← Data hasil pembersihan awal
│

└── logs/

    └── cleaning.log                      ← Catatan proses cleaning (log)
    

---

## 🔧 Catatan Tambahan

* Pastikan seluruh path di notebook sudah disesuaikan dengan Google Drive atau local.
* Proyek ini hanya mencakup domain **Pidana Klasifikasi TUN di Pengadilan PTUN BANDUNG**.
* Idealnya digunakan untuk pembelajaran, penelitian, atau pembuatan sistem hukum berbasis AI.

---

📄 **Author:** Faradhita Eka Septiana
