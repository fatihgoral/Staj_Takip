# Gün 9 — Veri setinin kurulması ve dönüştürme aracının yeniden yazılması

**Tarih:** 13 Ağustos 2026  
**Hedef:** Ham çıktıyı model eğitimine hazır bir veri setine dönüştürmek ve split kararına uygun şekilde veri setini oluşturmak.

## Genel Akış
Güne, ham çıktıyı eğitime hazır formata çeviren aracın yeniden yazılmasıyla başlandı. Bu tercih tesadüfi değildi: bir önceki gün fark edilen gizli bölme kuralı ve aracın gerçek çıktı yapısıyla uyuşmayan varsayımları, aracı "olduğu gibi" kullanmayı riskli hâle getirmişti. Araç yeniden yazıldıktan sonra önce küçük ölçekte, ardından tüm veri üzerinde test edildi. Ardından dönüşüm sonrası sızıntı (leakage) kontrolü gerçekleştirilerek günün ana hedefi olan "eğitime hazır ve güvenilir bir veri seti" elde edilmiş oldu.

## Yapılanlar

### Dönüştürme aracının yeniden yazılması
Ham üretim çıktısını model eğitiminin beklediği görsel + etiket formatına çeviren araç iki nedenle sıfırdan yazıldı:
- Eski hali, farklı bir amaç için tasarlanmış ve otomatik/gizli bir bölme kuralına bağımlıydı; bu kural, yeni split kararını fark ettirmeden bozabilirdi.
- Aracın varsaydığı çıktı yapısı (ayrı kare görselleri klasörü) ile üretim hattının gerçek çıktısı (video dosyası + ayrı etiket dosyaları) birbirini tutmuyordu.

Yeni araç, video dosyalarını kare kare okuyup her kareyi ilgili etiketiyle eşleştirecek ve karara uygun eğitim/doğrulama klasörlerini doğrudan kendisi yazacak şekilde tasarlandı. Bu tasarım tercihi, aracın gelecekte tekrar çalıştırılması gerektiğinde manuel bir adıma ihtiyaç duymadan aynı sonucu üretebilmesini sağlamayı amaçladı.

Önce küçük bir örnek veriyle test edildi; sorunsuz çalıştığı görüldükten sonra elimizdeki tüm üretilmiş veriye (yaklaşık 200 klip) uygulandı. Bu kademeli test yaklaşımı, olası bir hatanın küçük ölçekte yakalanıp büyük ölçekte tekrarlanmasının önüne geçmek için bilinçli olarak izlendi.

### Sızıntı kontrolü
Dönüşüm sonrası, eğitim klasöründe doğrulama kaynağından, doğrulama klasöründe de eğitim kaynağından hiçbir dosya bulunmadığı sayısal olarak doğrulandı — bu, modelin gerçek genelleme yerine "ezber" yapma riskini en aza indirdi. Bu kontrolün otomatik ve sayısal yapılması, "doğru klasöre yazdık" gibi öznel bir varsayıma dayanmak yerine somut bir kanıt sunması açısından önemliydi.

### Belgeleme
Alınan yöntem ve bölme kararları, gerekçeleriyle birlikte ilgili proje dokümanına işlendi. Bu belgeleme adımı, ileride "neden bu yöntem seçildi" sorusuna geri dönüldüğünde referans olması amacıyla o gün içinde tamamlandı.

## Çözülen Sorunlar
- **Gizli bölme kuralı riski:** Eski dönüştürme aracının bağımlı olduğu dış bir kural, dokümantasyon taranarak fark edildi ve yeni karara karışmadan devre dışı bırakıldı.
- **Format uyumsuzluğu:** Aracın varsaydığı çıktı yapısı ile gerçek çıktı arasındaki fark, video okuma mantığı eklenerek giderildi. Bu düzeltme sayesinde araç, üretim hattının gerçekte ürettiği formatla uyumlu hâle geldi ve manuel bir ara dönüşüm adımına gerek kalmadı.

## Karşılaşılan Zorluklar
- Eski aracın bağımlı olduğu dış kural, kod okumasında ilk bakışta göze çarpmıyordu; ilgili dokümantasyonun ayrıca taranması gerekti. Bu, sadece koda bakmanın bazen yeterli olmadığını, ilişkili dokümantasyonun da tarama kapsamına alınması gerektiğini gösterdi.

## Öğrenilenler

### Teknik
- Bir yardımcı aracı "olduğu gibi" tekrar kullanmadan önce, o aracın gizli/varsayılan kurallarının yeni senaryoyla çelişip çelişmediğini kontrol etmek gerekiyor.
- Sızıntı kontrolü (train/val ayrımının gerçekten temiz olduğunun sayısal olarak teyit edilmesi), "doğru klasöre yazdık" demekten çok daha güvenilir bir doğrulama yöntemi.

### Süreç ve yöntem
- Yeni bir aracı önce küçük örnekle, sonra tam veriyle test etmek, büyük ölçekli bir hatayı erken yakalamayı sağlıyor. Bu yöntem, zaman kaybı gibi görünse de aslında büyük ölçekli bir hatanın maliyetini önceden düşürüyor.

## Sonuç
Gün sonunda elimizde: 
1. Baştan yazılmış ve tam veriyle test edilmiş bir dönüştürme aracı
2. Bu araçtan üretilmiş, sızıntı kontrolünden geçmiş, on binlerce görsel/etiketten oluşan, eğitime hazır bir veri seti vardı. 

Bu veri seti, ertesi günkü model eğitiminin doğrudan girdisini oluşturdu.

## Sıradaki Adımlar
- [ ] Bulut tabanlı bir eğitim ortamının kurulması ve ilk model eğitiminin başlatılması.
- [ ] Model performansını ölçecek özel bir değerlendirme aracının hazırlanması.
