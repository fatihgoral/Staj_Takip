# Gün 11 — Ucuz kontrol kolunun hazırlanması ve tur maliyetinin çıkarılması

## 13 Ağustos 2026

**Hedef:** Ham çıktıyı model eğitimine hazır bir veri setine dönüştürmek ve split
kararına uygun şekilde veri setini oluşturmak.




**Görev:** Karşılaştırma zincirinin en alt basamağınıkurmak

veriyi üretmek, modeli eğitmek ve sonucu ölçmek. Bu basamağın

amacı, en düşük maliyetli müdahalenin tek başına ne kadar kazandırdığını

göstermek ve üstteki daha pahalı yöntemler için bir alt sınır oluşturmak.



**Durum: yaklaşık yarısı tamamlandı.** Hazırlık ve üretim tarafında yol alındı;

eğitim ve ölçüm adımlarına geçilmedi, dolayısıyla bu kolun metrikleri henüz

üretilmedi.







## Ne Yaptım



- **Görevi bağlamına oturttum.** Bu kolun tek başına bir çıktı olmadığını, bir

  karşılaştırma zincirinin alt basamağı olduğunu netleştirdim. Dolayısıyla asıl

  soru "bu kol iyi sonuç veriyor mu?" değil, "üstündeki pahalı yöntemler bunun

  ne kadar üzerine çıkıyor?" sorusuydu. Bu ayrım, kolun nasıl kurulacağını da

  belirledi.



- **Eldeki araçları inceledim.** Sıfırdan yazmak yerine mevcut altyapıda bu iş

  için zaten var olan bileşeni çıkardım; ne ürettiğini, hangi girdilere ihtiyaç

  duyduğunu ve hangi ayarların oynatılabilir olduğunu belirledim.



- **Karşılaştırmanın geçerlilik koşulunu tespit ettim.** Bu kolun sonucunun

  anlamlı olabilmesi için, referans koşuyla **aynı koşullarda** üretilmesi

  gerektiğini gördüm. Koşullar sabitlenmezse çıkan farkın yöntemden mi yoksa

  veri farkından mı kaynaklandığı ayırt edilemezdi — yani ölçüm yapılmış olur

  ama yorumlanamazdı. Bu kuralı sadece bu kol için değil, zincirin tamamı için

  sabitledim.



- **Zincirin tamamını planladım.** Üst basamakları da gözden geçirdim; her birinin

  ne üreteceğini ve neye ihtiyaç duyduğunu çıkardım. Böylece basamakları

  birbirinden bağımsız işler olarak değil, tek bir karşılaştırma tablosunun

  satırları olarak planlamış oldum.



- **Turun maliyetini hesapladım.** Aşağıdaki bölüm.



---



## Karşılaştığım Kısıt: Bekleme Süresi



Üretim tarafını kurarken, bu kolun tam turunun ne kadar süreceğini hesapladım.

Her basamak üç aşamalı bir tur demek: önce o basamağa özel verinin üretilmesi,

sonra modelin eğitilmesi, en sonunda ölçüm ve raporlama. Üstelik sonucun rastgele

bir tesadüf olmadığını gösterebilmek için eğitim aşaması tek seferlik değil,

tekrarlı yapılmak zorunda — bu da o aşamanın maliyetini kat kat artırıyor.



Hesap şunu gösterdi: **tek bir basamağın turu bile gün mertebesinde bir bekleme

yaratıyor.** Zincirde birden fazla basamak olduğu için toplam, haftalık takvimde

birkaç haftalık yer kaplıyor.



Buradaki kritik nokta şu: darboğaz kodun kendisi ya da geliştirme süresi değil,

**turun kendi süresi.** Kod bir günde yazılıp hazır hâle geliyor; ama sonucun

çıkması için beklenen süre bunun kat kat üzerinde. Yani bu kalemi hızlandırmanın

yolu daha çok geliştirme yapmak değil, hangi basamakların gerçekten gerekli

