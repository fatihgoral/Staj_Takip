# Gün 14 — Sentetik veriye geçiş hazırlığı

**Tarih:** 20 Ağustos 2026

**Hedef:** Trafik videosunda çalıştığı doğrulanan takip sistemini, projenin kendi
sentetik videolarıyla kullanılabilir hale getirmek.

## Genel Akış

Gün, sentetik veri setinin teslim alınması ve yapısının incelenmesiyle başladı;
bu sırada proje notlarındaki bir varsayımın yanlış olduğu ortaya çıktı ve ölçüm
planı değişti. Günün asıl bulgusu sonda geldi: sentetik verinin sahne kurgusu ile
yöntemin çalışma koşulları arasında yapısal bir uyumsuzluk var testin neyi ölçeceğinin
tanımlanmasıyla gün kapandı.

## Yapılanlar

* **Veri incelemesi:** Sentetik video seti için görüntü + etiket + üstveri
  dosyalarından oluşan tam bir set indirildi, klasör yapısı belgelendi.
* **Etiket formatı düzeltildi:** Notlarda etiketlerin piksel düzeyinde şekil
  bilgisi içerdiği yazıyordu; dosya açılınca bunun basit çerçeve (kutu) bilgisi
  olduğu görüldü. Ölçüm iki aşamalı hale getirildi — önce basit karşılaştırma,
  ayrıntılı şekil ölçümü sonraya.
* **Ölçek araştırması:** Yöntem her kareyi içeride sabit bir boyuta küçültüyor;
  sentetik videoların çözünürlüğü yüksek olduğu için nesne yaklaşık üçte bir
  oranında küçülerek sisteme ulaşıyor. Trafik videosunda takip edilebilen en
  küçük nesne referans alındı: sentetik videoda nesne sahnenin başında bu eşiğin
  çok altında, sonunda ise rahatlıkla üstünde — video boyunca yaklaşık yirmi kat
  büyüyor. İkinci videoda da aynı örüntü var, yani üretim hattının tasarımı.
* **Veri tuzağı tespiti:** Son görünür karenin etiketi boş; ölçüme katılsaydı
  sisteme sahte bir hata yazılacaktı, kapsam dışı bırakıldı.

## Karşılaşılan Zorluklar

* Proje notlarındaki bilginin eskimiş olması işin yönünü değiştirdi.
* Ölçek sorunu net bir hata belirtisi vermiyor; veriye ve yöntemin iç işleyişine
  ayrı ayrı bakıp yan yana koyunca ortaya çıktı.
* Uygulamalı test bugün yapılamadı.

## Öğrenilenler

* Yüksek çözünürlüklü veri tek başına avantaj değil; yöntemin girdiyi içeride
  nasıl işlediği beklenmedik bir sınır yaratabiliyor.
* Kritik bir adıma başlamadan önce ham veriyi bizzat açıp doğrulamak, kodu
  yazdıktan sonra hata aramaktan çok daha ucuz.
* Beklentiyle uyuşmayan sonuç da bir bulgudur; katkı bazen "çalıştı" demek değil,
  sınırın nerede olduğunu ölçmektir.

## Sonuç

Sentetik veriye geçişin zemini hazırlandı: veri yapısı çıkarıldı, hatalı bir
varsayım düzeltildi, ölçüm planı yeniden kurgulandı ve ölçümü bozacak bir tuzak
önceden yakalandı. En önemlisi, projenin cevaplayabileceği somut bir araştırma
sorusu tanımlandı: **engel kaç piksele düşene kadar takip edilebiliyor ve bu ne
kadar erken uyarı süresine karşılık geliyor?** Böylece ertesi günün ilk işi,
açık uçlu bir deneme değil, sonucu şimdiden anlamlı olan bir ölçüm haline geldi.
