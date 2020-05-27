---
title: Verizon Premium kural altyapısı özelliklerinden Azure CDN | Microsoft Docs
description: Verizon Premium kural altyapısı özelliklerinden Azure CDN için başvuru belgeleri.
services: cdn
author: asudbring
ms.service: azure-cdn
ms.topic: article
ms.date: 05/26/2020
ms.author: allensu
ms.openlocfilehash: d2d4090934a940809fe75ad70e0650eb1c9353f1
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83872726"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-features"></a>Verizon Premium kural altyapısı özelliklerinden Azure CDN

Bu makalede, Azure Content Delivery Network (CDN) [kuralları altyapısının](cdn-verizon-premium-rules-engine.md)kullanılabilir özelliklerinin ayrıntılı açıklamaları listelenmektedir.

Kuralın üçüncü bölümü özelliğidir. Bir özellik, bir eşleşme koşulları kümesi tarafından tanımlanan istek türüne uygulanan eylemin türünü tanımlar.


En son özellikler için [Verizon Rules Engine belgelerine](https://docs.vdms.com/cdn/index.html#Quick_References/HRE_QR.htm#Actions)bakın.


## <a name="azure-cdn-from-verizon-premium-rules-engine-features-reference"></a>Verizon Premium kural altyapısı Özellikler başvurusundan Azure CDN

---

### <a name="age-response-header"></a>Yaş yanıt üst bilgisi

**Amaç**: istek sahibine gönderilen yanıta bir yaş yanıtı üstbilgisinin eklenip eklenmeyeceğini belirler.

Değer|Sonuç
--|--
Etkin | Yaş yanıt üst bilgisi, istek sahibine gönderilen yanıta dahildir.
Devre dışı | Yaş yanıt üst bilgisi, istek sahibine gönderilen yanıttan çıkarılır.

**Varsayılan davranış**: devre dışı.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="bandwidth-parameters"></a>Bant genişliği parametreleri

**Amaç:** Bant genişliği azaltma parametrelerinin (örneğin, ec_rate ve ec_prebuf) etkin olup olmadığını belirler.

Bant genişliği azaltma parametreleri, bir istemcinin isteği için veri aktarım hızının özel bir oranla sınırlı olup olmadığını belirtir.

Değer|Sonuç
--|--
Etkin|Pop 'Ların bant genişliği azaltma isteklerini karşımasına izin verir.
Devre dışı|Pop 'Ların bant genişliği daraltma parametrelerini yoksaymasına neden olur. İstenen içerik normal olarak sunulur (yani, bant genişliği azaltma olmadan).

**Varsayılan davranış:** Etkinletir.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="bandwidth-throttling"></a>Bant genişliği azaltma

**Amaç:** Pop 'Lar tarafından girilen yanıtın bant genişliğini kısıtlar.

Bant genişliği azaltmayı doğru bir şekilde ayarlamak için aşağıdaki seçeneklerin her ikisi de tanımlanmalıdır.

Seçenek|Açıklama
--|--
KB/saniye|Bu seçeneği, yanıtı teslim etmek için kullanılabilecek maksimum bant genişliği (saniye başına KB) olarak ayarlayın.
Ön Saniyeler|Bu seçeneği, açılan sayının bant genişliği kısıtlanana kadar bekleyeceği saniye sayısına ayarlayın. Sınırsız bant genişliği için bu sürenin amacı, bir Media Player 'ın bant genişliği azaltmasından kaynaklanan sorunları veya arabelleğe alma sorunlarını yaşmasını önlemektir.

**Varsayılan davranış:** Devre dışı.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="bypass-cache"></a>Atlama önbelleği

**Amaç:** İsteğin önbelleğe alma işleminin atlanıp atlanmayacağını belirler.

Değer|Sonuç
--|--
Etkin|İçerik daha önce pop 'Larda önbelleğe alınmış olsa bile tüm isteklerin kaynak sunucuya dönmesine neden olur.
Devre dışı|Pop 'Ların, yanıt üst bilgilerinde tanımlanan önbellek ilkelerine göre önbelleğe alınmasına neden olur.

**Varsayılan davranış:**

- **Http büyük:** Devre dışı

<!---
- **ADN:** Enabled

--->

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="cacheable-http-methods"></a>Önbelleklenebilir HTTP yöntemleri

**Amaç:** Ağda Önbelleğe alınabilecek ek HTTP yöntemleri kümesini belirler.

Anahtar bilgileri:

- Bu özellik, GET yanıtlarının her zaman önbelleğe alınması gerektiğini varsayar. Sonuç olarak, bu özellik ayarlanırken GET HTTP yöntemi eklenmemelidir.
- Bu özellik yalnızca POST HTTP yöntemini destekler. Bu özelliği olarak ayarlayarak yanıt verme işlemini etkinleştirin `POST` .
- Varsayılan olarak, yalnızca gövdesi 14 KB 'tan küçük olan istekler önbelleğe alınır. En büyük istek gövdesi boyutunu ayarlamak için önbelleklenebilir Istek gövdesi boyutu özelliğini kullanın.

**Varsayılan davranış:** Yalnızca yanıtları al önbelleğe alınır.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="cacheable-request-body-size"></a>Önbelleklenebilir Istek gövdesi boyutu

**Amaç:** Bir POST yanıtının önbelleğe alınıp alınmayacağını belirlemek için eşiği tanımlar.

Bu eşik, en büyük istek gövdesi boyutu belirtilerek belirlenir. Daha büyük bir istek gövdesi içeren istekler önbelleğe alınmaz.

Anahtar bilgileri:

- Bu özellik yalnızca, POST yanıtları önbelleğe alma için uygun olduğunda geçerlidir. POST isteği önbelleğe almayı etkinleştirmek için önbelleklenebilir HTTP yöntemleri özelliğini kullanın.
- İstek gövdesi şunları göz önünde bulundurulmalıdır:
    - x-www-form-urlencoded değerleri
    - Benzersiz bir önbellek anahtarı sağlama
- Büyük bir en büyük istek gövdesi boyutunun tanımlanması, veri teslimi performansını etkileyebilir.
    - **Önerilen değer:** 14 KB
    - **En küçük değer:** 1 KB

**Varsayılan davranış:** 14 KB

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="cache-control-header-treatment"></a>Cache-Control üst bilgisi Işleme

**Amaç:** `Cache-Control`Dış en büyük yaş özelliği etkin olduğunda, pop 'un üst bilgi üretimini denetler.

Bu tür yapılandırmayı gerçekleştirmenin en kolay yolu, dış en yüksek yaş ve Cache-Control üst bilgisi Işleme özelliklerini aynı deyime yerleştirmekten biridir.

Değer|Sonuç
--|--
Üzerine yaz|Aşağıdaki eylemlerin gerçekleşmesini sağlar:<br/> - `Cache-Control` Kaynak sunucu tarafından oluşturulan üstbilginin üzerine yazar. <br/>- `Cache-Control` Dış en büyük yaş özelliği tarafından üretilen üstbilgiyi yanıta ekler.
Geçiş|`Cache-Control`Dış en büyük yaş özelliği tarafından üretilen üstbilginin yanıta hiçbir zaman eklenmemesini sağlar. <br/> Kaynak sunucu bir `Cache-Control` üst bilgi üretirse, son kullanıcıya geçer. <br/> Kaynak sunucu bir `Cache-Control` üst bilgi oluşturmuyorsa, bu seçenek yanıt üst bilgisinin bir başlık içermediği oluşmasına neden olabilir `Cache-Control` .
Eksikse Ekle|`Cache-Control`Kaynak sunucudan bir üst bilgi alınmadığında, bu seçenek `Cache-Control` dış en yüksek yaş özelliği tarafından üretilen üstbilgiyi ekler. Bu seçenek, tüm varlıkların bir üst bilgi atanmasını sağlamak için yararlıdır `Cache-Control` .
Kaldır| Bu seçenek `Cache-Control` , üst bilgi yanıtına bir üst bilgi dahil olmamasını sağlar. Bir `Cache-Control` üst bilgi zaten atanmışsa, üstbilgi yanıtından kaldırılır.

**Varsayılan davranış:** Yazılacak.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="cache-key-query-string"></a>Cache-Key sorgu dizesi

**Amaç:** Önbellek anahtarının bir istekle ilişkili sorgu dizesi parametrelerini içerip içermediğini veya dışmadığını belirler.

Anahtar bilgileri:

- Bir veya daha fazla sorgu dizesi parametre adı belirtin ve her bir parametre adını tek bir boşluk ile ayırın.
- Bu özellik, sorgu dizesi parametrelerinin önbellek anahtarından dahil edilip edilmediğini belirler. Aşağıdaki tablodaki her seçenek için ek bilgi sağlanır.

Tür|Açıklama
--|--
 Şunları Dahil Et:|  Belirtilen her parametrenin Cache-Key içine dahil edileceğini gösterir. Bu özellikte tanımlanan bir sorgu dizesi parametresi için benzersiz bir değer içeren her istek için benzersiz bir önbellek anahtarı oluşturulur.
 Tümünü dahil et  |Benzersiz bir sorgu dizesi içeren bir varlığa yönelik her istek için benzersiz bir önbellek anahtarı oluşturulduğunu gösterir. Bu tür bir yapılandırma genellikle önerilmez çünkü, küçük bir önbellek okuması yüzdesine yol açabilir. Az sayıda önbellek okuması, daha fazla istek sunması gerektiğinden, kaynak sunucu üzerindeki yükü artırır. Bu yapılandırma, sorgu dizesi önbelleğe alma sayfasında "benzersiz önbellek" olarak bilinen önbelleğe alma davranışını çoğaltır.
 Exclude | Önbellek anahtarından yalnızca belirtilen parametrelerin dışlandığını gösterir. Diğer tüm sorgu dizesi parametreleri Cache-Key ' a dahildir.
 Tümünü Dışla  |Tüm sorgu dizesi parametrelerinin önbellek anahtarından dışlandığını gösterir. Bu yapılandırma, sorgu dizesi önbelleğe alma sayfasında "standart önbellek" varsayılan önbelleğe alma davranışını çoğaltır.  

Rules motoru, sorgu dizesi önbelleğinin uygulanma şeklini özelleştirmenizi sağlar. Örneğin, sorgu dizesi önbelleğinin yalnızca belirli konumlarda veya dosya türlerinde gerçekleştirileceğini belirtebilirsiniz.

Sorgu dizesi önbelleğe alma sayfasında "önbelleğe alma" sorgu dizesi önbelleği davranışını yinelemek için, URL sorgusu joker karakter eşleştirme koşulunu ve bir atlama önbelleği özelliğini içeren bir kural oluşturun. URL sorgusu joker karakter eşleştirme koşulunu bir yıldız işareti (*) olarak ayarlayın.

>[!IMPORTANT]
> Bu hesapta herhangi bir yol için belirteç yetkilendirmesi etkinleştirilmişse, sorgu dizesi önbelleğe alma için kullanılabilecek tek mod standart önbellek modu olur. Daha fazla bilgi için bkz. [Sorgu dizeleri içeren Azure CDN önbelleğe alma davranışını kontrol etme](cdn-query-string-premium.md).

#### <a name="sample-scenarios"></a>Örnek senaryolar

Bu özellik için aşağıdaki örnek kullanım örnek bir istek ve varsayılan önbellek anahtarı sağlar:

- **Örnek istek:** http://wpc.0001.&lt ;D omain &gt; /800001/Origin/Folder/Asset.htm? SessionID = 1234&language = EN&UserID = 01
- **Varsayılan önbellek-anahtar:** /800001/Origin/Folder/Asset.htm

##### <a name="include"></a>Şunları Dahil Et:

Örnek yapılandırma:

- **Şunu yazın:** İçeriyor
- **Parametreler:** dil

Bu tür bir yapılandırma, şu sorgu dizesi parametresi önbellek anahtarını üretir:

    /800001/Origin/folder/asset.htm?language=EN

##### <a name="include-all"></a>Tümünü dahil et

Örnek yapılandırma:

- **Şunu yazın:** Tümünü dahil et

Bu tür bir yapılandırma, şu sorgu dizesi parametresi önbellek anahtarını üretir:

    /800001/Origin/folder/asset.htm?sessionid=1234&language=EN&userid=01

##### <a name="exclude"></a>Exclude

Örnek yapılandırma:

- **Şunu yazın:** Amaz
- **Parametreler:** sessioned Kullanıcı kimliği

Bu tür bir yapılandırma, şu sorgu dizesi parametresi önbellek anahtarını üretir:

    /800001/Origin/folder/asset.htm?language=EN

##### <a name="exclude-all"></a>Tümünü Dışla

Örnek yapılandırma:

- **Şunu yazın:** Tümünü Dışla

Bu tür bir yapılandırma, şu sorgu dizesi parametresi önbellek anahtarını üretir:

    /800001/Origin/folder/asset.htm

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="cache-key-rewrite"></a>Cache-Key yeniden yazma

**Amaç:** Bir istekle ilişkili önbellek anahtarını yeniden yazar.

Önbellek anahtarı, önbelleğe alma amacıyla bir varlığı tanımlayan göreli yoldur. Diğer bir deyişle, sunucular, bir varlığın önbelleğe alınmış bir sürümünü, önbellek anahtarı tarafından tanımlanan yoluna göre denetler.

Aşağıdaki seçeneklerden her ikisini tanımlayarak bu özelliği yapılandırın:

Seçenek|Açıklama
--|--
Özgün yol| Önbellek anahtarı yeniden yazıldı istek türlerinin göreli yolunu tanımlayın. Göreli bir yol, temel kaynak yolu seçilerek ve sonra bir normal ifade deseninin tanımlanmasına göre tanımlanabilir.
Yeni yol|Yeni önbellek anahtarı için göreli yolu tanımlayın. Göreli bir yol, temel kaynak yolu seçilerek ve sonra bir normal ifade deseninin tanımlanmasına göre tanımlanabilir. Bu göreli yol, [http değişkenleri](cdn-http-variables.md)kullanılarak dinamik olarak oluşturulabilir.

**Varsayılan davranış:** İsteğin Cache-Key değeri, istek URI 'SI tarafından belirlenir.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="comment"></a>Açıklama

**Amaç:** Kuralın içine bir notun eklenmesine izin verir.

Bu özellik için bir kullanım, kuralın genel amacına veya belirli bir eşleşme koşulunun veya özelliğinin kurala Eklenme nedeninizi hakkında ek bilgi sağlamaktır.

Anahtar bilgileri:

- En fazla 150 karakter belirtilebilir.
- Yalnızca alfasayısal karakterler kullanın.
- Bu özellik kuralın davranışını etkilemez. Yalnızca gelecekteki başvuru için bilgi sağlayabileceğiniz veya kuralda sorun giderirken yardımcı olabilecek bir alan sağlamaktır.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="complete-cache-fill"></a>Önbellek dolgusunu tamamla

**Amaç:** Bir istek bir POP 'ta kısmi önbellek isabetsizliği ile sonuçlanırsa ne olacağını belirler.

Kısmi önbellek isabetsizliği, bir POP 'a tamamen indirilmeyen bir varlık için önbellek durumunu açıklar. Bir varlık bir POP üzerinde yalnızca kısmen önbelleğe alınmışsa bu varlık için bir sonraki istek, kaynak sunucuya yeniden iletilir.
<!---
This feature is not available for the ADN platform. The typical traffic on this platform consists of relatively small assets. The size of the assets served through these platforms helps mitigate the effects of partial cache misses, since the next request will typically result in the asset being cached on that POP.

--->
Kısmi önbellek isabetsizlik genellikle, bir Kullanıcı bir indirmeyi iptal ettikten veya yalnızca HTTP Aralık istekleri kullanılarak istenen varlıklar için oluşur. Bu özellik, genellikle başlangıçtan sona kadar indirilmeyen büyük varlıklar (örneğin, videolar) için kullanışlıdır. Sonuç olarak, bu özellik HTTP büyük platformunda varsayılan olarak etkindir. Diğer tüm platformlarda devre dışıdır.

Müşteri kaynak sunucunuzdaki yükü azalttığı ve müşterilerinizin içeriğinizi indirme hızını artıracağından, HTTP büyük platformunun varsayılan yapılandırmasını koruyun.

Değer|Sonuç
--|--
Etkin|Varsayılan davranışı geri yükler. Varsayılan davranış, POP 'un kaynak sunucudan varlığın bir arka plan getirme işlemini başlatmasını zorlamaktır. Bundan sonra varlık, POP 'un yerel önbelleğinde olacaktır.
Devre dışı|Bir POP 'un varlık için bir arka plan getirme gerçekleştirmesini engeller. Sonuç, söz konusu varlığa ait bir sonraki isteğin, bir POP 'un müşteri kaynak sunucusundan talep etmesine neden olur.

**Varsayılan davranış:** Etkinletir.

#### <a name="compatibility"></a>Uyumluluk

Önbellek ayarlarının izlenme şekli nedeniyle, bu özellik aşağıdaki eşleşme koşullarıyla ilişkilendirilemez:

- AS numarası
- İstemci IP Adresi
- Cookie parametresi
- Tanımlama bilgisi parametre Regex
- Ülke
- Cihaz
- Microsoft Edge CNAME
- Başvurulan etki alanı
- İstek üst bilgisi sabit değeri
- İstek üst bilgisi Regex
- İstek üstbilgisi joker karakteri
- Request Yöntemi
- İstek düzeni
- URL sorgusu sabit değeri
- URL sorgu Regex
- URL sorgusu joker karakteri
- URL sorgu parametresi

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="compress-file-types"></a>Dosya türlerini sıkıştır

**Amaç:** Sunucuda sıkıştırılan dosyalar için dosya biçimlerini tanımlar.

Bir dosya biçimi, Internet medya türü (örneğin, Içerik türü) kullanılarak belirtilebilir. Internet medya türü, sunucuların belirli bir varlığın dosya biçimini belirlemesine izin veren platformdan bağımsız meta verilerdir. Aşağıda yaygın Internet medya türleri listesi verilmiştir.

Internet medya türü|Açıklama
--|--
metin/düz|Düz metin dosyaları
text/html| HTML dosyaları
text/css|Geçişli Stil Sayfaları (CSS)
uygulama/x-JavaScript|JavaScript
Uygulama/JavaScript|JavaScript

Anahtar bilgileri:

- Her birini tek bir boşlukla ayırarak birden çok Internet medya türü belirtin.
- Bu özellik yalnızca boyutu 1 MB 'tan küçük olan varlıkları sıkıştırır. Daha büyük varlıklar sunucular tarafından sıkıştırılmaz.
- Görüntü, video ve ses medya varlıkları (örneğin, JPG, MP3, MP4 vb.) gibi belirli içerik türleri zaten sıkıştırılmış. Bu varlık türlerinde ek sıkıştırma, dosya boyutunu önemli ölçüde azalmadığından, bunlarda sıkıştırmayı etkinleştirmezseniz.
- Yıldız işaretleri gibi joker karakterler desteklenmez.
- Bu özelliği bir kurala eklemeden önce, bu kuralın uygulandığı platformun sıkıştırma sayfasında sıkıştırma devre dışı seçeneğini ayarlamış olduğunuzdan emin olun.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="custom-log-field-1"></a>Özel günlük alanı 1

**Amaç:** Ham günlük dosyasındaki özel günlük alanına atanacak biçimi ve içeriği belirler.

Bu özel alan, günlük dosyalarınızda hangi istek ve yanıt üst bilgi değerlerinin saklandığını belirlemenizi sağlar.

Varsayılan olarak, özel günlük alanı "x-ec_custom-1" olarak adlandırılır. Bu alanın adı ham günlük ayarları sayfasından özelleştirilebilir.

İstek ve yanıt üst bilgilerini belirtmeye yönelik biçim aşağıdaki gibi tanımlanır:

Üst bilgi türü|Biçimlendir|Örnekler
-|-|-
İstek Başlığı|`%{[RequestHeader]()}[i]()` | % {Accept-Encoding} ı <br/> {Başvuran} ı <br/> % {Authorization} ı
Yanıt Üst Bilgisi|`%{[ResponseHeader]()}[o]()`| % {Age} o <br/> % {Content-Type} o <br/> % {Cookie} o

Anahtar bilgileri:

- Özel bir günlük alanı, üst bilgi alanları ve düz metin birleşimini içerebilir.
- Bu alan için geçerli karakterler şunlardır: alfasayısal (0-9, a-z, ve A-Z), tireler, iki nokta üst üste, noktalı virgül, kesme işareti, virgül, nokta, alt çizgi, eşittir işareti, parantezler, köşeli ayraçlar ve boşluklar. Yüzde simgesi ve küme ayraçları yalnızca bir üst bilgi alanı belirtmek için kullanıldığında izin verilir.
- Belirtilen her üst bilgi alanı için yazım denetimi, istenen istek/yanıt üst bilgisi adı ile aynı olmalıdır.
- Birden çok üst bilgi belirtmek istiyorsanız, her üstbilgiyi göstermek için bir ayırıcı kullanın. Örneğin, her üstbilgi için bir kısaltma kullanabilirsiniz:
    - AE:% {Accept-Encoding} ı:% {Authorization} kullanıyorum:% {Content-Type} o

**Varsayılan değer:** -

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---
### <a name="debug-cache-response-headers"></a>Önbellek yanıtı üst bilgilerinde hata ayıkla

**Amaç:** Yanıtın, istenen varlık için önbellek ilkesi hakkında bilgi sağlayan [X-EC-hata ayıklama yanıt üst bilgilerini](cdn-http-debug-headers.md)içerip içeremeyeceğini belirler.

Aşağıdakilerden her ikisi de doğru olduğunda hata ayıklama önbelleği yanıt üstbilgileri yanıta dahil edilir:

- Belirtilen istekte hata ayıklama önbelleği yanıt üstbilgileri özelliği etkinleştirildi.
- Belirtilen istek, yanıta dahil edilecek hata ayıklama önbelleği yanıt üst bilgileri kümesini tanımlar.

Aşağıdaki üst bilgi ve istekte belirtilen yönergeler eklenerek hata ayıklama önbelleği yanıt üstbilgileri istenebilir:

`X-EC-Debug: _&lt;Directive1&gt;_,_&lt;Directive2&gt;_,_&lt;DirectiveN&gt;_`

**Örneğinde**

X-EC-hata ayıklama: x-EC-önbellek, x-EC-Check-önbelleklenebilir, x-EC-Cache-Key, x-EC-Cache-State

Değer|Sonuç
-|-
Etkin|Hata ayıklama önbelleği yanıt üstbilgileri istekleri, X-EC-Debug üstbilgisini içeren bir yanıt döndürür.
Devre dışı|X-EC-Debug yanıt üst bilgisi yanıttan dışlanır.

**Varsayılan davranış:** Devre dışı.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---
### <a name="default-internal-max-age"></a>Varsayılan dahili maksimum yaş

**Amaç:** POP/kaynak sunucu önbelleği yeniden doğrulama için varsayılan en yüksek yaş aralığını belirler. Diğer bir deyişle, bir POP 'tan önce geçirilecek zaman miktarı, önbelleğe alınmış bir varlığın, kaynak sunucuda depolanan varlıkla eşleşip eşleşmediğini kontrol eder.

Anahtar bilgileri:

- Bu eylem yalnızca `Cache-Control` veya üst bilgisinde maksimum yaş bildirimi atamamış olan bir kaynak sunucudan yanıt almak için geçerlidir `Expires` .
- Bu eylem, önbelleğe alınabilir olarak kabul edilen varlıklar için gerçekleşmeyecektir.
- Bu eylem, tarayıcıyı POP önbelleği yeniden doğrulamaları için etkilemez. Bu tür yeniden doğrulamaları, `Cache-Control` `Expires` dış en yüksek yaş özelliğiyle özelleştirilebilen, tarayıcıya gönderilen ve üst bilgileri tarafından belirlenir.
- Bu eylemin sonuçlarında, yanıt üst bilgileri ve içeriklerinizin pop 'Larından döndürülen içerik üzerinde bir observable etkisi yoktur, ancak pop 'lerden kaynak sunucunuza gönderilen yeniden doğrulama trafiği miktarı üzerinde bir etkisi olabilir.
- Bu özelliği şu şekilde yapılandırın:
    - Varsayılan dahili maksimum yaş için uygulanabilir durum kodunu seçme.
    - Bir tamsayı değeri belirtip istenen zaman birimini seçin (örneğin, saniye, dakika, saat, vb.). Bu değer, varsayılan dahili maksimum yaş aralığını tanımlar.

- Zaman biriminin "kapalı" olarak ayarlanması, `Cache-Control` veya üst bilgisinde maksimum yaş göstergesi atanmamış istekler için 7 günlük varsayılan bir iç maksimum yaş aralığı atar `Expires` .

**Varsayılan değer:** 7 gün

#### <a name="compatibility"></a>Uyumluluk

Önbellek ayarlarının izlenme şekli nedeniyle, bu özellik aşağıdaki eşleşme koşullarıyla ilişkilendirilemez:
- AS numarası
- İstemci IP Adresi
- Cookie parametresi
- Tanımlama bilgisi parametre Regex
- Ülke
- Cihaz
- Edge CNAME
- Başvurulan etki alanı
- İstek üst bilgisi sabit değeri
- İstek üst bilgisi Regex
- İstek üstbilgisi joker karakteri
- Request Yöntemi
- İstek düzeni
- URL sorgusu sabit değeri
- URL sorgu Regex
- URL sorgusu joker karakteri
- URL sorgu parametresi

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="deny-access-403"></a>Erişimi Reddet (403)

**Amaç**: tüm Isteklerin 403 yasaklanmış bir Yanıt ile reddedildiğini belirler.

Değer | Sonuç
------|-------
Etkin| , Eşleşen ölçütlere uyan tüm isteklerin 403 yasaklanmış bir Yanıt ile reddedilmesini sağlar.
Devre dışı| Varsayılan davranışı geri yükler. Varsayılan davranış, kaynak sunucunun döndürülecek yanıt türünü belirlemesine izin vermeyecektir.

**Varsayılan davranış**: devre dışı

> [!TIP]
   > Bu özellik için olası bir kullanım, içeriğinize satır içi bağlantılar kullanan HTTP başvura erişimini engellemek için bir Istek üst bilgisi eşleştirme durumuyla ilişkilendirmedir.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="expires-header-treatment"></a>Süre sonu üst bilgisi Işleme

**Amaç:** `Expires`Dış en büyük yaş özelliği etkin olduğunda, BIR pop ile üstbilgileri oluşturmayı denetler.

Bu tür yapılandırmayı gerçekleştirmenin en kolay yolu, dış en büyük yaş ve süre sonu üst bilgisi Işleme özelliklerini aynı deyime yerleştirmekten biridir.

Değer|Sonuç
--|--
Üzerine yaz|Aşağıdaki eylemlerin gerçekleşmesini sağlar:<br/>- `Expires` Kaynak sunucu tarafından oluşturulan üstbilginin üzerine yazar.<br/>- `Expires` Dış en büyük yaş özelliği tarafından üretilen üstbilgiyi yanıta ekler.
Geçiş|`Expires`Dış en büyük yaş özelliği tarafından üretilen üstbilginin yanıta hiçbir zaman eklenmemesini sağlar. <br/> Kaynak sunucu bir `Expires` üst bilgi üretirse, son kullanıcıya geçiş yapılır. <br/>Kaynak sunucu bir `Expires` üst bilgi oluşturmuyorsa, bu seçenek yanıt üst bilgisinin bir başlık içermediği oluşmasına neden olabilir `Expires` .
Eksikse Ekle| `Expires`Kaynak sunucudan bir üst bilgi alınmadığında, bu seçenek `Expires` dış en yüksek yaş özelliği tarafından üretilen üstbilgiyi ekler. Bu seçenek, tüm varlıkların bir üst bilgi atanmasını sağlamak için yararlıdır `Expires` .
Kaldır| Üst `Expires` bilgi yanıtına bir üst bilgi dahil olmamasını sağlar. Bir `Expires` üst bilgi zaten atanmışsa, üstbilgi yanıtından kaldırılır.

**Varsayılan davranış:** Yazılacak

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="external-max-age"></a>Dış maksimum yaş

**Amaç:** Tarayıcı için AÇıLAN önbellek yeniden doğrulama için maksimum yaş aralığını belirler. Diğer bir deyişle, bir tarayıcıdan, bir tarayıcıdan bir varlığın yeni bir sürümünü kontrol etmeden önce geçirilecek zaman miktarı.

Bu özelliğin etkinleştirilmesi, `Cache-Control: max-age` `Expires` pop 'ları oluşturur ve bunları http istemcisine gönderir. Varsayılan olarak, bu üst bilgiler kaynak sunucu tarafından oluşturulan üstbilgilerin üzerine yazılır. Ancak, bu davranışı değiştirmek için Cache-Control üst bilgisi Işleme ve süre sonu üst bilgisi Işleme özellikleri kullanılabilir.

Anahtar bilgileri:

- Bu eylem, başlangıç kaynak sunucu önbelleği yeniden doğrulamaları 'nı etkilemez. Bu tür yeniden doğrulamaları, `Cache-Control` `Expires` kaynak sunucudan alınan ve üst bilgileri tarafından belirlenir ve varsayılan dahili maksimum yaş ve en uzun Iç kullanım süresi özellikleriyle özelleştirilebilir.
- Bu özelliği bir tamsayı değer belirterek ve istediğiniz zaman birimini seçerek yapılandırın (örneğin, saniye, dakika, saat, vb.).
- Bu özelliğin negatif bir değere ayarlanması, pop 'Ların bir `Cache-Control: no-cache` `Expires` kez ve tarayıcıya her yanıt olarak ayarlanan bir zaman gönderilmesine neden olur. Bir HTTP istemcisi yanıtı önbelleğe sunmayacak olsa da, bu ayar pop 'Ların kaynak sunucudan gelen yanıtı önbelleğe alma yeteneğini etkilemez.
- Zaman biriminin "kapalı" olarak ayarlanması, bu özelliği devre dışı bırakır. `Cache-Control` `Expires` Kaynak sunucunun yanıtı ile önbelleğe alınan ve üst bilgiler tarayıcıya geçiş yapılır.

**Varsayılan davranış:** Dışına

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="follow-redirects"></a>Yeniden yönlendirmeleri takip et

**Amaç:** İsteklerin bir müşteri kaynak sunucusu tarafından döndürülen konum üstbilgisinde tanımlanan ana bilgisayar adına yönlendirilip yönlendirilmeyeceğini belirler.

Anahtar bilgileri:

- İstekler yalnızca aynı platforma karşılık gelen Edge CNAMEs 'e yeniden yönlendirilebilir.

Değer|Sonuç
-|-
Etkin|İstekler yeniden yönlendirilebilir.
Devre dışı|İstekler yeniden yönlendirilmeyecektir.

**Varsayılan davranış:** Devre dışı.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="force-internal-max-age"></a>Maksimum Iç kullanım süresi

**Amaç:** POP/kaynak sunucu önbelleği yeniden doğrulama için maksimum yaş aralığını belirler. Diğer bir deyişle, bir POP 'tan önce geçecek süre, önbelleğe alınmış bir varlığın, kaynak sunucuda depolanan varlıkla eşleşip eşleşmediğini kontrol edebilir.

Anahtar bilgileri:

- Bu özellik, içinde tanımlanan en büyük yaş aralığını `Cache-Control` veya `Expires` bir kaynak sunucudan oluşturulan üst bilgileri geçersiz kılar.
- Bu özellik, tarayıcıyı POP önbelleği yeniden doğrulamaları için etkilemez. Bu tür yeniden doğrulamaları, `Cache-Control` `Expires` tarayıcıya gönderilen veya üstbilgilere göre belirlenir.
- Bu özelliğin istek sahibine bir POP tarafından teslim edilen yanıt üzerinde bir observable etkisi yoktur. Ancak, pop 'lerden kaynak sunucuya gönderilen yeniden doğrulama trafiği miktarı üzerinde bir etkisi olabilir.
- Bu özelliği şu şekilde yapılandırın:
    - Bir iç maksimum yaş için uygulanacak durum kodunu seçme.
    - Bir tamsayı değeri belirtme ve istediğiniz zaman birimini seçme (örneğin, saniye, dakika, saat, vb.). Bu değer, isteğin en yüksek yaş aralığını tanımlar.

- Zaman biriminin "kapalı" olarak ayarlanması bu özelliği devre dışı bırakır. Bir iç maksimum yaş aralığı, istenen varlıklara atanmaz. Özgün üst bilgi önbelleğe alma yönergeleri içermiyorsa, varlık varsayılan dahili maksimum yaş özelliğindeki etkin ayara göre önbelleğe alınır.

**Varsayılan davranış:** Dışına

#### <a name="compatibility"></a>Uyumluluk

Önbellek ayarlarının izlenme şekli nedeniyle, bu özellik aşağıdaki eşleşme koşullarıyla ilişkilendirilemez:
- AS numarası
- İstemci IP Adresi
- Cookie parametresi
- Tanımlama bilgisi parametre Regex
- Ülke
- Cihaz
- Edge CNAME
- Başvurulan etki alanı
- İstek üst bilgisi sabit değeri
- İstek üst bilgisi Regex
- İstek üstbilgisi joker karakteri
- Request Yöntemi
- İstek düzeni
- URL sorgusu sabit değeri
- URL sorgu Regex
- URL sorgusu joker karakteri
- URL sorgu parametresi

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="h264-support-http-progressive-download"></a>H. ula desteği (HTTP aşamalı Indirme)

**Amaç:** İçerik akışı için kullanılabilen H., dosya biçimlerinin türlerini belirler.

Anahtar bilgileri:

- Dosya uzantıları seçeneğinde, izin verilen bir H., izin adı uzantıları kümesi tanımlayın. Dosya uzantıları seçeneği, varsayılan davranışı geçersiz kılar. Bu seçeneği ayarlarken bu dosya adı uzantılarını ekleyerek MP4 ve F4V desteğini koruyun.
- Her dosya adı uzantısını (örneğin, _. mp4_, _. F4V_) belirttiğinizde bir nokta ekleyin.

**Varsayılan davranış:** HTTP aşamalı Indirme, MP4 ve F4V medyasını varsayılan olarak destekler.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="honor-no-cache-request"></a>Önbellek Isteğini kabul edin

**Amaç:** HTTP istemcisinin önbelleğe alma isteklerinin kaynak sunucuya iletilip iletilmeyeceğini belirler.

Http istemcisi `Cache-Control: no-cache` http isteğinde bir ve/veya üst bilgisi gönderdiğinde, No-Cache isteği oluşur `Pragma: no-cache` .

Değer|Sonuç
--|--
Etkin|HTTP istemcisinin önbelleğe alma isteklerinin kaynak sunucuya iletilmesine izin verir ve kaynak sunucu yanıt üst bilgilerini ve gövdesini geri AÇıLAN HTTP istemcisine geri döndürür.
Devre dışı|Varsayılan davranışı geri yükler. Varsayılan davranış, önbellek isteklerinin kaynak sunucuya iletilmesini önlemektir.

Tüm üretim trafiği için, bu özelliğin varsayılan devre dışı durumunda bırakılması kesinlikle önerilir. Aksi takdirde, kaynak sunucular, Web sayfalarını yenilerken çok sayıda önbellek isteğini yanlışlıkla tetikleyebilen son kullanıcılardan veya her video isteğiyle hiç önbellek üst bilgisi gönderecek şekilde kodlanmış birçok popüler medya oynatıcısında korumasız olmayacaktır. Bununla birlikte, yeni içeriğin kaynak sunucudan isteğe bağlı olarak çekebilmesini sağlamak için, bu özellik belirli bir üretim dışı hazırlama veya test etme dizinine uygulamak yararlı olabilir.

Bu özellik nedeniyle bir kaynak sunucuya iletilemeyen bir istek için bildirilen önbellek durumu `TCP_Client_Refresh_Miss` . Çekirdek raporlama modülünde kullanılabilen önbellek durumları raporu, önbellek durumuna göre istatistiksel bilgiler sağlar. Bu rapor, bu özellik nedeniyle bir kaynak sunucuya iletilemekte olan isteklerin sayısını ve yüzdesini izlemenize olanak sağlar.

**Varsayılan davranış:** Devre dışı.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="ignore-origin-no-cache"></a>Önbellek yok sayma

**Amaç:** CDN 'in bir kaynak sunucudan sunulan aşağıdaki yönergeleri yoksayıp saymayacağını belirler:

- `Cache-Control: private`
- `Cache-Control: no-store`
- `Cache-Control: no-cache`
- `Pragma: no-cache`

Anahtar bilgileri:

- Yukarıdaki yönergelerin yoksayılacağı bir durum kodu listesi tanımlayarak bu özelliği yapılandırın.
- Bu özellik için geçerli durum kodları kümesi şunlardır: 200, 203, 300, 301, 302, 305, 307, 400, 401, 402, 403, 404, 405, 406, 407, 408, 409, 410, 411, 412, 413, 414, 415, 416, 417, 500, 501, 502, 503, 504 ve 505.
- Bu özelliği boş bir değere ayarlayarak devre dışı bırakın.

**Varsayılan davranış:** Varsayılan davranış, yukarıdaki yönergeleri dikkate almak için kullanılır.

#### <a name="compatibility"></a>Uyumluluk

Önbellek ayarlarının izlenme şekli nedeniyle, bu özellik aşağıdaki eşleşme koşullarıyla ilişkilendirilemez:
- AS numarası
- İstemci IP Adresi
- Cookie parametresi
- Tanımlama bilgisi parametre Regex
- Ülke
- Cihaz
- Edge CNAME
- Başvurulan etki alanı
- İstek üst bilgisi sabit değeri
- İstek üst bilgisi Regex
- İstek üstbilgisi joker karakteri
- Request Yöntemi
- İstek düzeni
- URL sorgusu sabit değeri
- URL sorgu Regex
- URL sorgusu joker karakteri
- URL sorgu parametresi

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="ignore-unsatisfiable-ranges"></a>Unsatisfiable aralıklarını yoksay

**Amaç:** İstek bir 416 Istenen Aralık Satisfiable durum kodu oluşturduğunda istemcilere döndürülecek yanıtı belirler.

Varsayılan olarak, bu durum kodu, belirtilen bayt aralığı isteği bir POP tarafından karşılanmıyorsa ve If-Range istek üst bilgisi alanı belirtilmemişse döndürülür.

Değer|Sonuç
-|-
Etkin|Pop 'Ların bir 416 Istenen aralığı Satisfiable durum kodu ile geçersiz bir bayt aralığı isteğine yanıt vermesini engeller. Bunun yerine sunucular istenen varlığı teslim eder ve istemciye bir 200 Tamam döndürür.
Devre dışı|Varsayılan davranışı geri yükler. Varsayılan davranış, 416 Istenen aralığı Satisfiable durum kodu olarak kabul etmek için kullanılır.

**Varsayılan davranış:** Devre dışı.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="internal-max-stale"></a>İç maksimum-eski

**Amaç:** AÇıLAN bir varlık, önbelleğe alınmış varlığı kaynak sunucu ile yeniden doğrulayamamışsa, ön belleğe alınmış bir varlığın bir POP 'tan ne kadar süre geçmiş olduğunu denetler.

Genellikle, bir varlığın en fazla kullanım süresi sona erdiğinde, POP, kaynak sunucuya yeniden doğrulama isteği gönderir. Kaynak sunucu daha sonra, önbelleğe alınmış varlık üzerinde yeni bir kira sağlamak için değiştirilmemiş bir 304 ile yanıt verir veya POP 'u önbelleğe alınmış varlığın güncelleştirilmiş bir sürümüyle birlikte sunacak şekilde 200 Tamam ile yeniden dener.

POP, bu tür bir yeniden doğrulamaya çalışırken kaynak sunucuyla bağlantı kuramazsa, bu dahili maksimum-eski özelliği, POP 'un şimdi eski varlığa ne kadar süreyle çalışmaya devam edemeyeceğini denetler.

Bu zaman aralığının, başarısız yeniden doğrulama gerçekleşmediğinde değil, varlığın en fazla ömrü sona erdiğinde başlayacağını unutmayın. Bu nedenle, bir varlığın başarılı bir yeniden doğrulama olmadan sunulabilecek en uzun süre, maksimum yaş ve maksimum-eski birleşimi ile belirtilen sürenin miktarıdır. Örneğin, bir varlık en fazla 30 dakikalık ve en fazla 15 dakikalık bir olan 9:00 ' de önbelleğe alınmışsa, 9:44 ' de başarısız bir yeniden doğrulama denemesi, son kullanıcının bir 504 Gateway zaman aşımı süresi almasına neden olacak şekilde, son kullanıcının eski önbelleğe alınmış 9:46 varlığı almasına neden olur.

Bu özellik için yapılandırılmış herhangi bir değer, yerine `Cache-Control: must-revalidate` veya `Cache-Control: proxy-revalidate` kaynak sunucudan alınan üstlerdir. Bir varlık başlangıçta önbelleğe alındıktan sonra kaynak sunucudan bu üst bilgilerden biri alınmışsa, POP eski önbelleğe alınmış bir varlığa sahip olmaz. Böyle bir durumda, varlığın maksimum yaş aralığının süresi dolduğunda POP kaynak ile yeniden doğrulanamamışsa, POP bir 504 ağ geçidi zaman aşımı hatası döndürür.

Anahtar bilgileri:

- Bu özelliği şu şekilde yapılandırın:
    - En fazla bir için uygulanacak durum kodunu seçme.
    - Bir tamsayı değeri belirtip istenen zaman birimini seçin (örneğin, saniye, dakika, saat, vb.). Bu değer, uygulanacak iç maksimum-eski değeri tanımlar.

- Zaman biriminin "kapalı" olarak ayarlanması, bu özelliği devre dışı bırakır. Önbelleğe alınmış bir varlık, normal sona erme zamanından daha fazla sunulmayacak.

**Varsayılan davranış:** İki dakika

#### <a name="compatibility"></a>Uyumluluk

Önbellek ayarlarının izlenme şekli nedeniyle, bu özellik aşağıdaki eşleşme koşullarıyla ilişkilendirilemez:
- AS numarası
- İstemci IP Adresi
- Cookie parametresi
- Tanımlama bilgisi parametre Regex
- Ülke
- Cihaz
- Edge CNAME
- Başvurulan etki alanı
- İstek üst bilgisi sabit değeri
- İstek üst bilgisi Regex
- İstek üstbilgisi joker karakteri
- Request Yöntemi
- İstek düzeni
- URL sorgusu sabit değeri
- URL sorgu Regex
- URL sorgusu joker karakteri
- URL sorgu parametresi

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="log-query-string"></a>Günlük sorgu dizesi

**Amaç:** Sorgu dizesinin, erişim günlüklerindeki URL ile birlikte saklanıp saklanmayacağını belirler.

Değer|Sonuç
-|-
Etkin|Bir erişim günlüğüne URL 'Leri kaydederken sorgu dizelerinin depolanmasını sağlar. Bir URL bir sorgu dizesi içermiyorsa, bu seçenek bir etkiye sahip olmaz.
Devre dışı|Varsayılan davranışı geri yükler. Varsayılan davranış, bir erişim günlüğüne URL 'Leri kaydederken sorgu dizelerini yok saymanız olur.

**Varsayılan davranış:** Devre dışı.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="maximum-keep-alive-requests"></a>Maksimum etkin tut Istekleri

**Amaç:** Kapalı bir bağlantıyı kapatmadan önce, etkin tutma bağlantısı için en fazla istek sayısını tanımlar.

En fazla istek sayısını düşük bir değere ayarlamak önerilmez ve performans düşüşüne neden olabilir.

Anahtar bilgileri:

- Bu değeri tam tam sayı olarak belirtin.
- Belirtilen değere virgül veya nokta eklemeyin.

**Varsayılan değer:** 10.000 istek

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="modify-client-request-header"></a>Istemci Isteği üst bilgisini Değiştir

**Amaç:** Her istek, onu tanımlayan bir istek üst bilgisi kümesi içerir. Bu özellik aşağıdakilerden biri olabilir:

- İstek üstbilgisine atanan değerin sonuna ekleyin veya üzerine yazın. Belirtilen istek üst bilgisi yoksa, bu özellik isteği isteğe ekler.
- İstek üst bilgisini istekten silin.

Bir kaynak sunucuya iletilen istekler, bu özellik tarafından yapılan değişiklikleri yansıtır.

Bir istek üstbilgisinde aşağıdaki eylemlerden biri gerçekleştirilebilir:

Seçenek|Açıklama|Örnek
-|-|-
Ekle|Belirtilen değer, var olan istek üst bilgisi değerinin sonuna eklenecek.|**İstek üst bilgisi değeri (istemci):**<br/>Value1<br/>**İstek üst bilgisi değeri (kural altyapısı):**<br/>Value2 <br/>**Yeni istek üst bilgisi değeri:** <br/>Value1Value2
Üzerine yaz|İstek üst bilgisi değeri belirtilen değere ayarlanacak.|**İstek üst bilgisi değeri (istemci):**<br/>Value1<br/>**İstek üst bilgisi değeri (kural altyapısı):**<br/>Value2<br/>**Yeni istek üst bilgisi değeri:**<br/> Value2 <br/>
Sil|Belirtilen istek üst bilgisini siler.|**İstek üst bilgisi değeri (istemci):**<br/>Value1<br/>**İstemci isteği üst bilgisi yapılandırmasını değiştir:**<br/>Söz konusu istek üst bilgisini silin.<br/>**Kaynaklanan**<br/>Belirtilen istek üst bilgisi, kaynak sunucuya iletilecektir.

Anahtar bilgileri:

- Ad seçeneğinde belirtilen değerin, istenen istek üst bilgisi için tam bir eşleşme olduğundan emin olun.
- Büyük/küçük harf tanımlama durumu dikkate alınmaz. Örneğin, aşağıdaki `Cache-Control` üst bilgi adı varyasyonları tanımlamak için kullanılabilir:
    - Cache-Control
    - CACHE-CONTROL
    - cachE-Control
- Bir üst bilgi adı belirtirken yalnızca alfasayısal karakter, kısa çizgi veya alt çizgi kullanın.
- Üst bilgi silindiğinde, pop 'Ların bir kaynak sunucuya iletilmesi önlenir.
- Aşağıdaki üstbilgiler ayrılmıştır ve bu özellik tarafından değiştirilemez:
    - lirse
    - konak
    - yazıcısıyla
    - warning
    - x-iletilmiş-için
    - "X-EC" ile başlayan tüm üst bilgi adları ayrılmıştır.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="modify-client-response-header"></a>Istemci yanıtı başlığını Değiştir

Her yanıt, onu tanımlayan bir dizi yanıt üst bilgisi içerir. Bu özellik aşağıdakilerden biri olabilir:

- Bir yanıt üstbilgisine atanan değerin sonuna ekleyin veya üzerine yazın. Belirtilen yanıt üst bilgisi yoksa, bu özellik yanıta ekler.
- Yanıttan bir yanıt üst bilgisini silin.

Varsayılan olarak, yanıt üst bilgisi değerleri bir kaynak sunucu ve pop 'Lar tarafından tanımlanır.

Yanıt üst bilgisinde aşağıdaki eylemlerden biri gerçekleştirilebilir:

Seçenek|Açıklama|Örnek
-|-|-
Ekle|Belirtilen değer, var olan yanıt üst bilgisi değerinin sonuna eklenecektir.|**Yanıt üst bilgisi değeri (istemci):**<br />Value1<br/>**Yanıt üst bilgisi değeri (kural altyapısı):**<br/>Value2<br/>**Yeni yanıt üst bilgisi değeri:**<br/>Value1Value2
Üzerine yaz|Yanıt üst bilgisi değeri belirtilen değere ayarlanacak.|**Yanıt üst bilgisi değeri (istemci):**<br/>Value1<br/>**Yanıt üst bilgisi değeri (kural altyapısı):**<br/>Value2 <br/>**Yeni yanıt üst bilgisi değeri:**<br/>Value2 <br/>
Sil|Belirtilen yanıt üst bilgisini siler.|**Yanıt üst bilgisi değeri (istemci):**<br/>Value1<br/>**İstemci yanıt üst bilgisi yapılandırmasını değiştir:**<br/>Söz konusu yanıt üst bilgisini silin.<br/>**Kaynaklanan**<br/>Belirtilen yanıt üst bilgisi, istek sahibine iletilmeyecek.

Anahtar bilgileri:

- Ad seçeneğinde belirtilen değerin, istenen yanıt üst bilgisi için tam bir eşleşme olduğundan emin olun.
- Büyük/küçük harf tanımlama durumu dikkate alınmaz. Örneğin, aşağıdaki `Cache-Control` üst bilgi adı varyasyonları tanımlamak için kullanılabilir:
    - Cache-Control
    - CACHE-CONTROL
    - cachE-Control
- Üst bilgi silindiğinde, istek sahibine iletilmesini önler.
- Aşağıdaki üstbilgiler ayrılmıştır ve bu özellik tarafından değiştirilemez:
    - kabul etme-kodlama
    - yaş
    - bağlantı
    - İçerik kodlama
    - içerik uzunluğu
    - içerik aralığı
    - date
    - sunucu
    - Trey
    - aktarım kodlaması
    - yükseltme
    - değiş
    - yazıcısıyla
    - warning
    - "X-EC" ile başlayan tüm üst bilgi adları ayrılmıştır.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="partial-cache-sharing"></a>Kısmi önbellek paylaşımı

**Amaç:** Bir isteğin kısmen önbelleğe alınmış içerik oluşturup üretmeyeceğini belirler.

Bu kısmi önbellek daha sonra, istenen içerik tamamen önbelleğe alınana kadar bu içerik için yeni istekleri karşılamak üzere kullanılabilir.

Değer|Sonuç
-|-
Etkin|İstekler kısmen önbelleğe alınmış içerik oluşturabilir.
Devre dışı|İstekler, istenen içeriğin yalnızca tam önbelleğe alınmış bir sürümünü oluşturabilir.

**Varsayılan davranış:** Devre dışı.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="prevalidate-cached-content"></a>Önbelleğe alınmış Içeriği önceden doğrula

**Amaç:** Önbelleğe alınan içeriğin TTL 'nin süresi dolmadan erken yeniden doğrulama için uygun olup olmayacağını belirler.

Bir erken yeniden doğrulama için uygun olacağı, istenen içeriğin TTL 'SI dolmadan önce geçen süreyi tanımlayın.

Anahtar bilgileri:

- Zaman birimi olarak "kapalı" seçeneğinin belirlenmesi, önbelleğe alınmış içeriğin TTL 'SI dolduktan sonra yeniden doğrulamanın gerçekleşmesini gerektirir. Saat belirtilmemelidir ve yoksayıldı.

**Varsayılan davranış:** Dışına. Yeniden doğrulama işlemi yalnızca önbelleğe alınan içeriğin süresi dolduktan sonra gerçekleşebilir.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="proxy-special-headers"></a>Proxy özel üstbilgileri

**Amaç:** Bir POP 'tan kaynak sunucuya iletilecek [Verizon 'e özgü http istek üst bilgileri](cdn-verizon-http-headers.md) kümesini tanımlar.

