# Banka Müşteri Kaybı (Churn) Tahmini

Makine öğrenmesi bootcamp final ödevi kapsamında hazırlanmış uçtan uca bir
sınıflandırma (classification) projesidir.

## Projenin Amacı

Bir bankanın mevcut müşterilerine ait demografik ve bankacılık verilerini
kullanarak, bir müşterinin yakın gelecekte bankadan ayrılıp
ayrılmayacağını (**churn**) tahmin eden bir makine öğrenmesi modeli
geliştirmek. Bu sayede banka, ayrılma riski yüksek müşterilere yönelik
proaktif müşteri elde tutma (retention) stratejileri geliştirebilir.

## Veri Seti

- **Dosya:** `Churn_Modelling.csv`
- **Kaynak:** Kaggle (Bank Customer Churn Modelling)
- **Boyut:** 10.000 satır, 14 sütun
- **Hedef değişken:** `Exited` (1 = müşteri bankadan ayrıldı, 0 = müşteri kaldı)
- **Problem türü:** Sınıflandırma (classification), ikili (binary)
- **Öznitelikler:** `CreditScore`, `Geography`, `Gender`, `Age`, `Tenure`,
  `Balance`, `NumOfProducts`, `HasCrCard`, `IsActiveMember`,
  `EstimatedSalary` (kimlik sütunları olan `RowNumber`, `CustomerId`,
  `Surname` modelleme için anlamsız olduğundan çıkarılmıştır)

## Proje Akışı (`main.ipynb`)

1. Veri inceleme (satır/sütun sayısı, veri tipleri, temel istatistikler)
2. Eksik değer ve tekrarlanan satır kontrolü
3. Kategorik değişkenlerin encoding'i (One-Hot / Binary)
4. Aykırı değer incelemesi (IQR yöntemi) ve sınırlandırma (capping)
5. Öznitelik mühendisliği: `BalanceSalaryRatio`, `IsZeroBalance`,
   `ProductsPerTenure`, `AgeGroup`
6. Öznitelik seçimi: korelasyon analizi + Random Forest feature importance
7. Train (%60) / Validation (%20) / Test (%20) ayrımı (stratify ile)
8. 5 farklı modelin eğitimi: Logistic Regression, KNN, Decision Tree,
   Random Forest, SVM
9. 5 katlı çapraz doğrulama ve validation kümesi üzerinde model karşılaştırması
10. Grid Search ile hiperparametre ayarlama
11. En iyi modelin test kümesinde değerlendirilmesi (confusion matrix,
    accuracy, precision, recall, F1-score, ROC-AUC)
12. Sonuç yorumu ve model açıklanabilirliği (feature importance)

## Nasıl Çalıştırılır

```bash
# 1) Gerekli kütüphaneleri kurun
pip install -r requirements.txt

# 2) Jupyter'i başlatın ve main.ipynb dosyasını açın
jupyter notebook main.ipynb

# 3) Hücreleri sırasıyla (yukarıdan aşağıya) çalıştırın
```

`Churn_Modelling.csv` dosyasının `main.ipynb` ile aynı klasörde olduğundan
emin olun.

## Sonuç Özeti

5 model arasında en iyi performansı **Random Forest** göstermiştir
(hiperparametre ayarlaması sonrası). Test kümesi sonuçları:

| Metrik    | Değer |
|-----------|-------|
| Accuracy  | 0.860 |
| Precision | 0.782 |
| Recall    | 0.432 |
| F1-score  | 0.557 |
| ROC-AUC   | 0.852 |

Churn tahmininde en belirleyici değişkenler **yaş (Age)**, **ürün sayısı
(NumOfProducts)**, **tahmini maaş (EstimatedSalary)**, **kredi skoru
(CreditScore)** ve **bakiye (Balance)** olarak bulunmuştur. Veri setinin
dengesiz olması (%20 churn) nedeniyle model, ayrılan müşterilerin yaklaşık
%43'ünü yakalayabilmektedir (Recall); bankanın önceliğine göre karar eşiği
veya dengesiz veri teknikleri (class_weight, SMOTE) ile bu oran
artırılabilir. Detaylı yorum için `main.ipynb` içindeki Bölüm 16'ya
bakınız.

## Dosya Yapısı

```
.
├── main.ipynb            # Uçtan uca ML notebook'u
├── Churn_Modelling.csv   # Veri seti
├── requirements.txt      # Gerekli Python kütüphaneleri
└── README.md
```
