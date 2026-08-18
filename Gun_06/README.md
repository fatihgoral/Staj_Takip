
# Gün 6 — Genel Akış Özeti

**Tarih:** 11 Ağustos 2026

## Hedef
Anomali tespiti için gerçek doğrulama veri setinin ilk adımını atmak: bir etiketleme
projesi açmak, ham görselleri havuza almak, sınıf tanımlarını hazırlamak, otomatik
ön-etiketleme (bootstrap) turunu çalıştırmak ve etiketleme kurallarını (protokol)
yazılı hâle getirmek.

## Ne yaptık

### 1. Plan sapması ve çözümü
Başlangıçta öngörülen veri hacminden çok daha küçük bir kare seti elimize geçti;
üstelik gelen setin tamamı zaten "engelli/anomali içeren" örneklerden oluşuyordu.
Bu durum, modelin yanlış alarm oranını ölçebilmemiz için gereken "temiz/engelsiz"
örnek katmanını eksik bırakıyordu.

Çözüm: Daha önceki bir aşamada üretilmiş, doğrulanmış temiz video kliplerinden
kare kare görsel çıkararak eksik katmanı kendi imkânlarımızla (yerel işlem, ek bir
kaynak gerektirmeden) tamamladık.

### 2. Ortam kurulumu ve proje açma
Bulut tabanlı bir GPU ortamında etiketleme platformunu kurduk, projeyi açtık,
ham görselleri projeye bağladık, sınıf tanımlarını (birden çok sınıf, eş anlamlı
ifadelerle zenginleştirilmiş) elle yazdık.

**Karşılaşılan sorun:** İlk proje oluşturma denemesi eksik kalmış, sistem
yanlışlıkla önceki/varsayılan bir projenin ayarlarını kullanmaya devam ediyordu.
**Çözüm:** Proje oluşturma adımı çıktısı doğrulanarak tekrar çalıştırıldı, doğru
ayarlarla düzeltildi.

### 3. Otomatik ön-etiketleme (Bootstrap) turu
Görsellerin tamamı, açık kaynaklı nesne tespiti ve segmentasyon modelleri
kullanılarak otomatik olarak ön-etiketlendi.

**Karşılaşılan sorunlar:**
- Gerekli bir kütüphane ortamda kurulu değildi → kuruldu.
- Segmentasyon modelinin ağırlık dosyası (checkpoint, birkaç GB) hiç indirilmemişti
  → resmi kaynağından indirildi.

Bu düzeltmelerden sonra tur başarıyla tamamlandı: çok sayıda görsel birkaç saatte
işlendi, sonuçlar güven seviyesine göre üç gruba ayrıldı (yüksek güven / incelenmesi
gereken / reddedilen).

### 4. Etiketleme protokolü
Farklı zamanlarda farklı kişilerin tutarlı etiketleme yapabilmesi için yazılı bir
protokol hazırlandı: sınıf tanımları, mesafe/kısmi görünürlük/belirsizlik kuralları,
kalite kontrol yöntemi (örneklem çapraz kontrolü) ve veri setinin "dondurulma"
kuralı (bir kez onaylandıktan sonra bir daha değiştirilmeyeceği).

### 5. Kritik olay — geçici depolama / kalıcı depolama farkı
Bootstrap turunun ilk çalıştırması, çalışma ortamının **geçici** (oturuma bağlı)
diskinde gerçekleştirilmişti ve sonuçlar kalıcı depolamaya yedeklenmemişti. Oturum
kesintiye uğrayınca bu geçici disk sıfırlandı ve ilk turun çıktıları kayboldu.

**Çözüm:** Proje ve model dosyaları kalıcı depolamaya taşınacak şekilde yeniden
kuruldu, Bootstrap turu tekrar çalıştırıldı; sonuç ilk turla birebir aynı çıktıyı
verdi. Veri kaybı olmadı, yalnızca birkaç saatlik işlem tekrarlandı. Sonuçlar
bağımsız bir kanaldan fiziksel olarak doğrulandı.

## Öğrendiklerimiz
- Geçici disk ≠ kalıcı depolama — uzun süren işler başlamadan önce çıktının kalıcı
  depolamaya yazıldığından emin olunmalı.
- "Kaydedildi" demek yetmiyor, bağımsız bir kanaldan doğrulamak gerekiyor.
- Plan sapmaları (veri hacmi/bileşimi değişince) hemen ölçüm hedeflerine etkisi
  değerlendirilip erken telafi edilmeli.
- Eksik kütüphane/model dosyası gibi hatalar sırayla, panik yapmadan çözülebiliyor.

## Gün sonu durumu
- Etiketleme projesi kalıcı olarak kuruldu, ham veri havuzu tamamlandı.
- Otomatik ön-etiketleme turu tamamlandı, sonuçlar kalıcı depoda ve doğrulandı.
- Etiketleme protokolü yazıldı (yaşanan olay da şeffaflık için protokole not
  olarak eklendi).
- Bir sonraki adım: ön-etiketlenmiş, incelenmesi gereken karelerin insan
  gözetiminde tek tek onaylanması.
