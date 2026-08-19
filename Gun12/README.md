# Gün 12 — Literatür taraması ve hazır çözüm denemesi: Track-Anything

**Tarih:** 18 Ağustos 2026

**Hedef:** Bir önceki gün karşılaşılan işlem süresi sorununa çözüm aramak üzere alandaki mevcut yaklaşımları taramak, öne çıkan hazır bir çözümü kurup gerçek videolar üzerinde çalıştırmak ve bu çözümün projede temel olarak kullanılıp kullanılamayacağına dair somut bir karar vermek.

## Genel Akış

Gün, bir önceki günden devreden işlem süresi sorunuyla başladı. Sorunu doğrudan kendi kodumuz üzerinde optimize etmeye çalışmak yerine, önce bir adım geri gidilip alandaki mevcut yaklaşımların bu problemi nasıl ele aldığı tarandı. Taramanın sonunda, hem soruna yaklaşımı hem de hazır bir kullanıcı arayüzü sunması nedeniyle öne çıkan bir açık kaynak proje (Track-Anything) seçildi ve kurulumuna geçildi.

Kurulum beklenenden çok daha uzun sürdü: proje birkaç yıl önce yayınlanmış olduğundan, bağımlılıkları güncel çalışma ortamıyla uyuşmuyordu. Bunlar tek tek teşhis edilip çözüldü ve sistem çalışır hâle getirildi. Ardından örnek videolar ve proje kapsamına yakın gerçek trafik videoları üzerinde denemeler yapıldı; sistemin temel işlevi doğrulandı.

Ancak gün sonunda yapılan değerlendirmede, bu projeyi temel almanın uzun vadede sürdürülebilir olmadığı sonucuna varıldı ve **bu yoldan vazgeçme kararı** alındı. Günün en önemli çıktısı çalışan bir kurulum değil, bu karardı.

## Yapılanlar

### Literatür ve mevcut çözüm taraması

- Bir önceki gün karşılaşılan işlem süresi sorununun, yalnızca bizim uygulamamıza özgü bir problem mi yoksa alanın bilinen bir darboğazı mı olduğunu anlamak için sistematik bir tarama yapıldı.
- Tarama, problemin farklı çözüm ailelerini kapsayacak şekilde geniş tutuldu: klasik, hafif ve hızlı çalışan yöntemlerden, daha ağır ama daha isabetli çalışan güncel yöntemlere kadar bir yelpaze incelendi. Bu yöntemlerin birbirini nasıl takip ettiği, hangi eksiği kapatmak için ortaya çıktıkları ve hangi senaryolarda birbirlerinden ayrıştıkları çıkarıldı.
- Akademik yayınların yanında, bu yöntemleri fiilen çalışır hâle getirmiş açık kaynak projeler de tarandı — çünkü bir yöntemin makalede iyi görünmesiyle, kendi ortamımızda çalıştırılabilir olması ayrı şeyler.
- Taramanın sonucunda, işlem süresi sorununun büyük ölçüde **bilinen bir denge (trade-off)** olduğu görüldü: hız ile isabet arasında doğrudan bir takas var ve alandaki yöntemler esasen bu takasın farklı noktalarında konumlanıyor. Bu, sorunun "yanlış bir şey yapıyoruz" değil, "yanlış noktayı seçmişiz" türünde olduğunu gösterdi.
- Ayrıca tarama, projenin ilerleyen aşamaları için de bir harita çıkardı: hangi yaklaşımların denenmeye değer olduğu, hangilerinin bizim kısıtlarımız altında baştan elenebileceği ve karşılaştırma yaparken hangi ölçütlere bakılması gerektiği netleşti. Bu harita, günün geri kalanındaki seçimlerin dayanağı oldu.

### Hazır çözümün kurulumu

- Taramada öne çıkan projelerden biri (Track-Anything) seçildi. Seçim gerekçesi: hazır bir kullanıcı arayüzü sunması, kurulumunun özel derleme gerektirmemesi ve ele aldığı problemin bizim ihtiyacımıza yakın olması.
- Kurulum sırasında, projenin yayınlandığı dönemin kütüphane sürümleriyle bugünkü ortam arasında çok sayıda uyumsuzluk çıktı. Bunlar tek tek teşhis edilip çözüldü — bazıları sürüm sabitlemesiyle, bazıları kodun ilgili satırlarına küçük düzeltmeler yapılarak.
- Uyumsuzlukların bir kısmı zincirleme ilerledi: bir bağımlılık düzeltildiğinde bir sonraki ortaya çıktı. Her adımda düzeltme ayrı ayrı not edildi, böylece süreç tekrarlanabilir hâle getirildi.
- Projenin kullanmadığımız bir alt bileşeni, gereksiz yere ağır bir bağımlılık istediği için devre dışı bırakıldı — kapsam dışı bir parçayı çalıştırmak için zaman harcamamak adına bilinçli bir tercih.

### Videolar üzerinde deneme

