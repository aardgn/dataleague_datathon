# dataleague_datathon
Health Data No-Show Prediction pipeline using XGBoost, LightGBM, and CatBoost. Developed as the Team Lead for the Dataleague Datathon.
# Health Data No-Show Prediction (Dataleague Datathon)

Bu proje, Dataleague Datathon kapsamında takım lideri olarak kurgulayıp geliştirdiğim, hastaların sağlık randevularına gelmeme durumlarını tahmin etmeyi amaçlayan uçtan uca bir makine öğrenmesi pipeline'ıdır.

**Başarı Notu:** İlk datathon katılımım olmasına rağmen 72 takım arasından rekabetçi bir skor (0.5120) elde edilmiştir.

---

## Teknik Yaklaşım ve Mimari Çözümler

Kısıtlı süre altında karşılaşılan veri problemlerine uyguladığım mühendislik çözümleri:

### 1. Dengesiz Veri Seti Yönetimi
* **Sorun:** Veri setinde randevusuna gelen hasta sayısı, gelmeyenlere kıyasla çok daha baskındı.
* **Çözüm:** Model eğitiminde Scale Pos Weight parametresi kullanarak azınlık sınıfa daha fazla ağırlık verdim. Modelin gerçek performansını ölçmek için Accuracy yerine F1-Score ve PR-AUC metriklerine odaklandım.

### 2. Gelişmiş Feature Engineering
* **Türetilmiş Metrikler:** Ham "Randevu" ve "Kayıt" tarihlerini birbirinden çıkararak "Bekleme Süresi" gibi modelin tahmin gücünü artıran yeni özellikler ürettim.
* **Target Encoding:** Kategorik değişkenlerin (Bölüm, Randevu Tipi vb.) sınıf bazlı gelmeme oranlarını hesaplayarak modele sayısal girdiler olarak sundum.

### 3. İleri Seviye Makine Öğrenmesi Taktikleri
* **Ensemble Learning:** Sektör standardı XGBoost, LightGBM ve CatBoost modellerini kurdum ve çıktılarını `average_precision_score` değerini maksimize edecek şekilde ağırlıklandırdım.
* **Seed Averaging:** Rastgeleliğin yarattığı sapmaları engellemek için modelleri 5 farklı seed değeriyle eğitip ortalamalarını aldım.
* **Pseudo Labeling:** Modelin test verisindeki çok yüksek (%85 üstü) ve çok düşük (%10 altı) olasılıklı tahminlerini "sahte etiket" olarak eğitim setine dahil ederek modelin sınırlarını keskinleştirdim.

---

## Dosya Yapısı ve Kullanım

* `datathon_final.ipynb`: Veri ön işleme, özellik mühendisliği, model eğitimi, ensemble ağırlık optimizasyonu ve çıktı üretme adımlarının tamamını içeren detaylı Jupyter Notebook.
