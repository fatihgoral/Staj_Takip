# Gün 5 — İlk ölçüm: temel model sonucu (baseline) elde edildi

**Tarih:** 14 Ağustos 2026  
**Hedef:** Hazırlanan veri setiyle ilk model eğitimini gerçekleştirmek, projeye özel bir değerlendirme aracı geliştirmek ve tek bir eğitime güvenmeden birden fazla tekrarla ilk referans (baseline) sonucunu elde etmek — haftanın kapanışı ve Demo #2 için.

## Genel Akış
Gün, bulut tabanlı bir eğitim ortamının kurulmasıyla başladı. Eğitim ilerlerken veri setine daha yakından bakıldığında önemli bir kalite gözlemi yapıldı ve veri seti buna göre yeniden düzenlendi. Ardından, standart ölçütlerin ötesinde projeye özel soruları cevaplayacak bir değerlendirme aracı yazıldı; bu araçta karşılaşılan bir sorun sistematik olarak teşhis edilip çözüldü. Son olarak, tek bir sonuca güvenmemek için model üç farklı rastgele başlangıç koşuluyla baştan sona eğitilip ölçüldü, hafta bu sonuçların belgelenmesiyle kapandı. Günün genelinde dikkat çeken nokta, hiçbir adımın tek seferde sorunsuz ilerlememesiydi — her aşamada bir engelle karşılaşıldı, ama her biri gün bitmeden çözüldü.

## Yapılanlar

### Ortam kurulumu
* Bulut tabanlı, GPU destekli bir çalışma ortamı sıfırdan kuruldu, gerekli tüm bağımlılıklar yüklendi.
* Eğitim script'ine, aynı deneyi farklı rastgele başlangıç koşullarıyla (seed) tekrarlayabilme özelliği eklendi — tek bir şans eseri iyi sonucu gerçek başarıyla karıştırmamak için gerekliydi. Bu özellik, günün ilerleyen saatlerinde 3 tekrarlı ölçümün teknik altyapısını oluşturdu.

### Veri yerleşimi
* Veri setinin bulunduğu uzak depolama alanından doğrudan okuma denendiğinde ciddi bir yavaşlık fark edildi — çok sayıda küçük dosyanın tek tek uzaktan okunması darboğaz yarattı.
* Veri seti bir kerede, toplu halde çalışma ortamının kendi yerel diskine kopyalanarak bu sorun giderildi. Bu değişiklik sonrasında eğitim süreci beklenen hızda ilerlemeye başladı.

### Veri kalitesi incelemesi ve düzeltme
* Eğitim ilerlerken veri setine daha yakından bakıldığında, üretilen on binlerce görselin büyük kısmının, aynı klibin birbirine çok yakın ardışık karelerinden oluştuğu görüldü — görsel sayısı yüksekti ama gerçek sahne/senaryo çeşitliliği çok daha sınırlıydı.
* Bu gözlem üzerine, her klipten daha seyrek aralıklarla kare alınacak şekilde veri seti yeniden düzenlendi (yaklaşık 5 kat küçültüldü). Küçültmeden önce birkaç örnek elle incelenerek nesnelerin sahnede yeterince uzun süre kaldığı, bilgi kaybı olmayacağı doğrulandı. Bu doğrulama adımı, küçültme kararının rastgele değil, gözleme dayalı verildiğini göstermesi açısından önemliydi.

### Değerlendirme aracı geliştirme
* Standart başarı ölçütlerinin ötesinde, projeye özel soruları (küçük nesneleri ne kadar iyi buluyor, az yanlış alarmla çalışırken ne kadar güvenilir, uzaktaki nesnelerde başarım nasıl değişiyor) cevaplayacak özel bir değerlendirme aracı sıfırdan yazıldı.
* Araç ilk çalıştırmalarda, hiçbir hata mesajı vermeden ortasında bir yerde sessizce duruyordu. Sistematik bir teşhis yöntemiyle (sorunu küçük veri parçalarıyla adım adım büyüterek) kök sebep bulundu: çok sayıda görsel art arda işlenirken bellek alanının yeterince hızlı boşaltılmaması.
* Araç, görselleri küçük gruplar hâlinde işleyip her grup sonrası belleği zorla temizleyecek şekilde yeniden yazıldı; bu sayede sorun kalıcı olarak ortadan kalktı. Düzeltme sonrası araç, tüm veri üzerinde kesintisiz şekilde tamamlandı.