- Kurulum tamamlandıktan sonra sistem önce basit bir örnek video ile doğrulandı; temel işlevin (bir nesneyi seçip video boyunca takip etme) doğru çalıştığı görüldü.
- Ardından proje kapsamına daha yakın, gerçek trafik görüntüleri üzerinde denemeler yapıldı. Bu denemeler, sistemin davranışını kontrollü bir örnekte değil, gerçek sahne karmaşıklığında görmek açısından belirleyici oldu.
- Denemeler sırasında, donanım kısıtları nedeniyle görüntü çözünürlüğü ve video uzunluğu konusunda pratik sınırlar olduğu tespit edildi; çalışma bu sınırlar içinde yapılandırıldı.
- Sonuçlar incelendiğinde takip işlevinin doğru çalıştığı, üretilen çıktının beklendiği gibi olduğu doğrulandı. Buna karşılık arayüzün kendi önizleme katmanında, yine sürüm eskiliğinden kaynaklanan bir görüntüleme sorunu vardı — çıktı dosyası sağlamdı, sadece arayüz içinde gösterilemiyordu. Bu sorun bilinçli olarak kovalanmadı.

### Değerlendirme ve karar

- Gün sonunda, elde edilen çalışan kurulum objektif biçimde değerlendirildi ve şu sonuca varıldı: proje, üzerine inşa edilecek bir **temel** değil, olsa olsa bir **referans** olabilir.
- Gerekçeler: (1) proje güncel değil ve bakımı sürmüyor, bu yüzden her yeni adımda benzer uyumsuzluk maliyetleri çıkmaya devam edecek; (2) sunduğu yetenek, projenin ihtiyaç duyduğu kapsamın yalnızca bir bölümünü karşılıyor; (3) süre sorununa aradığımız çözümü sağlamıyor.
- Bu nedenle proje temel alınmaktan vazgeçildi. Kurulum, kanıt ve karşılaştırma referansı olarak dokunulmadan, ayrı ve izole bir çalışma alanında dondurulup saklandı. Asıl geliştirme için ayrı, temiz bir çalışma ortamı hazırlandı.
- Bu karar bir geri adım değil, bir eleme: hangi yolun yürünmeyeceğinin **denenerek** öğrenilmesi, tahmine dayalı olarak atlanmasından çok daha güvenilir bir bilgi üretti.

## Çözülen Sorunlar

- **Sürüm uyumsuzlukları:** Eski tarihli projenin çok sayıda bağımlılık çakışması, sürüm sabitlemeleri ve küçük kod düzeltmeleriyle tek tek giderildi; sistem çalışır hâle getirildi.
- **Gereksiz ağır bağımlılık:** Proje kapsamı dışındaki bir alt bileşen devre dışı bırakılarak, kurulumun gereksiz yere karmaşıklaşması önlendi.
- **Donanım kısıtı:** Bellek sınırları nedeniyle oluşan tıkanma, çözünürlük ve video uzunluğu ayarlanarak aşıldı.
- **Yön belirsizliği:** Bir önceki günden gelen süre sorununun kaynağı, literatür taraması sayesinde "hatalı uygulama" değil "bilinen bir hız–isabet takası" olarak netleşti.

## Karşılaşılan Zorluklar

- Eski tarihli bir projeyi güncel ortama taşımak, beklenenden belirgin şekilde uzun sürdü. Uyumsuzluklar zincirleme ortaya çıktığı için, her düzeltme bir sonrakini görünür kıldı — toplam süreyi baştan kestirmek mümkün olmadı.
- Hazır bir çözümün "çalışıyor" olması ile "kullanılabilir" olması arasındaki farkı görmek, ancak kurulum tamamlanıp gerçek veriyle denendikten sonra mümkün oldu. Bu, geriye dönük bakıldığında kaçınılmaz bir maliyetti.
- Sınırlı donanım kaynağı, deneme senaryolarının kapsamını daralttı; bazı ayarlar deneme-yanılma ile bulundu.
- Kurulum sırasında ortaya çıkan her sorunu kovalamak yerine, hangilerinin projeye değer katacağını ayırt etmek gerekti — kapsam dışı kalan bir arayüz hatası bilinçli olarak çözülmeden bırakıldı.

## Öğrenilenler

- Bir alandaki problemin çoğu zaman zaten bilinen bir denge (hız–isabet takası) etrafında şekillendiği; bu dengeyi görmeden yapılan optimizasyon çabasının yönsüz kalabileceği.
- Bakımı sürmeyen projelerin, ilk kurulumda ödenen maliyetle bitmediği — sonraki her adımda benzer bir maliyet çıkarmaya devam ettiği.
- Bir sistemin gerçek davranışının, kontrollü örnek verilerde değil ancak gerçek ve karmaşık veride ortaya çıktığı.
- Donanım kısıtlarının, yöntem seçimini teoriden bağımsız biçimde belirleyebildiği.

## Sonuç

Gün, çalışan bir kurulum ve net bir **eleme kararıyla** kapandı. Bir önceki günden devreden işlem süresi sorunu, literatür taraması sayesinde rastgele bir hata olmaktan çıkıp alanın bilinen bir tasarım takası olarak konumlandırıldı; bu, sonraki adımlar için doğrudan yön verici oldu. Öne çıkan hazır çözüm kurulup gerçek trafik videoları üzerinde çalıştırıldı ve temel işlevi doğrulandı — ancak güncelliğini yitirmiş olması ve ihtiyacın yalnızca bir kısmını karşılaması nedeniyle projeye temel olmaktan çıkarıldı, referans olarak saklandı.

Günün asıl kazanımı, bir şeyin **çalıştırılmış** olması değil, geri kalan süre içinde **neyin yazılmayacağının** belirlenmesiydi. Taramadan çıkan yol haritası, bundan sonraki geliştirmenin hangi yaklaşım üzerine kurulacağını ve başarının hangi ölçütle değerlendirileceğini netleştirdi; ertesi gün doğrudan bu temel üzerine geliştirmeye başlanacak.
