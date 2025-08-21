<!-- Badges -->
<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10%2B-blue" />
  <img src="https://img.shields.io/badge/Notebook-Jupyter-orange" />
  <img src="https://img.shields.io/badge/Topic-Customer%20Retention-10b981" />
  <img src="https://img.shields.io/badge/Stats-95%25%20CI%20%26%20A%2FB-8b5cf6" />
  <img src="https://img.shields.io/badge/License-MIT-black" />
</p>

<h1 align="center">Churn Segmentation + 14-Day A/B Scorecard (MVP)</h1>
<p align="center">Behavior-based micro-segments (RFM + Frequency/Monetary), 50/50 A/B, 14-day outcomes, scorecard with 95% CIs → GO/NO-GO.</p>

---

### 🔎 Nedir?
Bazı müşteriler “uzaklaşıyor”. Bu not defteri; **kim** uzaklaşıyor, **neden** ve **nasıl geri kazanırız** sorularına **davranış sinyalleri** ile yanıt arar:

- **Özellikler:** RFM (Recency proxy = `Months_Inactive_12_mon`), **F**(requency), **M**(onetary)  
- **Mikro-segmentler:** Düşük kullanım odaklı (örn. `RFM=Low | F=Low | M=Low/Mid-Low`)  
- **A/B (14 gün):** Yarı **test**, yarı **kontrol**  
- **Değerlendirme:** **Churn**, **Tx rate**, **Avg amount** farkları; **%95 CI** & **p-değeri** → **GO/NO-GO**

### 📌 Bulgu Özeti (örnek koşumdan)
- **Genel churn:** **%16.07**  
- **Riski en yüksek mikro-segment:** `RFM=Low | F=Low | M=Mid-Low` → **churn ≈ %82.16**  
- **A/B farkları (Test − Control):**
  - `RFM=Low | F=Low | M=Mid-Low` → **Churn −12.22 pp (p≈0.0021)**, **Tx +2.68 pp**, **Avg +2.98**
  - `RFM=Low | F=Low | M=Low`     → **Churn −5.46 pp (p≈0.0045)**, **Tx +4.81 pp**, **Avg +0.83**
- **Karar:** Düşük kullanım segmentleri için **GO**. Belirsiz segmentler için **NO-GO/Revize** (yeniden test).

> Not: Gerçek 14 günlük sonuç yoksa notebook **sentetik** (demo) sonuç üretebilir.

### 🧪 %95 CI bize ne söyler?
- **Test − Kontrol** farkının belirsizlik bandıdır.  
- CI **tamamı 0’ın altındaysa** (churn için) ve **p < 0.05**, etki büyük olasılıkla **gerçektir** → **GO**.  
- CI **0’ı kesiyorsa**, fark **tesadüf** olabilir → **NO-GO/Revize**.

### 🚀 Hızlı Başlangıç
```bash
# Klasör yapısı
# project/
# ├─ churn_ab_mvp.ipynb
# ├─ data/                 <-- CSV buraya (git'e dahil değil)
# ├─ outputs/              <-- çalışınca oluşur
# └─ reports/              <-- çalışınca oluşur

pip install -r requirements.txt   # opsiyonel
# Kaggle'dan veriyi indir:
# ./data/BankChurners.csv
# Notebook'u aç ve Run All
```

📂 **Yapı**

```bash
.
├─ churn_ab_mvp.ipynb     # TR/EN açıklamalı tek not defteri
├─ data/                  # (git dışı) BankChurners.csv
├─ outputs/               # skor kart & grafikler
└─ reports/               # OnePager_TR_EN.md
```

📊 **Yöntemler**

Kategorikler: **Ki-kare + Cramér’s V**

Sayısal/Ordinal: Spearman ρ

**A/B (14g): İki-oran z-testi (churn, tx), Ortalama farkı (Welch approx), Wilson %95 CI**



# Churn Segmentation + 14-Day A/B (MVP)

**Notebook:** [`churn_ab_mvp.ipynb`](./churn_ab_mvp.ipynb)  
**Data path:** `./data/BankChurners.csv` (not tracked — create locally)

## Quick Start
1) Create `data/` under the project root and place `BankChurners.csv` there.  
2) Open `churn_ab_mvp.ipynb` and **Run All**.  
3) Outputs will appear under `./outputs/` and `./reports/`.

## What this does
- Behavior features: RFM (recency proxy via `Months_Inactive_12_mon`), Frequency, Monetary  
- Micro-segmentation focused on low usage  
- 50/50 A/B assignment; 14-day outcomes (synthetic fallback)  
- Scorecard + 95% CI & p-values → **GO/NO-GO** decision

## Notes
- Dataset is intentionally excluded from git.  
- If you need figures in this README, copy selected PNGs into a `docs/` folder and link them (do not track the whole `outputs/`).

## **Methods**
- **EDA (exploratory):**
  - Categoricals: **Chi-square** + **Cramér’s V** (association & strength)
  - Numeric/ordinal: **Spearman ρ** (monotonic association)
- **A/B (14-day):**
  - **Two-proportion z-test** for **churn** and **tx rate**  
    → report **difference in percentage points** + **95% CI**
  - **Mean difference (Welch approx.)** for **average amount**  
    → difference + **95% CI**
  - **Wilson 95% CI** for per-arm proportions (used in error bars)
- **Decision rule:** **GO** if churn diff ≤ **−3.0 pp** **and** p < 0.05; otherwise **NO-GO/Revise** and re-test.

## Notes
- Dataset is **intentionally excluded** from git (keep it under `./data/` locally).  
- If you want to showcase figures in this README, copy selected PNGs into a `docs/` folder and reference them:
  ```markdown
  ![Churn diff (95% CI)](./docs/churn_diff_ci.png)
