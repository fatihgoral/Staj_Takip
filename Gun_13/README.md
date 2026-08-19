# Gün 13 — Yöntem seçimi ve ilk çalışan sürüm: SAM 2

**Tarih:** 19 Ağustos 2026

**Hedef:** Bir önceki gün elenen hazır çözümün yerine projeye temel olacak yöntemi araştırıp gerekçesiyle seçmek, bu yöntemi kendi kodumuzla çalışır hâle getirmek ve gerçek bir trafik videosu üzerinde "tıkla – segmente et – takip et" yeteneğini uçtan uca doğrulamak.

## Genel Akış

Gün, dünkü eleme kararının bıraktığı boşlukla başladı: hangi yolun yürünmeyeceği belliydi, hangisinin yürüneceği değil. Bu yüzden doğrudan koda geçilmedi; önce ihtiyacın ölçütleri yazıldı ve aday yaklaşımlar bu ölçütlere vurularak karşılaştırıldı. Karşılaştırma sonunda **SAM 2** seçildi ve seçim gerekçesi maddeler hâlinde kayda geçirildi.

Seçimle birlikte bir sınır da çizildi: dünkü dersin gereği olarak hazır bir arayüz projesi temel alınmadı, doğrudan resmi model üzerine ince ve bize ait bir katman yazıldı. Bu katman videodan kareleri çıkarıyor, kullanıcının tıkladığı noktayı modele istem olarak veriyor, oluşan maskeyi video boyunca yayıyor ve sonucu izlenebilir bir çıktıya dönüştürüyor. Hat, gerçek bir trafik videosu üzerinde çalıştırıldı ve temel işlev doğrulandı.

Günün asıl kısmı bundan sonra geldi: ilk çalışan sürümün ortaya çıkardığı sorunların teşhisi. Bunların bir kısmı **hata vermeden** ortaya çıkan, yani sistem "başarılı" derken aslında yanlış olan durumlardı ve ancak çıktı ayrıca incelendiği için yakalandı. Günün en değerli çıktısı çalışan sürümün kendisi değil, o sürümün gizlediği sessiz hataların yakalanmasıydı.

## Yapılanlar

### Yöntem araştırması ve seçim

- Önce ihtiyacın ölçütleri yazıldı, sonra adaylar bunlara vuruldu: kullanıcının işaret ettiği nesneyi alabilmek, önceden tanımlı bir sınıf listesine bağlı kalmamak, kutu değil piksel düzeyinde maske üretmek, video boyunca aynı nesneyi tutabilmek, bakımı süren güncel bir temel olmak ve elimizdeki donanımda çalışabilmek.
- Adaylar üç aile hâlinde ele alındı: klasik takip yöntemleri, önce-tespit-sonra-takip yaklaşımı ve etkileşimli segmentasyon yaklaşımı. Kritik ayrım şurada netleşti: ilk iki ailede "kullanıcının tıklaması" diye bir kavram yok — sistem önceden tanımlı bir sınıf listesini tarar, seçimi kullanıcı yapmaz. Bizim senaryomuzda seçimi yapan kullanıcı.
- Bu ölçütler karşısında SAM 2 seçildi: tek bir tıklamayı istem olarak kabul ediyor, sınıf listesine bağlı değil, piksel düzeyinde maske üretiyor ve video tarafında önceki kareleri hatırlayarak ilerlediği için takip ile segmentasyonu aynı modelde birleştiriyor.
- Seçimle birlikte bir sınır çizildi: hazır bir arayüz projesi benimsenmeyecek, doğrudan resmi model üzerine kendi katmanımız yazılacak — dün ödenen "başkasının eski kodunu ayakta tutma" maliyetini tekrar ödememek için.

### Kurulum ve uçtan uca hattın yazılması

- Projeye özel, yalıtılmış bir çalışma ortamı kuruldu ve GPU üzerinde çalıştığı doğrulandı; donanım kısıtı nedeniyle modelin orta boy varyantı tercih edildi.
- Resmi kaynak deposuna hiç dokunulmadı, bize ait kod ayrı tutuldu. Böylece yukarıdan gelecek bir güncelleme kendi değişikliklerimizle çakışmayacak.
- Çekirdek mantık arayüzden bağımsız yazıldı, arayüz yalnızca ince bir kabuk olarak bırakıldı: video yükle, bir kareyi gör, üzerine tıkla, sonucu izle.
- Kullanıcının ekranda tıkladığı nokta ile görüntünün gerçek çözünürlüğü aynı şey değil; aradaki dönüşüm ayrıca doğrulandı, çünkü hatalı olsaydı sistem hiç hata vermeden yanlış yeri segmente edecekti.

