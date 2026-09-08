# 🤖 Customer Churn Prediction — Modeling

Project lanjutan dari [Telco Customer Churn — Exploratory Data Analysis]([[../telco-churn-eda](https://github.com/BimantoroM/Telco-Customer-Churn-Exploratory-Data-Analysis)](https://github.com/BimantoroM/Telco-Customer-Churn-Exploratory-Data-Analysis)). Kalau project sebelumnya fokus **menemukan pola** pendorong churn, project ini melangkah lebih jauh: membangun **model klasifikasi** untuk memprediksi pelanggan mana yang berisiko churn, lengkap dengan validasi statistik atas hasilnya.

> 🔗 **Project sebelumnya:** [Telco Customer Churn — EDA]([../telco-churn-eda](https://github.com/BimantoroM/Telco-Customer-Churn-Exploratory-Data-Analysis)) — sumber insight & feature engineering yang dipakai ulang di project ini.

---

## 📌 Latar Belakang

Project EDA sebelumnya menemukan pola-pola jelas di balik churn (jenis kontrak, metode pembayaran, tenure, layanan tambahan). Pertanyaannya: **bisakah pola-pola ini dipakai untuk memprediksi**, bukan cuma menjelaskan, pelanggan mana yang akan churn — sebelum mereka benar-benar pergi?

---

## 🗂️ Dataset

Sama persis dengan project EDA — 7.043 pelanggan, 33 kolom awal — supaya kedua project bisa dibandingkan *apple-to-apple*.

---

## 🛠️ Tools & Libraries

`Python` · `Pandas` · `NumPy` · `Scikit-learn` · `Statsmodels` · `Matplotlib` · `Seaborn`

---

## 🔍 Alur Kerja

1. **Replikasi Feature Engineering** dari project EDA (konsistensi antar project)
2. **Pencegahan Data Leakage** — beberapa kolom sengaja dibuang dari fitur model:
   | Kolom | Keputusan | Alasan |
   |---|---|---|
   | `Churn Score` | Dibuang | Skor risiko buatan sistem lain (r = 0,66 dgn target) — kalau dipakai, model cuma menebak ulang |
   | `Churn Reason` | Dibuang | Hanya terisi untuk pelanggan yang **sudah** churn — bocor ke jawaban |
   | `CustomerID`, `City` | Dibuang | Tanpa nilai prediktif / kardinalitas terlalu tinggi untuk baseline |
   | `CLTV` | Dipakai | Korelasi lemah (r = -0,13) terhadap target, bukan proxy churn |
3. **Encoding & Split** — One-Hot Encoding, train-test split 80:20 (`stratify=y`)
4. **Model Building** — Logistic Regression & Random Forest (`class_weight='balanced'`)
5. **Evaluasi** — classification report, confusion matrix, ROC-AUC
6. **Validasi Robust** — 5-fold Stratified Cross-Validation + **McNemar's Test** untuk menguji signifikansi statistik perbedaan performa model
7. **Feature Importance** — dibandingkan silang dengan insight EDA

---

## 📊 Hasil Utama

| Metrik | Logistic Regression | Random Forest |
|---|---|---|
| Accuracy | 0,744 | **0,777** |
| Precision (Churn) | 0,511 | **0,562** |
| **Recall (Churn)** | **0,786** | 0,730 |
| F1-Score (Churn) | 0,620 | 0,635 |
| ROC-AUC | 0,849 | **0,851** |

**5-Fold CV (Recall):** LR = 0,811 ± 0,024 · RF = 0,739 ± 0,016 — hasil stabil, bukan kebetulan split.

**McNemar's Test:** p-value = 0,0001 (< 0,05) → perbedaan performa kedua model **signifikan secara statistik**.

---

## 💡 Key Findings

- Data churn **imbalanced** (73,5% : 26,5%) — Accuracy sengaja tidak dipakai sebagai metrik penentu, melainkan **Recall**, karena biaya kehilangan pelanggan yang benar-benar churn lebih mahal dibanding *false alarm*
- **Logistic Regression dipilih** sebagai model final — meski lebih sederhana dari Random Forest, recall-nya lebih tinggi dan terbukti signifikan lewat cross-validation & McNemar's test
- Feature importance kedua model (Tenure, Contract, Charges, Payment Method) **konsisten dengan temuan EDA sebelumnya** — memvalidasi ulang insight dari kedua project
- Trade-off diakui secara jujur: precision model hanya 51,1%, artinya perlu anggaran contact yang cukup di sisi tim retensi

---

## 📁 Struktur Repository

```
├── Churn_Prediction_Modeling.ipynb   # Notebook utama (kode + hasil eksekusi)
├── Telco_customer_churn.csv           # Dataset (sama dengan project EDA)
└── README.md
```

## ▶️ Cara Menjalankan

```bash
pip install pandas numpy scikit-learn statsmodels matplotlib seaborn
jupyter notebook Churn_Prediction_Modeling.ipynb
```



Terbuka untuk diskusi, feedback, atau peluang kolaborasi 🙌
