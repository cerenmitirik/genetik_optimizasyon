# 🏭 Endüstriyel Boya Karışımı Optimizasyonu (Genetik Algoritma)

Bu proje, **Yapay Zeka Sistemleri** dersi kapsamında, kısıtlı bir optimizasyon problemini çözmek amacıyla geliştirilmiştir. Projede, fabrikadaki iki farklı pigmentin (A ve B) ideal karışım oranlarını bulan bir **Genetik Algoritma** tasarlanmıştır.

---

## 👤 Öğrenci Bilgileri
* **Ad Soyad:** Ceren Mıtırık
* **Öğrenci No:** 2212721032
* **Ders:** BLG307 - Yapay Zeka Sistemleri
* **Proje Konusu:**  Endüstriyel Boya Karışımı 

---

## 📌 Problem Tanımı ve Senaryo

Bir fabrika, iki tür pigment karışımıyla ideal renk yoğunluğunu yakalamak istemektedir. Problem, belirli kısıtlar altında kalite puanını maksimize etmeye dayalıdır.
Not: Kalite, puan, ideal kavramlarında problemi maksimize etmemiz gerekir.

### 1. Matematiksel Model
**Amaç Fonksiyonu (Renk Kalitesi):**
$$y = 5x_1 + 2x_2 - x_1x_2$$

**Değişkenler:**
* $x_1$: Pigment A Oranı (%)
* $x_2$: Pigment B Oranı (%)

### 2. Kısıtlar (Constraints)
Algoritma aşağıdaki zorunlu kurallara uymak zorundadır:
1. **Karışım Kısıtı:** $x_1 + x_2 = 100$ (Toplam oran tam %100 olmalıdır)
2. **Hammadde Kısıtı:** $x_1 \ge 30$ (A pigmenti en az %30 kullanılmalıdır)

---

## 🧬 Kullanılan Yöntem ve Algoritma Detayları

Bu projede, problemin doğası gereği (negatif puan ihtimali ve kesin kısıtlar) özel genetik operatörler tercih edilmiştir.

### 1. Ceza Yöntemi
Kısıtları sağlamayan bireyler doğrudan elenmek yerine, hata paylarına göre cezalandırılmıştır. Bu sayede çeşitliliği koruyarak bu problemi çözmüş oluruz.
* **Toplam Hatası:** Eğer $x_1+x_2 \neq 100$ ise, fark başına **50 puan** ceza.
* **Sınır Hatası:** Eğer $x_1 < 30$ ise, fark başına **100 puan** ceza.
* *Sonuç:* Algoritma, ceza yememek için kısıtları sağlamayı "öğrenmiştir".

### 2. Genetik Operatörler
* **Seçim (Selection):** `Turnuva Seçimi (Tournament Selection)` kullanılmıştır. 
    * *Neden?* Ceza puanlarından dolayı Fitness değeri negatife düşebilmektedir. Rulet yöntemi olasılık hesabı yaptığı için ($Olasılık = Puan / Toplam$) negatif sayılarla matematiksel olarak çalışamaz. Rank yöntemi ise her döngüde tüm popülasyonu sıralamayı (sorting) gerektirdiği için işlem maliyeti yüksek olur. 
    * Turnuva seçimi, puanların sayısal değeriyle değil, büyüklük sıralamasıyla ilgilenir. (Örn: $-50 > -5000$). Bu yüzden negatif puanlarda bile hatasız çalışır.
* **Çaprazlama (Crossover):** `Aritmetik Çaprazlama` kullanılmıştır.
    * *Neden?* Değişkenler sürekli (reel) sayı olduğu için, anne ve babanın genlerinin ağırlıklı ortalaması alınarak yeni bireyler üretilmiştir.
* **Mutasyon:** `Uniform Mutasyon` kullanılmıştır.
    * Genlerde %20 ihtimalle ±5 birimlik rastgele değişimler yapılarak yerel tuzaklardan kaçış sağlanmıştır. Örneğin pigment A %40'tı artık %45 oldu. Pigment A'nın hangi durumda daha iyi olacağını bunu yapmadan bilemezdik. Mutasyon sayesinde deneme şansımız oldu.
      

---

## 📊 Sonuçların Analizi

Algoritma 100 nesil boyunca çalıştırılmış ve aşağıdaki sonuçlar elde edilmiştir:

| Parametre | Bulunan Değer | Hedef / Açıklama |
|-----------|---------------|------------------|
| **Pigment A ($x_1$)** | **%100.00** | Kısıt ($x_1 \ge 30$) sağlandı. |
| **Pigment B ($x_2$)** | **%0.00** | - |
| **Toplam Oran** | **%100.00** | Kısıt ($x_1+x_2=100$) sağlandı. |
| **Fitness Puanı** | **500.0** | Global Optimum'a ulaşıldı. |

**Yorum:**
Algoritma ilk 20 nesilde hızlı bir öğrenme süreci geçirmiş, hatalı (ceza alan) bireyleri eleyerek kısıtları sağlayan bölgeye yönelmiştir. Matematiksel olarak formülde $x_1$'in katsayısı daha büyük olduğu için, algoritma $x_1$'i maksimize edip $x_2$'yi minimize ederek (ancak toplamı 100'de tutarak) mümkün olan en yüksek puanı (500) bulmuştur.

---

## 🚀 Kurulum ve Çalıştırma

Proje Google Colab üzerinde geliştirilmiştir. Çalıştırmak için:
1. `.ipynb` uzantılı dosyayı Google Colab'de açın.
2. `Runtime` > `Run all` menüsüne tıklayın.
3. En altta sonuç grafikleri ve raporu görüntülenecektir.

---
