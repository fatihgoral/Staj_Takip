# Gün 5 — Çalışma Özeti

**Tarih:** 7 Ağustos 2026
**Hedef:** Sentetik veri üretim hattı ile model eğitim altyapısını ilk kez uçtan uca birbirine bağlamak ve iki sistemin gerçekten birlikte çalışabildiğini kanıtlamak (Demo #1). Bu aşamada amaç yüksek model performansı elde etmek değil, iki bağımsız bileşen arasındaki veri akışının sorunsuz işlediğini doğrulamaktı.

## Genel Akış

Güne, iki sistemin birbirine nasıl bağlanacağını netleştirerek başlandı. Önce her iki tarafın veri beklentileri incelendi, aralarında bir uyumsuzluk olduğu görüldü ve bunu gideren bir ara katman (dönüştürücü) geliştirildi. Ardından bulut tabanlı bir çalışma ortamı kurulup dönüştürücü bu ortamda test edildi, veri seti hazırlandı, küçük ölçekli bir deneme eğitimi koşturuldu ve sonuçlar ölçülüp kayıt altına alındı. Son olarak süreç boyunca edinilen gözlemler ve karşılaşılan sorunlar yazılı hale getirildi.

## Yapılanlar

- İki farklı sistemi (sentetik veri üretimi + eğitim altyapısı) birbirine bağlayan bir dönüştürücü/köprü script'i sıfırdan tasarlanıp yazıldı.
- Google Colab üzerinde GPU destekli bir çalışma ortamı kuruldu ve gerekli bağımlılıklar yüklendi.
- Sentetik veri seti, köprü script'inden geçirilerek eğitim altyapısının beklediği formata ve klasör yapısına dönüştürüldü; dönüşüm sürecinde veri kaybı olup olmadığı ayrıca doğrulandı.
- Dönüştürülen veriyle eğitim altyapısının kendi veri hazırlama adımı çalıştırılarak eğitime hazır bir set oluşturuldu.
- Bu setle küçük ölçekli, kısa süreli bir deneme eğitimi koşturuldu.
- Eğitim sonucu, standart bir değerlendirme aracıyla metrik tablosuna dökülüp kanıt olarak saklandı.
- Süreç boyunca karşılaşılan teknik sorunlar, nedenleri ve önerilen çözümleriyle birlikte ayrı bir belgede raporlandı.

## Çözülen Sorunlar

- **Veri formatı uyumsuzluğu:** İki sistemin etiket formatları farklıydı; bu fark önce fark edilmeseydi veri sessizce kaybolabilirdi. Bir dönüştürme mantığı geliştirilerek giderildi.
- **Ortam/çalıştırma hataları:** Çalışma ortamı ilk kurulumda bazı modülleri doğru şekilde tanımadı; çalıştırma yöntemi düzeltilerek çözüldü.
- **Donanım kısıtı:** İlk denemede GPU tanımlı değildi, bu eğitim adımının başlamasını engelledi; ortam ayarı güncellenerek giderildi (bu değişiklik ortamın sıfırlanmasına yol açtı, önceki adımlar yeniden koşturuldu).
- **Veri bölme kısıtı:** Eğitim altyapısının bir güvenlik kuralı gereği, yalnızca sentetik veriyle çalışırken doğrulama grubu boş kalıyordu ve bu da eğitimin başlamasını engelliyordu; bu adım için geçici ve sınırlı bir çözümle aşıldı.

## Karşılaşılan Zorluklar

- Sistemler arasında hazır bir entegrasyon ya da dokümantasyon bulunmuyordu; veri sözleşmesi (format, klasör düzeni, isimlendirme) baştan çıkarılmak zorunda kalındı.
- Bazı komut/parametre davranışları yeterince dokümante edilmemişti, doğru kullanım deneme-yanılma yoluyla bulundu.
- Kod deposuna doğrudan erişim kısıtı nedeniyle dosyalar alternatif bir yöntemle taşınmak zorunda kalındı; bu durumun kalıcı olarak çözülmesi gerekiyor.
- Eğitim çıktılarının saklandığı klasör yapısı beklenenden farklı çıktı, doğru konumu bulmak ek zaman aldı.

## Öğrenilenler

- Farklı amaçlarla geliştirilmiş iki sistemi entegre ederken, önce veri sözleşmesinin (format, yapı, isimlendirme) titizlikle doğrulanması gerektiği.
- Bulut tabanlı GPU ortamının kurulumu, yapılandırılması ve ortam değişikliklerinin etkilerinin yönetilmesi.
- Eğitim sonrası doğrulama ve metrik çıkarma sürecinin uçtan uca nasıl işlediği.
- Erken aşama/bağlantı testlerinde elde edilen sonuçların gerçek performans göstergesi olarak yorumlanmaması, test koşullarının açıkça belirtilmesi gerektiği.
- Karşılaşılan sürtünmelerin sistematik olarak belgelenmesinin, hem ileriki adımlar hem de ilgili ekipler için değerli bir geri bildirim kaynağı oluşturduğu.

## Sonuç

İki sistem arasındaki uçtan uca bağlantı başarıyla kurulup doğrulandı: veri, biçim dönüşümünden geçerek eğitim altyapısına ulaştı, eğitim tamamlandı ve sonuçlar ölçülüp kayıt altına alındı. Bu aşamadaki sonuçlar bir performans kanıtı değil, bağlantının çalıştığının kanıtıdır.

## Sıradaki Adımlar

- Gerçek (sentetik olmayan) veriyle asıl performans ölçümünün yapılması.
- Bugün geçici olarak aşılan veri bölme/kısıt sorununun kalıcı ve doğru şekilde çözülmesi.
- Kod deposu erişim kısıtının giderilmesi.
- Haftalık ilerleme raporunun hazırlanması.