olduğuna karar vermek.



Bu hesabı çıkardıktan sonra, işi sonuna kadar götürüp tıkanmak yerine konuyu

mentöre taşıdım.



---



## Ne Öğrendim



Bu gün, teknik bir adımdan çok yöntem ve planlama tarafında öğretici oldu:



1. **Bir deneyin maliyeti, onu yazma süresiyle ölçülmüyor.** Kodu bitirmek işin

   küçük kısmıydı; asıl maliyet, sonucu görmek için beklenen süreydi. Plan

   yaparken "bu kaç günde yazılır" değil, "bu kaç günde sonuç verir" diye

   sormak gerekiyormuş.



2. **Karşılaştırma, ancak tek değişken oynatıldığında karşılaştırmadır.**

   Koşullar sabitlenmeden alınan bir ölçüm sayı üretir ama bilgi üretmez. Bir

   sonucun "iyi" olduğunu söyleyebilmek için neye göre iyi olduğunun, sonuç

   alınmadan önce yazılı olarak sabitlenmiş olması gerekiyor.



3. **Tekrarlı koşu bilimsel bir zorunluluk ama takvimsel bir yük.** Sonucun

   tesadüf olmadığını göstermek maliyeti doğrusal biçimde artırıyor. Bu, bilimsel

   geçerlilikle proje takvimi arasında gerçek bir gerilim — ve bu gerilimin

   sessizce göz ardı edilmesi yerine açıkça konuşulup karara bağlanması gerekiyor.



4. **Bir maliyeti erken ölçüp paydaşa taşımak, işi sonuna kadar götürüp sonra

   tıkanmaktan iyi.** Turun süresini görür görmez mentöre gitmem, haftalarca

   sürecek bir kalemin doğru zamanda yeniden önceliklendirilmesini sağladı.

   Kötü haberi geç vermek, onu daha pahalı hâle getiriyor.



5. **Kapsam yönetimi de işin bir parçası.** Her basamağı koşmak mümkün, ama

   asıl soru hangisinin bir kararı değiştireceği. Sonucu kararı değiştirmeyecek

   bir deneyi koşmak, doğru yapılmış ama gereksiz bir iş oluyor.



6. **Ertelenen iş, yazılı bırakılırsa kayıp değildir.** Kolun tanımı, parametreleri

   ve karşılaştırma kuralı kayıt altına alındığı için, bu kaleme geri dönüldüğünde

   sıfırdan başlanmayacak. Bunu deneyimledikten sonra, yarım bırakılan her işin

   "nerede kaldığını" yazılı bırakmayı alışkanlık hâline getirdim.



---



## Mentör Toplantısı — Karar Değişikliği



Bekleme süresi tespitini mentöre aktardım ve toplantıda birlikte değerlendirdik.



**Mentörün değerlendirmesi:** Bu görevlerin tek tek koşulduğunda **uzun sürdüğü**

belirtildi. Her basamağın kendi üretim + eğitim + ölçüm turunu gerektirmesi,

yaklaşan sunum takvimiyle örtüşmüyordu; zincir tamamlanana kadar gösterilebilir

somut bir çıktı üretilemeyecekti.



**Alınan karar:** Bu basamaklara **ilerleyen haftalarda** dönülecek. Kalem iptal

edilmedi, takvimde ileriye alındı.



**Kararın pratik sonucu:**



- Yapılan hazırlık boşa gitmedi; kolun tanımı ve karşılaştırma kuralı hazır

  durumda bekliyor, geri dönüldüğünde sıfırdan başlanmayacak.

- Karşılaştırma kuralının referansla hizalanmış olması kalıcı bir kazanım — bu

  kaleme ne zaman dönülürse dönülsün sonuçlar aynı tabloya girebilecek.

- Kısa vadede odak, sunum takviminde karşılığı olan ve daha hızlı somut çıktı

  veren Hafta 3 kalemlerine kaydırıldı.
