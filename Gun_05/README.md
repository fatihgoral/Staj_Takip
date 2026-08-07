# Gün 5 — Çalışma Özeti

**Tarih:** 7 Ağustos 2026

**Hedef:** Sentetik veri üretim hattı ile model eğitim altyapısını ilk kez uçtan uca birbirine bağlamak ve iki sistemin gerçekten birlikte çalışabildiğini kanıtlamak (Demo #1). Bu aşamada amaç yüksek model performansı elde etmek değil, iki bağımsız bileşen arasındaki veri akışının sorunsuz işlediğini doğrulamaktı.


## Genel Akış

Güne, iki sistemin birbirine nasıl bağlanacağını netleştirerek başlandı. Önce her iki tarafın veri beklentileri incelendi, aralarında bir uyumsuzluk olduğu görüldü ve bunu gideren bir ara katman (dönüştürücü) geliştirildi. Ardından bulut tabanlı bir çalışma ortamı kurulup dönüştürücü bu ortamda test edildi, veri seti hazırlandı, küçük ölçekli bir deneme eğitimi koşturuldu ve sonuçlar ölçülüp kayıt altına alındı. Son olarak süreç boyunca edinilen gözlemler ve karşılaşılan sorunlar yazılı hale getirildi.

## Yapılanlar

**Analiz ve tasarım**
- İki sistemin veri giriş/çıkış beklentileri incelendi, aralarındaki uyumsuzluk noktaları tespit edildi.
- Uyumsuzluğu gidermek için gereken dönüşüm mantığı tasarlandı ve küçük bir örnek üzerinde doğrulandı.

**Geliştirme**
- İki sistemi birbirine bağlayan bir dönüştürücü/köprü script'i sıfırdan yazıldı.
- Script; veri dönüşümü, dosya taşıma/yeniden adlandırma ve gerekli yapılandırma dosyasının otomatik oluşturulması gibi birden fazla işlevi tek seferde yapacak şekilde tasarlandı.
- Yazılan script çalıştırılmadan önce sözdizimi ve mantık hatası kontrolünden geçirildi.

**Ortam kurulumu**
- Bulut tabanlı, GPU destekli bir çalışma ortamı sıfırdan kuruldu.
- Gerekli tüm bağımlılıklar ve yapılandırmalar bu ortama yüklendi.
- Ortam, süreç içinde bir kez sıfırlanmak zorunda kaldı; bu durumda kurulum adımları eksiksiz şekilde tekrarlandı.

**Veri hazırlama**
- Dönüştürücü script çalıştırılarak sentetik veri seti hedef sisteme uygun hale getirildi.
- Dönüşüm sonrası veri kaybı yaşanıp yaşanmadığı sayısal olarak doğrulandı.
- Hedef sistemin kendi veri hazırlama/bölme adımı çalıştırılarak eğitime hazır bir set oluşturuldu.

**Eğitim ve değerlendirme**
- Hazırlanan veriyle küçük ölçekli, kısa süreli bir deneme eğitimi koşturuldu.
- Eğitim çıktısı standart bir değerlendirme aracıyla ölçülüp bir metrik tablosuna dönüştürüldü.
- Elde edilen kanıt dosyaları (script, sonuç tabloları, kayıt dosyaları) kalıcı bir konumda saklandı.

**Belgeleme**
- Süreç boyunca atılan her adım, karşılaşılan sorun ve alınan karar ayrıntılı şekilde not edildi.
- Sistemi yeni bir alanda kullanırken fark edilen sürtünme noktaları, ayrı bir geri bildirim belgesi olarak yazıldı.

## Çözülen Sorunlar

- **Veri formatı uyumsuzluğu:** İki sistemin etiket formatları farklıydı; bu fark önce fark edilmeseydi veri sessizce kaybolabilirdi. Bir dönüştürme mantığı geliştirilerek giderildi.
- **Ortam/çalıştırma hataları:** Çalışma ortamı ilk kurulumda bazı modülleri doğru şekilde tanımadı; çalıştırma yöntemi düzeltilerek çözüldü.
- **Donanım kısıtı:** İlk denemede GPU tanımlı değildi, bu eğitim adımının başlamasını engelledi; ortam ayarı güncellenerek giderildi.
- **Veri bölme kısıtı:** Eğitim altyapısının bir güvenlik kuralı gereği, yalnızca sentetik veriyle çalışırken doğrulama grubu boş kalıyordu; bu adım için geçici ve sınırlı bir çözümle aşıldı.

## Karşılaşılan Zorluklar

- Sistemler arasında hazır bir entegrasyon ya da dokümantasyon bulunmuyordu; veri sözleşmesi (format, klasör düzeni, isimlendirme) baştan çıkarılmak zorunda kalındı.
- Bazı komut/parametre davranışları yeterince dokümante edilmemişti, doğru kullanım deneme-yanılma yoluyla bulundu.
- Kod deposuna doğrudan erişim kısıtı nedeniyle dosyalar alternatif bir yöntemle taşınmak zorunda kalındı.
- Eğitim çıktılarının saklandığı klasör yapısı beklenenden farklı çıktı, doğru konumu bulmak ek zaman aldı.

## Öğrenilenler

**Teknik**
- Farklı amaçlarla geliştirilmiş iki sistemi entegre ederken, önce veri sözleşmesinin (format, yapı, isimlendirme) titizlikle doğrulanması gerektiği.
- Bir sistemin sessizce veri reddedebileceği, bu yüzden entegrasyon adımlarında sayısal doğrulama (girdi/çıktı sayılarının karşılaştırılması) yapmanın önemi.
- Bulut tabanlı GPU ortamının kurulumu, yapılandırılması ve ortam değişikliklerinin (örn. donanım değişikliği) süreci nasıl etkileyebileceği.
- Eğitim sonrası doğrulama ve metrik çıkarma sürecinin uçtan uca nasıl işlediği.
- Bir sistemin çıktı/dosya konumlarının her zaman varsayılan/beklenen yerde olmayabileceği, doğrulanması gerektiği.

**Süreç ve yöntem**
- Erken aşama/bağlantı testlerinde elde edilen sonuçların gerçek performans göstergesi olarak yorumlanmaması, test koşullarının açıkça belirtilmesi gerektiği.
- Küçük, kontrollü bir denemeyle (az veri, kısa eğitim) önce "çalışıyor mu" sorusunu yanıtlamanın, büyük ölçekli denemelere göre çok daha hızlı geri bildirim sağladığı.
- Karşılaşılan sürtünmelerin sistematik olarak belgelenmesinin, hem ileriki adımlar hem de ilgili ekipler için değerli bir geri bildirim kaynağı oluşturduğu.
- Geçici/kısayol çözümlerin (örn. test amaçlı veri düzenlemeleri) net şekilde işaretlenmesinin, ileride yanlış yorumlanmalarının önüne geçtiği.

## Sonuç

İki sistem arasındaki uçtan uca bağlantı başarıyla kurulup doğrulandı: veri, biçim dönüşümünden geçerek eğitim altyapısına ulaştı, eğitim tamamlandı ve sonuçlar ölçülüp kayıt altına alındı. Bu aşamadaki sonuçlar bir performans kanıtı değil, bağlantının çalıştığının kanıtıdır.

## Sıradaki Adımlar

- Gerçek (sentetik olmayan) veriyle asıl performans ölçümünün yapılması.
- Bugün geçici olarak aşılan veri bölme/kısıt sorununun kalıcı ve doğru şekilde çözülmesi.
- Kod deposu erişim kısıtının giderilmesi.
- Haftalık ilerleme raporunun hazırlanması.