Anahtar bilgileri:

- Bu özellikte tanımlanan her bir CDN 'e özgü istek üst bilgisi bir kaynak sunucuya iletilir. Dışlanan üstbilgiler iletilmez.
- CDN 'e özgü bir istek üstbilgisinin iletilmesini engellemek için, üst bilgi listesi alanındaki alana ayrılmış listeden kaldırın.

Aşağıdaki HTTP üstbilgileri varsayılan listeye dahil edilmiştir:
- Yazıcısıyla
- X-Iletilmiş-Için
- X-Iletilen-proto
- X-ana bilgisayar
- X-Midgress
- X-Gateway-List
- X-EC-adı
- Ana bilgisayar

**Varsayılan davranış:** Tüm CDN 'e özgü istek üstbilgileri, kaynak sunucuya iletilir.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="refresh-zero-byte-cache-files"></a>Sıfır baytlık önbellek dosyalarını Yenile

**Amaç:** Bir HTTP istemcisinin 0 baytlık önbellek varlığı için nasıl bir istek pop tarafından işleneceğini belirler.

Geçerli değerler:

Değer|Sonuç
--|--
Etkin|POP 'un kaynak sunucudan varlık tekrar al çalışmasına neden olur.
Devre dışı|Varsayılan davranışı geri yükler. Varsayılan davranış, istek üzerine geçerli önbellek varlıklarını sunacak.