### Eğitim ve ölçüm
* Düzeltilmiş veri seti ve iyileştirilmiş değerlendirme aracıyla, model 3 farklı rastgele başlangıç koşuluyla baştan sona eğitilip her biri ayrı ayrı ölçüldü.
* Bir eğitim koşusu ortasında bir bağlantı kopmasına uğradı; düzenli aralıklarla kaydedilen ara kayıt (checkpoint) sayesinde eğitim kaldığı yerden sürdürüldü, kayıp yalnızca birkaç dakikalık ilerlemeyle sınırlı kaldı. Bu, düzenli ara kayıt almanın pratikte ne kadar değerli olduğunu somut biçimde gösterdi.

### Belgeleme ve kapanış
* Elde edilen sonuçlar, kullanılan yapılandırma ve hafta boyunca alınan kararlar ilgili proje dokümanlarına işlendi.
* Haftalık özet rapor hazırlandı.
* Gün 1'de hatalı girdiyle açılmış, artık kullanılmayan çalışma alanı temizlendi.

## Çözülen Sorunlar
* **Yavaş veri erişimi:** Uzak depolamadan okuma kaynaklı yavaşlık, veriyi yerel diske kopyalayarak giderildi.
* **Veri verimsizliği:** Veri setindeki aşırı kare tekrarı, örnekleme sıklığı ayarlanarak giderildi (~5 kat küçültme).
* **Sessiz çökme:** Değerlendirme aracındaki teşhisi zor çökme, kademeli bellek birikimi kök sebebi bulunup gruplu işleme yöntemiyle kalıcı olarak çözüldü.
* **Kesintiye uğrayan eğitim:** Ara kayıttan devam ettirilerek kayıpsız tamamlandı.

## Karşılaşılan Zorluklar
* Sessiz çökme türündeki hatalar, normal hata mesajı veren sorunlara göre çok daha zor teşhis edildi; sistemli, adım adım daraltma yöntemi gerekti.
* Veri kalitesi sorunu (kare tekrarı) eğitim başladıktan sonra fark edildi, bu bir miktar zaman kaybına yol açtı — ama büyük ölçekli sonuçları etkilemeden, erken aşamada yakalanabildi.
* Bulut ortamının bazı davranışları (depolama erişim hızı, yapılandırma dosyası yol çözümlemesi) yeterince belirgin değildi, doğru kullanım deneme-yanılma yoluyla bulundu.

## Öğrenilenler

### Teknik
* Model için önemli olanın ham görsel sayısı değil, gerçek sahne/senaryo çeşitliliği olduğu; büyük bir veri seti bile düşük çeşitlilikte olabiliyor.
* "Hiçbir hata vermeden duran" bir sistem, hata veren bir sistemden daha zor teşhis edilebiliyor — sistematik, adım adım daraltma yöntemi böyle durumlarda en güvenilir yol.
* Bulut ortamlarında veri konumunun (uzak depolama mı, yerel disk mi) performansı ne kadar etkileyebileceği ilk elden görüldü.
* Düzenli ara kayıt almanın, uzun süren işlemlerdeki kesintileri zaman kaybından öteye geçirmediği.

### Süreç ve yöntem
* Büyük ölçekli bir eğitim başlatmadan önce veri kalitesini küçük örneklerle kontrol etmenin, hatanın büyük ölçekte tekrarlanmasını önlediği.
* Tek bir ölçüme güvenmemek (birden fazla tekrarla ölçüm almak), sonucun tesadüf mü yoksa gerçek bir örüntü mü olduğunu ayırt etmeyi sağlıyor.
* Karşılaşılan sürtünme noktalarının sistematik olarak not edilmesinin, hem bu hafta hem gelecek haftalar için değerli bir referans oluşturduğu.

## Sonuç
Hafta, projenin ilk tekrarlanabilir referans sonucuyla (baseline) kapandı — üç bağımsız eğitim tekrarının ortalaması olarak genel tespit başarımı yaklaşık **%80** (daha katı bir ölçütte yaklaşık **%63**), küçük nesnelerde ise yaklaşık **%43** seviyesinde ölçüldü. Üç tekrar arasındaki sonuçların birbirine yakın çıkması, modelin kararlı/tutarlı öğrendiğini gösterdi. Ayrıca bir engel kategorisinin diğerlerine göre sistematik olarak daha zayıf kaldığı fark edildi — bu, sonraki hafta için somut bir iyileştirme hedefi olarak not edildi. Bu üç günlük çalışma, sadece bir sayı üretmekle kalmadı; aynı zamanda projenin geri kalanında güvenle kullanılabilecek bir veri hazırlama ve ölçüm sürecinin de temelini attı.

## Sıradaki Adımlar
- [ ] Sistematik olarak zayıf kalan engel kategorisinin kök sebebinin araştırılması.
- [ ] Düşük yanlış-alarm noktasındaki sonuç tutarlılığının artırılması.
- [ ] Bir sonraki haftanın iyileştirme hedeflerinin ilgili sorumluyla netleştirilmesi.
