# Çiçek Türleri Görüntü Sınıflandırma Projesi (IYU328)

Bu proje, IYU328 dersi kapsamında geliştirdiğim görüntü sınıflandırma çalışmasıdır. 5 farklı çiçek türünü (papatya, karahindiba, gül, ayçiçeği, lale) sınıflandırmak için üç farklı yaklaşımı karşılaştırdım: klasik makine öğrenmesi (SVM), sıfırdan eğitilmiş CNN ve transfer öğrenme (MobileNetV2).

---

## Proje Özeti

Çalışmada temel amacım, aynı veri kümesi üzerinde farklı yaklaşımların performansını adil bir şekilde karşılaştırmaktı. Veri kümesini eğitim/doğrulama/test olarak ayırırken sınıf bazlı rastgele bölme yaptım, böylece veri sızıntısı olmadan değerlendirme yapabildim.

Geliştirdiğim üç model:
- **SVM (Lineer Destek Vektör Makinesi):** Düzleştirilmiş piksel vektörleri üzerinde klasik ML yaklaşımı
- **Özel CNN (Sıfırdan Derin Öğrenme):** 2 evrişim bloğundan oluşan basit bir CNN mimarisi
- **MobileNetV2 (Transfer Öğrenme):** ImageNet üzerinde önceden eğitilmiş, dondurulmuş taban katmanlı model

---

## Veri Kümesi

- **Kaynak:** TensorFlow resmi örnek veri sunucusu
- **Bağlantı:** https://storage.googleapis.com/download.tensorflow.org/example_images/flower_photos.tgz
- **Sınıflar:** 5 (daisy, dandelion, roses, sunflowers, tulips)
- **Toplam Görüntü:** 3670
- **Bölünme:** %70 eğitim (2567), %15 doğrulama (551), %15 test (552)
- **Seed:** 42 (tekrarlanabilirlik için)

Notebook çalıştırıldığında veri kümesi otomatik olarak indirilip train/val/test klasörlerine ayrıştırılıyor. Ekstra bir kurulum adımına gerek yok.

---

## Kurulum ve Gereksinimler

Projeyi çalıştırmak için gerekli kütüphaneler `requirements.txt` dosyasında listelenmiştir. Aşağıdaki komutla kurabilirsiniz:

```bash
pip install -r requirements.txt
```

**Önerilen ortam:** Google Colab (GPU çalışma zamanı ile). Eğitim süreleri CPU'ya göre oldukça kısa.

---

## Kullanım

1. Notebook'u Google Colab'da açın ve GPU çalışma zamanı seçin (`Runtime > Change runtime type > GPU`).
2. Hücreleri sırasıyla çalıştırın.

Hücrelerin akışı şu şekilde:
- **Hücre 0-1:** Veri kümesini indirme ve bölme
- **Hücre 2:** `ImageDataGenerator` ile veri jeneratörlerini oluşturma
- **Hücre 3:** SVM modeli eğitimi ve değerlendirme
- **Hücre 4-6:** Özel CNN modeli oluşturma ve 20 epoch eğitim
- **Hücre 7:** MobileNetV2 transfer öğrenme modeli oluşturma ve 20 epoch eğitim
- **Hücre 8:** Öğrenme eğrileri (accuracy/loss) görselleştirme
- **Hücre 9:** Test kümesi üzerinde sınıflandırma raporları ve karmaşıklık matrisleri
- **Hücre 10:** En iyi modelin yanlış sınıflandırdığı örneklerin analizi
- **Hücre 11:** Grad-CAM ile açıklanabilirlik (XAI) analizi
- **Hücre 12:** Üç modelin performans metriklerinin karşılaştırma tablosu
- **Hücre 13:** Detaylı hata analizi

**Tahmini çalışma süresi:** GPU'lu Colab ortamında toplam ~20-25 dakika.

---

## Sonuçlar

| Model | Doğruluk | Precision | Recall | Macro F1 |
|-------|----------|-----------|--------|----------|
| SVM (Klasik ML) | %40.2 | 0.412 | 0.387 | 0.387 |
| Özel CNN | %69.0 | 0.722 | 0.678 | 0.674 |
| MobileNetV2 (Transfer Öğrenme) | **%86.1** | **0.864** | **0.854** | **0.856** |

En başarılı model **MobileNetV2** oldu. Transfer öğrenme yaklaşımının, sınırlı veri ile bile güçlü sonuçlar verdiğini gözlemledim. Hata analizinde modelin en çok gül ve lale sınıfları arasında karıştığını fark ettim; bu da görsel benzerlikten kaynaklanıyor. Grad-CAM analizi ile modelin karar verirken çiçeğin taç yaprak ve yaprak bölgelerine odaklandığını doğruladım.

---

## Proje Yapısı

```
.
├── IYU328PROJE.ipynb       # Ana çalışma notebook'u
├── IYU328_Rapor.pdf        # IEEE formatında proje raporu
├── README.md                 # Bu dosya
├── requirements.txt         # Python bağımlılıkları
└── .gitignore               # Git ignore kuralları
```

---

## Karşılaştığım Zorluklar ve Notlar

- Veri kümesi boyutu (3670 görüntü) derin öğrenme için biraz sınırlıydı. Bu yüzden transfer öğrenme en mantıklı seçenek oldu.
- Görüntüleri 128x128 çözünürlüğe indirgemek bazı ince detayların kaybolmasına neden olabilir. Daha yüksek çözünürlük denenebilir.
- MobileNetV2'de sadece üst katmanları eğittim, taban katmanlara ince ayar (fine-tuning) yapmadım. İleriye dönük olarak fine-tuning ile daha iyi sonuçlar alınabilir.

---

## Yazar

**Taha Doğukan Bahadır**  
Yapay Zeka Mühendisliği  
OSTİM Teknik Üniversitesi

---

*Bu proje IYU328 dersi kapsamında hazırlanmıştır.*