Bu özellik, doğru önbelleğe alma ve içerik teslimi için gerekli değildir, ancak geçici bir çözüm olarak yararlı olabilir. Örneğin, kaynak sunuculardaki dinamik içerik oluşturucuları, bu pop 'Lara yanlışlıkla 0 baytlık yanıt gönderilmesine neden olabilir. Bu yanıt türleri genellikle pop 'Lar tarafından önbelleğe alınır. Bu tür içerikler için 0 baytlık bir yanıtın hiçbir şekilde geçerli yanıt olduğunu biliyorsanız, bu özellik bu varlık türlerinin istemcilerinize sunulmasını engelleyebilir.

**Varsayılan davranış:** Devre dışı.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="set-cacheable-status-codes"></a>Önbelleklenebilir durum kodlarını ayarlama

**Amaç:** Önbelleğe alınmış içerikle sonuçlanabilme durum kodları kümesini tanımlar.

Varsayılan olarak, önbelleğe alma yalnızca 200 Tamam yanıt için etkindir.

İstenen durum kodlarının boşlukla ayrılmış bir kümesini tanımlayın.

Anahtar bilgileri:

- Önbellek yok saymayı yoksay özelliğini etkinleştirin. Bu özellik etkinleştirilmemişse, 200 olmayan Tamam yanıtı önbelleğe alınmamış olabilir.
- Bu özellik için geçerli durum kodları kümesi şunlardır: 203, 300, 301, 302, 305, 307, 400, 401, 402, 403, 404, 405, 406, 407, 408, 409, 410, 411, 412, 413, 414, 415, 416, 417, 500, 501, 502, 503, 504 ve 505.
- Bu özellik, 200 Tamam durum kodu üreten yanıtlar için önbelleğe almayı devre dışı bırakmak için kullanılamaz.