### Gerçek videoyla deneme ve teşhis

- Doğrulama, kontrollü bir örnek yerine doğrudan proje kapsamına uygun bir görüntüyle yapıldı. Tek tıklamayla seçilen araç segmentlendi ve video boyunca takip edildi; aynı anda iki aracın takibi de denendi. Sonuç yalnızca göz kararıyla değil, fiziksel tutarlılıkla da kontrol edildi: araçlar uzaklaştıkça maske alanları düzenli biçimde küçülüyordu.
- **Sessiz başarısızlık:** Çıktı videosunun yazılması hiçbir hata vermeden bozuk dosya üretiyordu — sistem "başarılı" diyor, dosya açılmıyordu. Yazma yolu değiştirilerek çözüldü.
- **Bellek birikmesi:** Arayüz her yenilendiğinde yeni bir çalışma oturumu açılıyor, öncekinin ayırdığı GPU belleği serbest kalmıyordu. Çalışma durumu tek bir paylaşılan yerde tutulacak şekilde düzenlendi.
- **Yanlış teşhis edilen yavaşlık:** Yavaşlığın nedeni tahmin edilmek yerine ölçüldü. Sonuç: kodda optimize edilecek bir darboğaz yok, dizüstü bilgisayar pil durumu nedeniyle işlemcileri sıkı bir güç tavanına kısıyordu ve aynı kod güç serbest kaldığında kat kat hızlı çalışıyordu. Buradan kalıcı bir kural çıktı: hız ölçümü almadan önce makinenin güç durumu kontrol edilecek.

### Doğruluk hatası ve sıradaki hedef

- Sistem bir testte "çok sayıda kare boyunca takip edildi" diyerek başarılı görünüyordu; ancak maske alanının zaman grafiğinde tek bir düzgün eğri yerine birden fazla tepe vardı. Çıktı kareleri üzerinden maskenin yörüngesi geriye doğru çıkarıldı: takip edilen araç kadrajın alt kenarından çıkıyor, model ise kısa süre sonra yeni araçların sahneye girdiği bölgede başka bir araca kilitleniyordu. Yani tek bir araç değil, sırayla birkaç farklı araç takip edilmişti.
- Çözüm olarak, bir nesne art arda birkaç kare bulunamazsa takibi kapatılıyor ve sonradan yeniden belirse bile yok sayılıyor; arayüz artık nesnenin kadrajı kaçıncı karede terk ettiğini söylüyor ve bildirilen metrik buna göre düzeltiliyor. Bunun önemi bugüne değil ileriye ait: sonraki aşamada maske bilinen bir referansla karşılaştırılarak ölçülecek ve bu hata varken ölçüm anlamsız olurdu.
- Sistem baştan sona elle test edildi — hazırlık, tıklayarak seçim, video boyunca takip ve arayüz yenilendikçe bellek davranışı — dört başlık da geçti.
- Geriye kalan tek somut şikâyet videonun akıcılığı değil **toplam bekleme süresi** oldu. Sürenin hangi aşamada harcandığı aşama aşama ölçüldü ve iyileştirme adımları en ucuzdan en pahalıya sıralandı: doğruluktan ödün vermeden elde edilebilecek kazanç tüketilmeden model tarafına dokunulmayacak.

## Çözülen Sorunlar

- **Yön belirsizliği:** Dünkü eleme kararının ardından açıkta kalan "hangi yöntem" sorusu, tahminle değil ölçütlü bir karşılaştırmayla kapatıldı ve gerekçesiyle yazıya geçirildi.
- **Sessiz bozuk çıktı:** Hata vermeden bozuk video üreten yazma adımı, farklı bir yola taşınarak düzeltildi.
- **Bellek birikmesi:** Arayüz yenilendikçe GPU belleğinin serbest kalmaması, çalışma durumunun paylaşılan tek bir yerde tutulmasıyla giderildi.
- **Yanlış teşhis edilen yavaşlık:** Koda atfedilen yavaşlığın gerçek nedeninin donanımın güç kısıtlaması olduğu ölçümle kanıtlandı; gereksiz bir optimizasyon turu böylece engellendi.
- **Kimlik sıçraması:** Takibin sessizce başka bir nesneye atlaması tespit edilip kapatıldı, bildirilen metrik gerçeği yansıtacak hâle getirildi.

