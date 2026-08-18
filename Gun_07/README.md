# Gün 7 — Yöntem kararı ve üretim hattının tam kapasiteye geçirilmesi

**Tarih:** 11 Ağustos 2026

**Hedef:** Doğrulama seti için izlenecek yöntemi netleştirmek ve üretim hattının bu yönteme uygun, tam kapasitede çalıştığından emin olmak.

## Genel Akış

Gün, ilgili sorumluyla yapılan bir değerlendirme görüşmesiyle başladı. Görüşmede doğrulama setinin nasıl oluşturulacağına dair yöntem netleştirildi ve veri bölme (eğitim/doğrulama ayrımı) stratejisi üzerinde duruldu. Kararlar netleşir netleşmez, üretim hattının bu kararları destekleyecek ölçekte çalışıp çalışmadığı kontrol edildi; bu kontrolde hattın henüz tam kapasitede çalıştırılmadığı görüldü ve aynı gün giderildi.

## Yapılanlar

**Yöntem değerlendirmesi**

- İlgili sorumluyla, doğrulama setinin nasıl oluşturulacağına dair bir değerlendirme görüşmesi yapıldı.
- Doğrulama setinin, eğitim setiyle tutarlı bir mantıkla — gerçek arka plan videosu üzerine sentetik engel yerleştirilerek — oluşturulmasına karar verildi. Bu yaklaşımın hem üretilebilir hem de kaynak bazlı bağımsız, sağlam bir karşılaştırma sağladığı değerlendirildi.

**Veri bölme stratejisi**

- Elimizdeki iki bağımsız kaynak videonun eğitim/doğrulama arasında nasıl bölüneceği değerlendirildi; birkaç seçenek karşılaştırıldı.
- Modelin aynı sahne/ışık/kamera açısını hem eğitimde hem doğrulamada görüp "ezber" yapma riskini en aza indiren seçenek — bir kaynağı tamamen eğitim, diğerini tamamen doğrulama için kullanmak — tercih edildi.

**Üretim hattı kontrolü ve tam kapasiteye geçiş**

- Karar netleşince, üretim hattının bu kararı destekleyecek ölçekte veri üretip üretmediği kontrol edildi.
- Kontrolde, hattın önceki adımlarının (temiz video hazırlama ve engel yerleştirme) o ana kadar yalnızca küçük, test amaçlı bir modda çalıştırıldığı, hiç tam kapasitede işletilmediği görüldü.
- Bu eksiklik aynı gün giderildi: hattın ilk aşaması, hedeflenen tüm veri miktarıyla tam kapasitede yeniden başlatıldı.

**Belgeleme**

- Alınan yöntem ve bölme kararları, gerekçeleriyle birlikte ilgili proje dokümanına işlendi.

## Çözülen Sorunlar

- **Test modunda kalma riski:** Üretim hattının sadece küçük ölçekte çalıştırılmış olması kontrol edilmeden fark edilmeyebilirdi; aynı gün içinde tespit edilip düzeltildi.
- **Veri bölme belirsizliği:** Birden fazla makul seçenek arasında, sızıntı riskini en aza indiren yöntem gerekçeleriyle birlikte netleştirildi.

## Karşılaşılan Zorluklar

- Sistemin daha önce bir kez küçük ölçekte çalıştırılmış olması "üretim zaten yapıldı" izlenimi verebiliyordu; bunun sadece test verisi olduğunu ayırt etmek dikkatli bir kontrol gerektirdi.
- Birden fazla veri bölme seçeneğinin her birinin kendi avantaj/riskini tartmak, tek bir "doğru" cevabı olmayan bir karar sürecini gerektirdi.

## Öğrenilenler

**Teknik**

- Bir sistemin "çalıştı" görünmesi, hedeflenen ölçekte/gerçek modda çalıştığı anlamına gelmiyor — çıktının hacmini ve modunu ayrıca doğrulamak gerekiyor.

**Süreç ve yöntem**

- Yöntem kararını netleştirdikten hemen sonra altyapının bu kararı destekleyip desteklemediğini kontrol etmek, kararla uygulama arasındaki boşluğu ortadan kaldırıyor.
- Veri bölme gibi kararlarda "en kolay" seçenek yerine sızıntı riskini en aza indiren seçeneği tercih etmek, ileride ortaya çıkabilecek güvenilirlik sorunlarını baştan önlüyor.

## Sonuç

Gün sonunda hem doğrulama seti yöntemi hem de veri bölme stratejisi netleşmiş, üretim hattı da bu karara uygun şekilde tam kapasitede çalışır hale getirilmişti.

## Sıradaki Adımlar

- Üretim hattının tamamlanmasının beklenmesi ve çıktının doğrulanması.
- Çıktının, karar verilen bölme stratejisine göre eğitime hazır hale getirilmesi.