**Varsayılan davranış:** Önbelleğe alma yalnızca 200 OK durum kodu oluşturan yanıtlar için etkindir.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="set-client-ip-custom-header"></a>Istemci IP özel üstbilgisini ayarla

**Amaç:** İstek için istenen istemciyi IP adresine göre tanımlayan bir özel üst bilgi ekler.

Üstbilgi adı seçeneği, istemcinin IP adresinin depolandığı özel istek üstbilgisinin adını tanımlar.

Bu özellik, bir müşteri kaynak sunucusunun özel bir istek üst bilgisi aracılığıyla istemci IP adreslerini bulmasını sağlar. İstek önbellekten sunulduysa, kaynak sunucuya istemcinin IP adresi bilgilendirilmeyecektir. Bu nedenle, bu özelliğin önbelleğe alınmamış varlıklarla kullanılması önerilir.

Belirtilen üst bilgi adının aşağıdaki adlarla eşleşmediğinden emin olun:

- Standart istek üst bilgisi adları. Standart üstbilgi adlarının bir listesi, [RFC 2616](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)' de bulunabilir.
- Ayrılmış üst bilgi adları:
    - iletilen
    - konak
    - değiş
    - yazıcısıyla
    - warning
    - x-iletilmiş-için
    - "X-EC" ile başlayan tüm üst bilgi adları ayrılmıştır.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="stale-content-delivery-on-error"></a>Hatada eski Içerik teslimi