## Karşılaşılan Zorluklar

- Günün en zor kısmı bozulan bir şeyi tamir etmek değil, **çalışıyor görünen bir şeyin aslında yanlış olduğunu fark etmekti.** Hem bozuk video hem de takibin nesne değiştirmesi, sistemin hiçbir hata üretmediği durumlardı.
- Performans ölçümlerinin bir turu, donanımın o anki güç durumu yüzünden boşa gitti; aynı kod, aynı makinede, farklı zamanlarda kat kat farklı sonuç verdi.
- Dizüstü GPU'sunun belleği, işlenebilecek kare sayısına doğrudan bir tavan koyuyor; bu, denenebilecek senaryoların kapsamını daralttı.
- İyileştirme talebinin ne anlama geldiği başlangıçta belirsizdi: videonun akıcılığı mı, sonucun gelmesi için beklenen süre mi? İkisi bambaşka işler; yanlış olanı optimize etmemek için koda dokunmadan önce bu ayrımın netleşmesi gerekti.

## Öğrenilenler

- Bir yöntemi seçmenin, o yöntemin iyi olduğunu göstermekten değil, ihtiyacın ölçütlerini önce yazıp adayları o ölçütlere vurmaktan geçtiği; gerekçe yazılı olmadığında seçimin ileride savunulamaz hâle geldiği.
- Hata mesajının yokluğunun bir başarı kanıtı olmadığı — bir sistemin çıktısı, sistemin kendi raporundan bağımsız olarak doğrulanmalı.
- Bir metriğin iyi görünmesinin işin doğru yapıldığı anlamına gelmediği; bugün "yüksek başarı" bildiren metrik aslında birbirinden bağımsız nesneleri tek nesne sanıyordu.
- Performans sorunlarında ölçüm koşulları kontrol edilmeden alınan sayıların tamamen yanıltıcı olabildiği; doğruluğu bozmayan iyileştirmeler tüketilmeden doğruluk bedeli olanlara geçilmemesi gerektiği.

## Sonuç

Gün, gerekçesi yazılı bir yöntem seçimi ve çalışan bir ilk sürümle kapandı. Dün elenen hazır çözümün yerine, ihtiyacın ölçütleri üzerinden SAM 2 seçildi; seçim, hazır bir arayüzü benimseme biçiminde değil resmi modelin üzerine kendi katmanımızı yazma biçiminde uygulandı — böylece dün ödenen maliyet tekrarlanmadı ve ortaya çıkan sistemin tamamı bizim kontrolümüzde kaldı.

Hedeflenen temel yetenek — videoda geçen bir araca tıklayınca onun segmentlenip video boyunca takip edilmesi — gerçek trafik görüntüsü üzerinde uçtan uca çalıştırıldı ve elle test edilerek doğrulandı. Günün asıl kazanımı ise bu değil, çalışan sürümün **gizlediği sessiz hataların** yakalanmasıydı: biri hata vermeden bozuk çıktı üretiyordu, diğeri takibi fark ettirmeden başka bir nesneye atlıyor ve olduğundan iyi bir metrik bildiriyordu. İkincisi, projenin ilerideki ölçüm aşamasının geçerliliğini doğrudan etkilediği için en kıymetli düzeltme oldu.

Kalan bekleme süresi aşama aşama ölçülerek nereye ne kadar zaman gittiği belirlendi ve iyileştirme adımları doğruluğu bozmayanlardan doğruluk bedeli olanlara doğru sıralandı. Bir sonraki adım bu sıralamanın en ucuz basamağından başlamak; işin ikinci bölümü olan sentetik veriyle birleştirme ve ölçüm aşaması ise geri bildirim alınana kadar bilinçli olarak beklemede tutuluyor.
