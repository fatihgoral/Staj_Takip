# Gün 8 — Üretim hattının tamamlanması ve veri çıktısının doğrulanması

**Tarih:** 12 Ağustos 2026  
**Hedef:** Bir önceki gün alınan karar doğrultusunda (video-bazlı ayrım, sentetik doğrulama seti) üretim sürecini tamamlamak ve split kararını veriyle doğrulayıp kesinleştirmek.

## Genel Akış
Güne, bir önceki gün başlatılan üretim sürecinin ilerleyişini izleyerek başlandı. Üretim iki ayrı aşamadan oluşuyordu ve bu aşamaların birbirine bağımlı olması nedeniyle sıralı ilerlemek gerekiyordu: önce temiz sahne hazırlığı tamamlanmalı, ancak ondan sonra ikinci aşama olan engel yerleştirme başlatılabilirdi. Sürecin ilk aşaması tamamlanınca ikinci aşama (engel yerleştirme) de tam kapasitede çalıştırıldı ve sonuna kadar takip edildi. Süreç bittiğinde, çıktının sadece "tamamlandı" görünmesiyle yetinilmedi; gerçekten kullanılabilir olup olmadığı ayrıca sorgulandı. Bu sorgulama, günün ikinci yarısını oluşturan doğrulama ve karar kesinleştirme çalışmasının temelini attı.

## Yapılanlar

### Üretim çıktısının doğrulanması
- Üretim sürecinin ilk aşaması (temiz sahne hazırlığı) tamamlandı; çıktı sayısı ve iki kaynağa göre dağılımı kontrol edildi. Bu kontrol, sürecin sonraki aşamasına sağlam bir temel üzerinden geçilmesini sağlamak amacıyla yapıldı.
- İkinci aşama (engel yerleştirme) tam kapasitede çalıştırılıp tamamlandı. Bu aşamanın tam kapasitede çalıştırılması, önceki günlerde küçük ölçekli denemelerle sınırlı kalan sürecin artık üretim ölçeğine taşındığı anlamına geliyordu.
- Elde edilen çıktıda, projenin ilgilendiği 8 engel kategorisinin her iki kaynakta da dengeli dağıldığı sayısal olarak doğrulandı. Bu doğrulama, sadece toplam çıktı miktarına bakmanın yeterli olmadığı, kategori bazında da denge aranması gerektiği düşüncesiyle bilinçli olarak eklendi.

### Split kararının kesinleştirilmesi
- Dengeli dağılım görülünce, bir önceki gün taslak olarak belirlenen video-bazlı ayrım yöntemi (bir kaynağın tamamen eğitim, diğerinin tamamen doğrulama için kullanılması) kesin karar olarak onaylandı. Kararın taslaktan kesinleşmeye geçmesi, sadece yöntemsel bir tercihin değil, o tercihi destekleyen somut bir gözlemin de arkasında durması anlamına geliyordu.

## Çözülen Sorunlar
- **Gizli bölme kuralı riski:** Eski dönüştürme aracının bağımlı olduğu dış bir kural, dokümantasyon taranarak fark edildi ve yeni karara karışmadan devre dışı bırakıldı. Bu tür gizli/örtük kuralların fark edilmeden kalması, ileride açıklaması zor bir tutarsızlığa yol açabileceği için, bu adımın erken aşamada atılması önemli görüldü.

## Karşılaşılan Zorluklar
- Üretim sürecinin ikinci aşaması, her kaynak için yüzlerce olası aday arasından belirli sayıda örnek seçtiği için, ilk bakışta çıktının "yeterli" olup olmadığını anlamak sayım ve dağılım kontrolü gerektirdi. Sadece toplam sayıya bakmak yanıltıcı olabilirdi; bu yüzden kontrol, kategori bazında ayrıştırılarak yapıldı.

## Öğrenilenler

### Süreç ve yöntem
- Bir kararı taslaktan kesinleştirmeye geçirmeden önce veriyle doğrulamak (burada: dengeli dağılım kontrolü), kararın sağlamlığını artırıyor. Bu yaklaşım, ileride benzer karar noktalarında da bir alışkanlık olarak sürdürülmesi gereken bir yöntem olarak not edildi.

## Sonuç
Gün sonunda elimizde: 
1. Tamamlanmış ve doğrulanmış üretim çıktısı
2. Kesinleşmiş bir split kararı vardı. 

Bu iki çıktı, sonraki günün çalışmasının doğrudan girdisini oluşturdu.

## Sıradaki Adımlar
- [ ] Ham üretim çıktısını model eğitimine hazır formata dönüştürmek.
- [ ] Dönüştürme aracının yeniden yazılması ve test edilmesi.
- [ ] leakage kontrolünün gerçekleştirilmesi.