**Amaç:** Önbellek yeniden doğrulama işlemi sırasında veya müşterinin kaynak sunucusundan istenen içerik alınırken bir hata oluştuğunda, süre dolma önbelleğe alınmış içeriğin teslim edilip edilmeyeceğini belirler.

Değer|Sonuç
-|-
Etkin|Kaynak sunucuya bağlantı sırasında hata oluştuğunda, eski içerik istek sahibine sunulur.
Devre dışı|Kaynak sunucunun hatası, istek sahibine iletilir.

**Varsayılan davranış:** Devre dışı

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="stale-while-revalidate"></a>Yeniden doğrulama eski

**Amaç:** , Yeniden doğrulama gerçekleştiğinde pop 'Ların eski içerikleri istek sahibine sunması için performansı geliştirir.

Anahtar bilgileri:

- Bu özelliğin davranışı, seçilen zaman birimine göre farklılık gösterir.
    - **Zaman birimi:** Eski içerik teslimine izin vermek için bir süre belirtin ve bir zaman birimi (örneğin, saniye, dakika, saat, vb.) seçin. Bu tür bir kurulum, CDN 'nin, aşağıdaki formüle göre doğrulama gerektirmeksizin içerik teslim etmesi için gereken süreyi genişlemesine olanak tanır: TTL, **TTL**  +  **yeniden doğrulama zamanı**
    - **Kapalı:** Eski içerik için bir istek sunulmadan önce yeniden doğrulama gerektirmek için "kapalı" seçeneğini belirleyin.
        - Uygulanmadığından ve yok sayılacak bu yana zaman uzunluğu belirtmeyin.

