# 🍷 Sıfırdan Vektörize Şarap Kalitesi Tahmini (Pure NumPy)

![Python](https://img.shields.io/badge/Python-3.13-blue?logo=python&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-Pure%20Vectorized-013243?logo=numpy&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)

Bu proje, `scikit-learn` veya `pandas` gibi hazır makine öğrenmesi ve veri manipülasyonu kütüphaneleri kullanmadan, **yalnızca saf `NumPy` ve lineer cebir işlemleriyle** uçtan uca veri ön işleme, istatistiksel analiz ve Çoklu Doğrusal Regresyon (OLS) modeli geliştirmeyi amaçlar.

---

## 🎯 Projenin Amacı

Hazır model fonksiyonlarını kara kutu olarak çağırmak yerine, bir makine öğrenmesi hattının arka planındaki matematiksel temelleri (ölçeklendirme, kovaryans/korelasyon matrisleri, özellik seçimi ve Normal Denklem çözümü) vektörize matris operasyonlarıyla sıfırdan inşa etmek.

---

## 📊 Veri Seti Özeti

Projede **Red Wine Quality (Kırmızı Şarap Kalitesi)** veri seti kullanılmıştır.

* **Gözlem Sayısı:** 1.599 satır
* **Bağımsız Değişkenler (`X`):** 11 fizikokimyasal özellik (`fixed acidity`, `volatile acidity`, `citric acid`, `residual sugar`, `chlorides`, `free sulfur dioxide`, `total sulfur dioxide`, `density`, `pH`, `sulphates`, `alcohol`)
* **Hedef Değişken (`y`):** `quality` (şarap kalite puanı)
* **Veri Bütünlüğü:** Eksik (`NaN`) veya sonsuz (`Inf`) değer bulunmamaktadır.

---

## ⚙️ Uygulanan Adımlar

1. **Veri Yükleme ve Ayrıştırma:** CSV dosyası `np.genfromtxt` ile okunarak başlıklar ayrıştırılmış, sayısal özellik matrisi (`X`) ve hedef vektör (`y`) oluşturulmuştur.
2. **Veri Kalitesi ve Koşullu Filtreleme:** `NaN`/`Inf` kontrolleri yapılmış; yüksek kaliteli şaraplar (`quality >= 7`) ile ortalamanın üzerinde alkol oranına ve `3.0–3.5` pH aralığına sahip örnekler vektörize maskeleme (`np.where`) ile filtrelenmiştir.
3. **Özellik Ölçeklendirme (Feature Scaling):**
   * **Min-Max Normalizasyonu:** $X_{normal} = \frac{X - X_{\min}}{X_{\max} - X_{\min}}$
   * **Z-Score Standardizasyonu:** $X_{standart} = \frac{X - \mu}{\sigma}$ (ortalama $\approx 0$, standart sapma $= 1$ olarak doğrulanmıştır).
4. **İstatistiksel Analiz ve Özellik Seçimi:** Standartlaştırılmış veri üzerinden $11 \times 11$ kovaryans ve korelasyon matrisleri hesaplanmış; hedef değişken (`quality`) ile mutlak korelasyonu en yüksek 3 özellik seçilmiştir:
   * `sulphates` (indeks 9)
   * `volatile acidity` (indeks 1)
   * `alcohol` (indeks 10)
5. **Sıfırdan Doğrusal Regresyon (Normal Equation):** Seçilen 3 özelliğe sabit terim (`bias`) sütunu eklenerek analitik çözümle en uygun ağırlık vektörü ($\theta$) hesaplanmıştır:

$$\theta = (X^T X)^{-1} X^T y$$

---

## 📈 Model Sonuçları ve Yorumlar

### Öğrenilen Parametreler ($\theta$)

Standartlaştırılmış özellikler üzerinden hesaplanan ağırlıklar, her değişkenin kalite puanına etkisini doğrudan göstermektedir:

| Parametre | Değişken | Ağırlık ($\theta$) | Etki Yönü |
| :--- | :--- | :---: | :--- |
| $\theta_0$ | Bias (Sabit Terim) | `5.6360` | Temel ortalama kalite puanı |
| $\theta_1$ | `sulphates` | `+0.1151` | Pozitif etki |
| $\theta_2$ | `volatile acidity` | `-0.2186` | Negatif etki (yüksek asidite kaliteyi düşürür) |
| $\theta_3$ | `alcohol` | `+0.3294` | En güçlü pozitif etki |

### Performans Metrikleri

| Metrik | Değer |
| :--- | :---: |
| **Ortalama Kare Hata (MSE)** | `0.4328` |
| **Belirlilik Katsayısı ($R^2$)** | `0.3359` |

*Yalnızca 3 özellik ve doğrusal bir model ile şarap kalitesindeki varyansın yaklaşık **%33,6'sı** açıklanabilmiştir.*

---

## 🚀 Kurulum ve Çalıştırma

1. Repoyu bilgisayarınıza klonlayın:
   ```bash
   git clone [https://github.com/KULLANICI_ADIN/REPO_ADIN.git](https://github.com/KULLANICI_ADIN/REPO_ADIN.git)
   cd REPO_ADIN
   ```

2. Gerekli kütüphaneleri yükleyin:
   ```bash
   pip install numpy jupyter
   ```

3. `winequality-red.csv` veri setinin notebook dosyasıyla aynı klasörde olduğundan emin olun *(kod içindeki dosya yolunu kendi dosya adınızla eşleştirin)*.

4. Jupyter Notebook'u başlatın ve hücreleri sırayla çalıştırın:
   ```bash
   jupyter notebook
   ```

---

## 🔮 Gelecek Geliştirmeler

* Veriyi eğitim ve test seti (`Train / Test Split`) olarak ayırarak genelleme performansını ölçmek
* Tüm 11 özelliği dahil ederek ve düzenlileştirme (Ridge / L2) ekleyerek $R^2$ skorunu karşılaştırmak
* Korelasyon ısı haritası ve gerçek–tahmin saçılım grafikleri eklemek

---

