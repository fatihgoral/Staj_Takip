# Gün 14 — Sentetik veriye geçiş hazırlığı

**Tarih:** 20 Ağustos 2026

**Hedef:** Trafik videosunda çalıştığı doğrulanan takip sistemini, projenin kendi
sentetik videolarıyla kullanılabilir hale getirmek; gelen veri setinin yapısını ve
etiket formatını inceleyerek planlanan doğruluk ölçümünün uygulanabilirliğini
araştırmak.

## Genel Akış

Gün, sentetik veri setinin teslim alınması ve yapısının incelenmesiyle başladı;
bu sırada proje notlarındaki bir varsayımın yanlış olduğu ortaya çıktı ve ölçüm
planı değişti. Ardından veri setinin yanında gelen üstveri dosyalarının
beklenenden zengin olduğu fark edildi, bu da ölçüm için ikinci bir referans kaynağı
kazandırdı. Günün asıl bulgusu sonda geldi: sentetik verinin sahne kurgusu ile
yöntemin çalışma koşulları arasında yapısal bir uyumsuzluk var — ve bu uyumsuzluk,
aslında projenin cevaplaması gereken asıl soruyu tanımlıyor. Uygulamalı test
yapılmadı; gün, testin neyi ölçeceğinin tanımlanmasıyla kapandı. Dikkat çeken
nokta, işe hiç başlanmadan önce iki ayrı engelin yakalanmış olmasıydı — ikisi de
kod yazıldıktan sonra fark edilseydi, sistem hata vermeden sessizce yanlış sonuç
üretecekti.

## Yapılanlar

* **Veri incelemesi:** Sentetik video seti için görüntü + etiket + üstveri
  dosyalarından oluşan tam bir set indirildi, klasör yapısı belgelendi. Yapının,
  projenin daha önceki üretim aşamasındakinden farklı olduğu görüldü; diğer iki
  video için etiketler henüz indirilemedi, bu eksik not edildi.
* **Etiket formatı düzeltildi:** Notlarda etiketlerin piksel düzeyinde şekil
  bilgisi içerdiği yazıyordu; dosya açılınca bunun basit çerçeve (kutu) bilgisi
  olduğu görüldü. Yani sistemin ürettiği ayrıntılı çıktı ile referans etiket aynı
  türden değil. Ölçüm iki aşamalı hale getirildi — önce basit karşılaştırma,
  ayrıntılı şekil ölçümü sonraya.
* **Üstveri dosyalarının keşfi:** Veri setinin yanında gelen dosyalarda her kare
  için nesnenin konumu, boyutu, görünürlüğü ve sahneyi terk ettiği kare kayıtlı.
  Bu, hem etiketlerden bağımsız bir referans kaynağı, hem de önceki gün eklenen
  "nesne kadrajı terk etti" mantığını sınama imkânı sağlıyor.
* **Ölçek araştırması:** Yöntem her kareyi içeride sabit bir boyuta küçültüyor;
  sentetik videoların çözünürlüğü yüksek olduğu için nesne yaklaşık üçte bir
  oranında küçülerek sisteme ulaşıyor. Trafik videosunda takip edilebilen en
  küçük nesne referans alındı: sentetik videoda nesne sahnenin başında bu eşiğin
  çok altında, sonunda ise rahatlıkla üstünde — video boyunca yaklaşık yirmi kat
  büyüyor. İkinci videoda da aynı örüntü var, yani üretim hattının tasarımı.
* **Olası çözümün araştırılması:** Karenin tamamını vermek yerine ilgi bölgesinden
  kırpma yapılabileceği görüldü; bu durumda küçültme yerine büyütme olur. Ancak bu
  değişikliğin daha önce bir kez tuzağa düşülen koordinat dönüşümü mantığına
  dokunduğu tespit edildi. Fikir kaydedildi, bugün uygulanmadı.
* **Veri tuzağı tespiti:** Son görünür karenin etiketi boş; ölçüme katılsaydı
  sisteme sahte bir hata yazılacaktı, kapsam dışı bırakıldı.

## Çözülen Sorunlar

* **Yanlış varsayım:** Etiket formatına dair eski ve hatalı bir kayıt, ham dosya
  incelenerek düzeltildi; ölçüm planı buna göre yeniden kurgulandı.
* **Gizli veri tuzağı:** Son karenin etiketsiz olması tespit edilip ölçüm kapsamı
  dışına alındı.
* **Belirsiz ölçüm hedefi:** "Sistem sentetik veride çalışıyor mu?" gibi açık uçlu
  bir soru, ölçülebilir bir eşik sorusuna dönüştürüldü.

## Karşılaşılan Zorluklar

* Veri bulut depolamada; sadece bir video için tam set indirilebildi, bu da
  karşılaştırmalı ölçümün kapsamını şimdilik daraltıyor.
* Proje notlarındaki bilginin eskimiş olması işin yönünü değiştirdi; veri üretim
  hattı geliştikçe formatın da değişebileceği görüldü.
* Ölçek sorunu net bir hata belirtisi vermiyor; veriye ve yöntemin iç işleyişine
  ayrı ayrı bakıp yan yana koyunca ortaya çıktı.
* Uygulamalı test bugün yapılamadı.

## Öğrenilenler

* Yüksek çözünürlüklü veri tek başına avantaj değil; yöntemin girdiyi içeride
  nasıl işlediği beklenmedik bir sınır yaratabiliyor.
* Bir sistemin başarısını, üzerinde çalıştığı en zor koşulla tanımlamak gerekiyor;
  ortalama koşulda çalışıyor olması yeterli bilgi vermiyor.
* Kritik bir adıma başlamadan önce ham veriyi bizzat açıp doğrulamak, kodu
  yazdıktan sonra hata aramaktan çok daha ucuz.
* Beklentiyle uyuşmayan sonuç da bir bulgudur; katkı bazen "çalıştı" demek değil,
  sınırın nerede olduğunu ölçmektir.

## Sonuç

Sentetik veriye geçişin zemini hazırlandı: veri yapısı çıkarıldı, hatalı bir
varsayım düzeltildi, ölçüm planı yeniden kurgulandı ve ölçümü bozacak bir tuzak
önceden yakalandı. En önemlisi, sentetik veri ile kullanılan yöntem arasındaki
yapısal uyumsuzluk tespit edildi — nesne sahnenin başında çok küçük, sonunda çok
büyük — ve bu, projenin cevaplayabileceği somut bir araştırma sorusuna
dönüştürüldü: **engel kaç piksele düşene kadar takip edilebiliyor ve bu ne kadar
erken uyarı süresine karşılık geliyor?** Böylece ertesi günün ilk işi, açık uçlu
bir deneme değil, sonucu şimdiden anlamlı olan bir ölçüm haline geldi.

## Sıradaki Adımlar

* Sentetik videoda takibin çalışıp çalışmadığının test edilmesi; sonraki her adım
  bunun çıktısına bağlı.
* Nesnenin büyük olduğu geç karelerden başlanarak takibin koptuğu kırılma
  noktasının aranması.
* Sistem çıktısını referans etiketle karşılaştıran ölçüm aracının yazılması ve
  kare kare doğruluk grafiğinin üretilmesi.
* Gerekirse ilgi bölgesinden kırpma yönteminin eklenmesi, trafik videosuyla
  sınanması.