**Varsayılan davranış:** Dışına. İstenen içeriğe sunulabilecek şekilde yeniden doğrulama gerçekleşmelidir.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="token-auth"></a>Belirteç kimlik doğrulaması

**Amaç:** Belirteç tabanlı kimlik doğrulamasının bir isteğe uygulanıp uygulanmayacağını belirler.

Belirteç tabanlı kimlik doğrulaması etkinleştirilirse, yalnızca şifrelenmiş belirteç sağlayan ve bu belirteç tarafından belirtilen gereksinimlere uyan istekler kabul edilir.

Belirteç değerlerini şifrelemek ve şifrelerini çözmek için kullanılan şifreleme anahtarı, birincil anahtar ve belirteç kimlik doğrulaması sayfasında yedekleme anahtarı seçenekleri tarafından belirlenir. Şifreleme anahtarlarının platforma özgü olduğunu aklınızda bulundurun.

**Varsayılan davranış:** Devre dışı.

Bu özellik, URL yeniden yazma özelliğinin özel durumu ile birçok özelliğin öncelikli olduğunu alır.

Değer | Sonuç
------|---------
Etkin | Belirteç tabanlı kimlik doğrulamasıyla istenen içeriği korur. Yalnızca geçerli bir belirteç sağlayan ve gereksinimlerini karşılayan istemcilerden gelen istekler kabul edilir. FTP işlemleri belirteç tabanlı kimlik doğrulamasından çıkarılır.
Devre dışı| Varsayılan davranışı geri yükler. Varsayılan davranış, belirteç tabanlı kimlik doğrulama yapılandırmanızın bir isteğin güvenliğinin sağlanıp sağlanmadığını belirlemesine izin vermeyecektir.

