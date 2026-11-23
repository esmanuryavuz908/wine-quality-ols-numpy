# Wine Quality OLS Linear Regression (NumPy)

Bu proje, harici bir makine öğrenmesi kütüphanesi (`scikit-learn` vb.) kullanmadan yalnızca **NumPy** ve vektörize matris işlemleri ile sıfırdan **En Küçük Kareler (OLS - Ordinary Least Squares) Doğrusal Regresyon** modeli kurarak kırmızı şarap kalitesini tahmin etmeyi amaçlar.

## Veri Seti

- **Dosya:** `winequality-red.csv`
- **Boyut:** 1.599 gözlem, 11 bağımsız fizikokimyasal özellik ve 1 hedef değişken (`quality`)
- **Özellikler:** `fixed acidity`, `volatile acidity`, `citric acid`, `residual sugar`, `chlorides`, `free sulfur dioxide`, `total sulfur dioxide`, `density`, `pH`, `sulphates`, `alcohol`

## Uygulanan Adımlar

1. **Veri Yükleme ve Kontrol:** Veri seti `np.genfromtxt` ile okunarak başlıklar ve sayısal matris (`X`, `y`) ayrıştırıldı; eksik (`NaN`) ve sonsuz (`Inf`) değer bulunmadığı doğrulandı.
2. **Filtreleme:** Yüksek kaliteli şaraplar (`quality >= 7`) ile ortalamanın üzerinde alkol oranına ve `3.0 - 3.5` pH aralığına sahip gözlemler filtrelendi.
3. **Ölçeklendirme:** Özellik matrisi üzerinde hem **Min-Max Normalizasyonu** hem de **Z-Score Standardizasyonu** uygulandı.
4. **Kovaryans ve Korelasyon Analizi:** Standardize edilmiş verinin kovaryans ve korelasyon matrisleri hesaplandı.
5. **Özellik Seçimi:** Kalite (`quality`) ile mutlak korelasyonu en yüksek olan 3 özellik seçildi:
   - `alcohol` (indeks 10)
   - `volatile acidity` (indeks 1)
   - `sulphates` (indeks 9)
6. **Modelleme (Normal Denklem):** Bias sütunu eklenerek ağırlık vektörü $\theta = (X^T X)^{-1} X^T y$ formülüyle doğrudan hesaplandı.

## Sonuçlar

| Metrik / Parametre | Değer |
| --- | --- |
| Bias ($\theta_0$) | `5.6360` |
| `sulphates` katsayısı ($\theta_1$) | `0.1151` |
| `volatile acidity` katsayısı ($\theta_2$) | `-0.2186` |
| `alcohol` katsayısı ($\theta_3$) | `0.3294` |
| Ortalama Kare Hata (MSE) | `0.4328` |
| Başarı Skoru ($R^2$) | `0.3359` |

## Kurulum ve Çalıştırma

```bash
pip install -r requirements.txt
jupyter notebook wine_quality_ols_numpy.ipynb
```