#### <a name="compatibility"></a>Uyumluluk

Belirteç kimlik doğrulamasını her zaman eşleştirme durumuyla birlikte kullanmayın.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="token-auth-denial-code"></a>Belirteç kimlik doğrulaması reddi kodu

**Amaç:** Belirteç tabanlı kimlik doğrulaması nedeniyle bir istek reddedildiğinde kullanıcıya döndürülecek yanıtın türünü belirler.

Kullanılabilir yanıt kodları aşağıdaki tabloda listelenmiştir.

Yanıt Kodu|Yanıt adı|Açıklama
-------------|-------------|--------
301|Kalıcı olarak taşındı|Bu durum kodu, yetkisiz kullanıcıları konum üstbilgisinde belirtilen URL 'ye yönlendirir.
302|Bulundu|Bu durum kodu, yetkisiz kullanıcıları konum üstbilgisinde belirtilen URL 'ye yönlendirir. Bu durum kodu, yeniden yönlendirme gerçekleştirmeye yönelik sektör standardı yöntemidir.
307|Geçici yeniden yönlendirme|Bu durum kodu, yetkisiz kullanıcıları konum üstbilgisinde belirtilen URL 'ye yönlendirir.
401|Yetkisiz|Bu durum kodunu WWW-Authenticate Yanıt üstbilgisiyle birleştirmek, kullanıcıdan kimlik doğrulaması yapmasını ister.
403|Yasak|Bu ileti, yetkisiz bir kullanıcının korumalı içeriğe erişmeye çalışırken göreceği standart 403 yasaklanmış durum iletisidir.
404|Dosya bulunamadı|Bu durum kodu, HTTP istemcisinin sunucuyla iletişim kurabildiğini, ancak istenen içeriğin bulunamadığını gösterir.

#### <a name="compatibility"></a>Uyumluluk

Belirteç kimlik doğrulama reddi kodunu, her zaman eşleştirme durumuyla birlikte kullanmayın. Bunun yerine, **Yönetim** portalı 'Nın **belirteç kimlik doğrulama** sayfasındaki **özel reddetme işleme** bölümünü kullanın. Daha fazla bilgi için bkz. [belirteç kimlik doğrulamasıyla Azure CDN varlıkların güvenliğini sağlama](cdn-token-auth.md).

#### <a name="url-redirection"></a>URL yeniden yönlendirme

Bu özellik, bir 3xx durum kodu döndürecek şekilde yapılandırıldığında Kullanıcı tanımlı URL 'ye URL yeniden yönlendirmeyi destekler. Aşağıdaki adımlar gerçekleştirilirken bu kullanıcı tanımlı URL belirtilebilir:

1. Belirteç kimlik doğrulama kodu çevirme özelliği için bir 3xx yanıt kodu seçin.
2. Isteğe bağlı üstbilgi adı seçeneğinde "konum" ı seçin.
3. Isteğe bağlı üst bilgi değeri seçeneğini istenen URL 'ye ayarlayın.

Bir URL bir 3xx durum kodu için tanımlanmamışsa, kullanıcıya bir 3xx durum kodu için Standart yanıt sayfası döndürülür.

URL yönlendirmesi yalnızca 3xx yanıt kodları için geçerlidir.

Isteğe bağlı üstbilgi değeri seçeneği alfasayısal karakterleri, tırnak işaretlerini ve boşlukları destekler.

#### <a name="authentication"></a>Kimlik Doğrulaması

Bu özellik, belirteç tabanlı kimlik doğrulamasıyla korunan içeriğe yetkisiz bir isteğe yanıt vermediğinde WWW-Authenticate üst bilgisini dahil etme özelliğini destekler. Yapılandırmanızda WWW-Authenticate üstbilgisi "temel" olarak ayarlandıysa, yetkisiz kullanıcıdan hesap kimlik bilgileri istenir.

Yukarıdaki yapılandırma aşağıdaki adımları gerçekleştirerek elde edilebilir:

1. Belirteç kimlik doğrulaması kod çevirme özelliği için yanıt kodu olarak "401" seçeneğini belirleyin.
2. Isteğe bağlı üstbilgi adı seçeneğinde "WWW-Authenticate" seçeneğini belirleyin.
3. Isteğe bağlı üst bilgi değeri seçeneğini "temel" olarak ayarlayın.

WWW-Authenticate üstbilgisi yalnızca 401 yanıt kodu için geçerlidir.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="token-auth-ignore-url-case"></a>Belirteç kimlik doğrulaması URL 'sini yoksay

**Amaç:** Belirteç tabanlı kimlik doğrulaması tarafından yapılan URL karşılaştırmalarının büyük/küçük harfe duyarlı olup olmadığını belirler.

Bu özellikten etkilenen parametreler şunlardır:

- ec_url_allow
- ec_ref_allow
- ec_ref_deny

Geçerli değerler:

Değer|Sonuç
---|----
Etkin|Belirteç tabanlı kimlik doğrulama parametrelerine yönelik URL 'Leri karşılaştırırken POP 'un büyük/küçük harf durumunu yoksaymasına neden olur.
Devre dışı|Varsayılan davranışı geri yükler. Varsayılan davranış, belirteç kimlik doğrulaması için URL karşılaştırmalarının büyük/küçük harfe duyarlı olmasını sağlar.

**Varsayılan davranış:** Devre dışı.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="token-auth-parameter"></a>Belirteç kimlik doğrulaması parametresi

**Amaç:** Belirteç tabanlı kimlik doğrulama sorgu dizesi parametresinin yeniden adlandırılması gerekip gerekmediğini belirler.

Anahtar bilgileri:

- Değer seçeneği, bir belirtecin belirtibileceği sorgu dizesi parametre adını tanımlar.
- Değer seçeneği "ec_token" olarak ayarlanamaz.
- Değer seçeneğinde tanımlanan adın yalnızca geçerli URL karakterleri içerdiğinden emin olun.

Değer|Sonuç
----|----
Etkin|Değer seçeneği, belirteçlerin tanımlanması gereken sorgu dizesi parametre adını tanımlar.
Devre dışı|Belirteç, istek URL 'sinde tanımsız bir sorgu dizesi parametresi olarak belirtilebilir.

**Varsayılan davranış:** Devre dışı. Belirteç, istek URL 'sinde tanımsız bir sorgu dizesi parametresi olarak belirtilebilir.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="url-redirect"></a>URL yeniden yönlendirme

**Amaç:** İstekleri konum üst bilgisi aracılığıyla yeniden yönlendirir.

Bu özelliğin yapılandırması için aşağıdaki seçenekleri ayarlamak gerekir:

Seçenek|Açıklama
-|-
Kod|İstek sahibine döndürülecek yanıt kodunu seçin.
Kaynak & kalıbı| Bu ayarlar, yeniden yönlendirilebilolabilecek isteklerin türünü tanımlayan bir istek URI 'SI deseninin tanımlar. Yalnızca URL 'SI aşağıdaki ölçütlerin her ikisini de karşılayan istekler yeniden yönlendirilir: <br/> <br/> **Kaynak (veya içerik erişim noktası):** Kaynak sunucuyu tanımlayan göreli bir yol seçin. Bu yol _/xxxx/_ bölümü ve uç nokta adıdır. <br/><br/> **Kaynak (model):** İstekleri göreli yola göre tanımlayan bir düzenin tanımlanması gerekir. Bu normal ifade deseninin önceden seçilmiş içerik erişim noktası (yukarıya bakın) sonrasında doğrudan başlayan bir yol tanımlanmalıdır. <br/> -Daha önce tanımlanan istek URI ölçütlerinin (yani, kaynak & deseninin) Bu özellik için tanımlanan tüm eşleşme koşulları ile çakışmadığından emin olun. <br/> -Bir model belirtin; model olarak boş bir değer kullanırsanız, tüm dizeler eşleştirilir.
Hedef| Yukarıdaki isteklerin yönlendirileceği URL 'YI tanımlayın. <br/><br/> Bu URL 'YI kullanarak dinamik olarak oluşturun: <br/> -Normal ifade deseninin <br/>- [HTTP değişkenleri](cdn-http-variables.md) <br/><br/> $_N_ ' i kullanarak kaynak modelinde yakalanan değerleri hedef düzenine koyun. burada _n_ , bir değeri yakalandığı sıraya göre tanımlar. Örneğin, $1, kaynak modelinde yakalanan ilk değeri temsil ederken, $2 ikinci değeri temsil eder. <br/>

Mutlak bir URL kullanmanız önemle önerilir. Göreli URL kullanımı, CDN URL 'Lerini geçersiz bir yola yönlendirebilir.

**Örnek senaryo**

Bu örnek, bu temel CDN URL 'sine çözümlenen bir Edge CNAME URL 'sini yeniden yönlendirmeyi gösterir: http: \/ /Marketing.azureedge.net/Brochures

Uygun istekler bu temel kenar CNAME URL 'sine yönlendirilecek: http: \/ /CDN.mydomain.com/resources

Bu URL yeniden yönlendirmesi şu yapılandırma ile sağlanabilir: ![ URL yeniden yönlendirme](./media/cdn-rules-engine-reference/cdn-rules-engine-redirect.png)

**Anahtar noktaları:**

- URL yeniden yönlendirme özelliği, yönlendirileceği istek URL 'Lerini tanımlar. Sonuç olarak, ek eşleşme koşulları gerekli değildir. Eşleşme koşulu "Always" olarak tanımlansa da, yalnızca "pazarlama" müşteri başlangıcının "broşürler" klasörüne işaret eden istekler yeniden yönlendirilir.
- Tüm eşleşen istekler, hedef seçeneğinde tanımlanan Edge CNAME URL 'sine yeniden yönlendirilir.
    - Örnek Senaryo #1:
        - Örnek istek (CDN URL 'SI): http: \/ /Marketing.azureedge.net/Brochures/widgets.PDF
        - İstek URL 'SI (yeniden yönlendirdikten sonra): http: \/ /CDN.mydomain.com/resources/widgets.PDF  
    - Örnek Senaryo #2:
        - Örnek istek (Edge CNAME URL 'SI): http: \/ /Marketing.mydomain.com/Brochures/widgets.PDF
        - İstek URL 'SI (yeniden yönlendirdikten sonra): http: \/ /CDN.mydomain.com/resources/widgets.PDF örnek senaryosu
    - Örnek Senaryo #3:
        - Örnek istek (Edge CNAME URL 'SI): http: \/ /Brochures.mydomain.com/campaignA/final/productC.ppt
        - İstek URL 'SI (yeniden yönlendirdikten sonra): http: \/ /CDN.mydomain.com/resources/campaignA/final/productC.ppt 
- Istek düzeni (% {Scheme}) değişkeni, hedef seçeneğinde yararlanılabilir, bu da isteğin düzeninin yeniden yönlendirmeden sonra değişmeden kalmasını sağlar.
- İstekten yakalanan URL kesimleri, "$1" aracılığıyla yeni URL 'ye eklenir.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="url-rewrite"></a>URL Yeniden Yazma

**Amaç:** İstek URL 'sini yeniden yazar.

Anahtar bilgileri:

- Bu özelliğin yapılandırması için aşağıdaki seçenekleri ayarlamak gerekir:

Seçenek|Açıklama
-|-
 Kaynak & kalıbı | Bu ayarlar, yeniden yazılabilir olabilecek isteklerin türünü tanımlayan bir istek URI 'SI deseninin tanımlar. Yalnızca URL 'SI aşağıdaki ölçütlerin her ikisini de karşılayan istekler yeniden yazılır: <br/><br/>  - **Kaynak (veya içerik erişim noktası):** Kaynak sunucuyu tanımlayan göreli bir yol seçin. Bu yol _/xxxx/_ bölümü ve uç nokta adıdır. <br/><br/> - **Kaynak (model):** İstekleri göreli yola göre tanımlayan bir düzenin tanımlanması gerekir. Bu normal ifade deseninin önceden seçilmiş içerik erişim noktası (yukarıya bakın) sonrasında doğrudan başlayan bir yol tanımlanmalıdır. <br/> Daha önce tanımlanan istek URI ölçütlerinin (yani, kaynak & deseninin) Bu özellik için tanımlanan eşleşme koşullarından hiçbiriyle çakışmadığını doğrulayın. Bir model belirtin; model olarak boş bir değer kullanırsanız, tüm dizeler eşleştirilir.
 Hedef  |Yukarıdaki isteklerin üzerine yazılması için göreli URL 'YI tanımlayın: <br/>    1. bir kaynak sunucuyu tanımlayan bir içerik erişim noktası seçme. <br/>    2. şunu kullanarak göreli bir yol tanımlama: <br/>        -Normal ifade deseninin <br/>        - [HTTP değişkenleri](cdn-http-variables.md) <br/> <br/> $_N_ ' i kullanarak kaynak modelinde yakalanan değerleri hedef düzenine koyun. burada _n_ , bir değeri yakalandığı sıraya göre tanımlar. Örneğin, $1, kaynak modelinde yakalanan ilk değeri temsil ederken, $2 ikinci değeri temsil eder.

 Bu özellik, pop 'Ların geleneksel yeniden yönlendirme gerçekleştirmeden URL 'YI yeniden yazması için izin verir. Diğer bir deyişle, istek sahibi URL 'SI istenmiş olarak aynı yanıt kodunu alır.

**Örnek Senaryo 1**

Bu örnek, bu temel CDN URL 'sine çözümlenen bir Edge CNAME URL 'sini yeniden yönlendirmeyi gösterir: http: \/ /Marketing.azureedge.net/Brochures/

Uygun istekler bu temel kenar CNAME URL 'sine yönlendirilecek: http: \/ /MyOrigin.azureedge.net/Resources/

Bu URL yeniden yönlendirmesi şu yapılandırma ile sağlanabilir: ![ URL yeniden yönlendirme](./media/cdn-rules-engine-reference/cdn-rules-engine-rewrite.png)

**Örnek Senaryo 2**

Bu örnek, bir Edge CNAME URL 'sini normal ifadeler kullanılarak büyük harfe küçük harfe nasıl yönlendirebileceğinizi gösterir.

Bu URL yeniden yönlendirmesi şu yapılandırma ile sağlanabilir: ![ URL yeniden yönlendirme](./media/cdn-rules-engine-reference/cdn-rules-engine-to-lowercase.png)

**Anahtar noktaları:**

- URL yeniden yazma özelliği, yeniden yazılması için istek URL 'Lerini tanımlar. Sonuç olarak, ek eşleşme koşulları gerekli değildir. Eşleşme koşulu "her zaman" olarak tanımlansa da, yalnızca "pazarlama" müşteri başlangıcının "broşürler" klasörüne işaret eden istekler yeniden yazılır.

- İstekten yakalanan URL kesimleri, "$1" aracılığıyla yeni URL 'ye eklenir.

#### <a name="compatibility"></a>Uyumluluk

Bu özellik, bir isteğe uygulanmadan önce karşılanması gereken eşleştirme ölçütlerini içerir. Çakışan eşleşme ölçütlerini ayarlamayı engellemek için, bu özellik aşağıdaki eşleşme koşullarıyla uyumlu değildir:

- AS numarası
- CDN kaynağı
- İstemci IP Adresi
- Müşteri kaynağı
- İstek düzeni
- URL yolu dizini
- URL yolu uzantısı
- URL yolu dosya adı
- URL yolu sabit değeri
- URL yolu Regex
- URL yolu joker karakteri
- URL sorgusu sabit değeri
- URL sorgu parametresi
- URL sorgu Regex
- URL sorgusu joker karakteri

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="user-variable"></a>Kullanıcı değişkeni

**Amaç:** Yalnızca iç kullanım içindir.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)
## <a name="next-steps"></a>Sonraki adımlar

- [Kural altyapısı başvurusu](cdn-verizon-premium-rules-engine-reference.md)
- [Kural altyapısı koşullu ifadeleri](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Kural altyapısı eşleştirme koşulları](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [Kural altyapısını kullanarak HTTP davranışını geçersiz kılma](cdn-verizon-premium-rules-engine.md)
- [Azure CDN genel bakış](cdn-overview.md)
