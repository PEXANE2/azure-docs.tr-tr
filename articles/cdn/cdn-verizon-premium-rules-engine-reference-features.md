---
title: Verizon Premium kuralları motor özellikleri nden Azure CDN | Microsoft Dokümanlar
description: Verizon Premium kuralları motor özelliklerinden Azure CDN için başvuru belgeleri.
services: cdn
author: asudbring
ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: allensu
ms.openlocfilehash: 373e7838327d11b1b54278ee0c16c6e6ae554b0b
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81253501"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-features"></a>Verizon Premium kuralları motor özelliklerinden Azure CDN

Bu makalede, Azure İçerik Teslim Ağı (CDN) [Kuralları Altyapısı](cdn-verizon-premium-rules-engine.md)için kullanılabilir özelliklerin ayrıntılı açıklamaları listelenmektedir.

Kuralın üçüncü bölümü özelliktir. Özellik, bir dizi eşleşme koşuluyla tanımlanan istek türüne uygulanan eylem türünü tanımlar.

## <a name="access-features"></a>Erişim özellikleri

Bu özellikler içeriğe erişimi denetlemek için tasarlanmıştır.

Adı | Amaç
-----|--------
[Erişimi Reddet (403)](#deny-access-403) | Tüm isteklerin 403 Forbidden yanıtıyla reddedilip reddedilmediğini belirler.
[Token Auth](#token-auth) | Bir istekiçin Belirteç Tabanlı Kimlik Doğrulama'nın uygulanıp uygulanmadığını belirler.
[Token Auth İnkar Kodu](#token-auth-denial-code) | Belirteç tabanlı kimlik doğrulaması nedeniyle istek reddedildiğinde kullanıcıya döndürülen yanıt türünü belirler.
[Token Auth Göz ardı URL Örneği](#token-auth-ignore-url-case) | Token Tabanlı Kimlik Doğrulama tarafından yapılan URL karşılaştırmalarının büyük/küçük harf duyarlı olup olmadığını belirler.
[Token Auth Parametresi](#token-auth-parameter) | Belirteç Tabanlı Kimlik Doğrulama sorgu dize parametresinin yeniden adlandırılıp adlandırılmayacağını belirler.

## <a name="caching-features"></a>Önbelleğe alma özellikleri

Bu özellikler, içeriğin önbelleğe ne zaman ve nasıl önbelleğe alınmış olduğunu özelleştirmek için tasarlanmıştır.

Adı | Amaç
-----|--------
[Bant Genişliği Parametreleri](#bandwidth-parameters) | Bant genişliği azaltma parametrelerinin (örneğin, ec_rate ve ec_prebuf) etkin olup olmadığını belirler.
[Bant Genişliği Azaltma](#bandwidth-throttling) | Durum noktası (POP) tarafından sağlanan yanıt için bant genişliğini daraltır.
[Önbellek'i Atlama](#bypass-cache) | İsteğin önbelleğe alma atlayıp atlamayacağına karar ver.
[Önbellek-Kontrol Başlığı Tedavisi](#cache-control-header-treatment) | Harici Max-Age özelliği etkin olduğunda POP tarafından `Cache-Control` üstbilgi üretimini denetler.
[Önbellek Anahtarı Sorgu Dizesi](#cache-key-query-string) | Önbellek anahtarının istekle ilişkili sorgu dize parametrelerini işeyleyip içermediğini veya dışlayıp kapsamadığını belirler.
[Önbellek Anahtarı Yeniden Yazma](#cache-key-rewrite) | Bir istekle ilişkili önbellek anahtarını yeniden yazar.
[Önbellek Dolgusu Tamamlandı](#complete-cache-fill) | Bir istek POP'ta kısmi önbellek kaçırmayla sonuçlandığında ne olacağını belirler.
[Dosya Türlerini Sıkıştır](#compress-file-types) | Sunucuda sıkıştırılmış dosyaların dosya biçimlerini tanımlar.
[Varsayılan Dahili Max-Age](#default-internal-max-age) | POP'un başlangıç sunucusu önbelleği yeniden validemi için varsayılan maksimum yaş aralığını belirler.
[Sona Erer Başlık Tedavisi](#expires-header-treatment) | Harici Max-Age özelliği etkin olduğunda `Expires` üstbilgi üretimini POP ile denetler.
[Dış Max-Age](#external-max-age) | Tarayıcının POP önbelleği yeniden validasyonuna kadar olan maksimum yaş aralığını belirler.
[Kuvvet İç Max-Age](#force-internal-max-age) | POP'un başlangıç sunucusu önbelleği yeniden validemi için maksimum yaş aralığını belirler.
[H.264 Desteği (HTTP Aşamalı İndir)](#h264-support-http-progressive-download) | İçerik akışı için kullanılabilecek H.264 dosya biçimlerinin türlerini belirler.
[Onur No-Önbellek İsteği](#honor-no-cache-request) | HTTP istemcisinin önbellek yok isteklerinin kaynak sunucuya iletilip iletilmediğini belirler.
[Kaynak Yokyokla No-Önbellek](#ignore-origin-no-cache) | CDN'nin bir kaynak sunucusundan sunulan belirli yönergeleri yoksayıp yoksaymadığını belirler.
[Satatisfiable Aralıkları Yoksay](#ignore-unsatisfiable-ranges) | İstem 416 İstenilen Aralık Tatmin Edilemeyen Bir Durum Kodu oluşturduğunda istemcilere döndürülen yanıtı belirler.
[Dahili Max-Bayat](#internal-max-stale) | POP önbelleğe alınan varlığı niçin kaynağı sunucuyla yeniden doğrulayamadığında, önbelleğe alınmış bir varlığın bir POP'tan ne kadar süreyle hizmet verebileceğini denetler.
[Kısmi Önbellek Paylaşımı](#partial-cache-sharing) | Bir isteğin kısmen önbelleğe alınmış içerik oluşturup oluşturamayacağını belirler.
[Önbelleğe Alınmış İçeriği Önceden Yoklayın](#prevalidate-cached-content) | Önbelleğe alınmış içeriğin TTL süresi dolmadan önce erken yeniden validasyon için uygun olup olmadığını belirler.
[Sıfır Bayt Önbellek Dosyalarını Yenile](#refresh-zero-byte-cache-files) | BIR HTTP istemcisinin 0 baytönbellek varlığı için isteğinin POP'lar tarafından nasıl işleneceğini belirler.
[Önbelleğe Alan Durum Kodlarını Ayarlama](#set-cacheable-status-codes) | Önbelleğe alınmış içeriğe neden olabilecek durum kodları kümesini tanımlar.
[Hata üzerine Bayat İçerik Teslimi](#stale-content-delivery-on-error) | Önbellek yeniden validemi sırasında veya müşteri kaynağı sunucusundan istenen içeriği alırken bir hata oluştuğunda süresi dolmuş önbelleğe alınmış içeriğin teslim edilip edilemeyeceğini belirler.
[Bayat Yeniden Geçersiz Kılın](#stale-while-revalidate) | Yeniden validasyon gerçekleşirken, SAHTE İstemci'nin istekte bulundurucuya bayat istemciye hizmet etmesine izin vererek performansı artırır.

## <a name="comment-feature"></a>Yorum özelliği

Bu özellik, bir kural içinde ek bilgi sağlamak için tasarlanmıştır.

Adı | Amaç
-----|--------
[Yorum](#comment) | Bir notun kural içinde eklenmesine izin verir.

## <a name="header-features"></a>Üstbilgi özellikleri

Bu özellikler, üstbilgileri istek veya yanıttan eklemek, değiştirmek veya silmek için tasarlanmıştır.

Adı | Amaç
-----|--------
[Yaş Yanıt Üstbilgi](#age-response-header) | İstekçiye gönderilen yanıtta Yaş yanıt üstbilgisinin dahil edilip edilemeyeceğini belirler.
[Hata Ayıklama Yanıt Üstbilgi](#debug-cache-response-headers) | Yanıtın, istenen varlığın önbellek ilkesi hakkında bilgi sağlayan X-EC-Hata Ayıklama yanıt üstbilgisini içerip içermeyeceğini belirler.
[İstemci İstek Üstbilgisini Değiştir](#modify-client-request-header) | Üstbilginin üzerine yazar, ekler veya üstbilgi isteğinden siler.
[İstemci Yanıt Üstbilgisini Değiştir](#modify-client-response-header) | Üstbilginin üzerine yazar, ekler veya üstbilgibir yanıtı siler.
[İstemci IP Özel Üstbilgi Ayarla](#set-client-ip-custom-header) | İstenen istemcinin IP adresinin isteğe özel istek üstbilgisi olarak eklenmesine izin verir.

## <a name="logging-features"></a>Günlük özellikleri

Bu özellikler, ham günlük dosyalarında depolanan verileri özelleştirmek için tasarlanmıştır.

Adı | Amaç
-----|--------
[Özel Günlük Alanı 1](#custom-log-field-1) | Ham günlük dosyasında özel günlük alanına atanan biçimi ve içeriği belirler.
[Günlük Sorgu Dizesi](#log-query-string) | Erişim günlüklerinde URL ile birlikte bir sorgu dizesi depolanıp depolanmadığını belirler.


<!---
## Optimize

These features determine whether a request will undergo the optimizations provided by Edge Optimizer.

Name | Purpose
-----|--------
Edge Optimizer | Determines whether Edge Optimizer can be applied to a request.
Edge Optimizer – Instantiate Configuration | Instantiates or activates the Edge Optimizer configuration associated with a site.

### Edge Optimizer
**Purpose:** Determines whether Edge Optimizer can be applied to a request.

If this feature has been enabled, then the following criteria must also be met before the request will be processed by Edge Optimizer:

- The requested content must use an edge CNAME URL.
- The edge CNAME referenced in the URL must correspond to a site whose configuration has been activated in a rule.

This feature requires the ADN platform and the Edge Optimizer feature.

Value|Result
-|-
Enabled|Indicates that the request is eligible for Edge Optimizer processing.
Disabled|Restores the default behavior. The default behavior is to deliver content over the ADN platform without any additional processing.

**Default Behavior:** Disabled


### Edge Optimizer - Instantiate Configuration
**Purpose:** Instantiates or activates the Edge Optimizer configuration associated with a site.

This feature requires the ADN platform and the Edge Optimizer feature.

Key information:

- Instantiation of a site configuration is required before requests to the corresponding edge CNAME can be processed by Edge Optimizer.
- This instantiation only needs to be performed a single time per site configuration. A site configuration that has been instantiated will remain in that state until the Edge Optimizer – Instantiate Configuration feature that references it is removed from the rule.
- The instantiation of a site configuration does not mean that all requests to the corresponding edge CNAME will automatically be processed by Edge Optimizer. The Edge Optimizer feature determines whether an individual request will be processed.

If the desired site does not appear in the list, then you should edit its configuration and verify that the Active option has been marked.

**Default Behavior:** Site configurations are inactive by default.
--->

## <a name="origin-features"></a>Menşe özellikleri

Bu özellikler, CDN'nin bir başlangıç sunucusuyla nasıl iletişim kurduğunu denetlemek için tasarlanmıştır.

Adı | Amaç
-----|--------
[Maksimum Tutma-Canlı İstekleri](#maximum-keep-alive-requests) | Kapanmadan önce Keep-Alive bağlantısı için en fazla istek sayısını tanımlar.
[Proxy Özel Başlıklar](#proxy-special-headers) | POP'tan bir başlangıç sunucusuna iletilen CDN'ye özgü istek üstbilgilerini tanımlar.

## <a name="specialty-features"></a>Özel özellikler

Bu özellikler gelişmiş kullanıcılar için gelişmiş işlevsellik sağlar.

Adı | Amaç
-----|--------
[Önbelleğe Uygun HTTP Yöntemleri](#cacheable-http-methods) | Ağda önbelleğe alınabilecek ek HTTP yöntemleri kümesini belirler.
[Önbelleğe Uygun İstek Gövde Boyutu](#cacheable-request-body-size) | POST yanıtının önbelleğe alınıp alınamayacağını belirlemek için eşiği tanımlar.
[Kullanıcı Değişkeni](#user-variable) | Yalnızca dahili kullanım içindir.

## <a name="url-features"></a>URL özellikleri

Bu özellikler, bir isteğin farklı bir URL'ye yeniden yönlendirilmesine veya yeniden yazılmasına olanak sağlar.

Adı | Amaç
-----|--------
[Yönlendirmeleri takip et](#follow-redirects) | İsteklerin müşteri kökenli bir sunucu tarafından döndürülen Konum üstbilgisinde tanımlanan ana bilgisayar ada mı yönlendirilemeyeceğini belirler.
[URL Yönlendirme](#url-redirect) | İstekleri Konum üstbilgisi üzerinden yönlendirir.
[URL Yeniden Yazma](#url-rewrite)  | İstek URL'sini yeniden yazar.

## <a name="azure-cdn-from-verizon-premium-rules-engine-features-reference"></a>Verizon Premium kuralları motorundan Azure CDN referans özellikleri

---

### <a name="age-response-header"></a>Yaş Yanıt Üstbilgi

**Amaç**: İstekçiye gönderilen yanıtta Yaş yanıtı üstbilgisinin dahil edilip edilemeyeceğini belirler.

Değer|Sonuç
--|--
Etkin | Yaş yanıt üstbilgisi, istekte bulunana gönderilen yanıtta yer alar.
Devre dışı | Yaş yanıt üstbilgisi, istekte bulundurucuya gönderilen yanıtın dışında tutulur.

**Varsayılan Davranış**: Devre dışı bırakılmış.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="bandwidth-parameters"></a>Bant Genişliği Parametreleri

**Amaç:** Bant genişliği azaltma parametrelerinin (örneğin, ec_rate ve ec_prebuf) etkin olup olmadığını belirler.

Bant genişliği azaltma parametreleri, istemcinin isteği için veri aktarım hızının özel bir oran ile sınırlı olup olmadığını belirler.

Değer|Sonuç
--|--
Etkin|POP'ların bant genişliği azaltma isteklerini yerine getirmesini sağlar.
Devre dışı|PS'lerin bant genişliği azaltma parametrelerini yoksaymasını neden olur. İstenen içerik normal olarak sunulur (diğer bir şekilde bant genişliği azaltma olmadan).

**Varsayılan Davranış:** Etkin.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="bandwidth-throttling"></a>Bant Genişliği Azaltma

**Amaç:** POP'lar tarafından sağlanan yanıt için bant genişliğini daraltır.

Bant genişliği azaltmayı düzgün bir şekilde ayarlamak için aşağıdaki seçeneklerin her ikisi de tanımlanmalıdır.

Seçenek|Açıklama
--|--
Saniyede Kbyte|Bu seçeneği yanıtı vermek için kullanılabilecek maksimum bant genişliğine (saniyede Kb) ayarlayın.
Prebuf saniye|Bu seçeneği, BANT GENIŞLIĞI azaltılana kadar bekleyen ÇEKP'lerin saniye sayısına ayarlayın. Sınırsız bant genişliği bu zaman diliminin amacı, bir medya oynatıcının bant genişliği azaltma nedeniyle kekemelik veya arabelleğe alma sorunları yaşamasını önlemektir.

**Varsayılan Davranış:** Devre dışı.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="bypass-cache"></a>Önbellek'i Atlama

**Amaç:** İsteğin önbelleğe alma atlayıp atlamayacağına karar ver.

Değer|Sonuç
--|--
Etkin|İçerik daha önce ÖNSEZ'lerde ÖNbelleğe alınmış olsa bile, tüm isteklerin başlangıç sunucusuna düşmesine neden olur.
Devre dışı|YANıT üstbilgilerinde tanımlanan önbellek ilkesine göre, SAHTE'lerin varlıkları önbelleğe almasına neden olur.

**Varsayılan Davranış:**

- **HTTP Büyük:** Devre dışı

<!---
- **ADN:** Enabled

--->

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="cacheable-http-methods"></a>Önbelleğe Uygun HTTP Yöntemleri

**Amaç:** Ağda önbelleğe alınabilecek ek HTTP yöntemleri kümesini belirler.

Önemli bilgiler:

- Bu özellik, GET yanıtlarının her zaman önbelleğe alınması gerektiğini varsayar. Sonuç olarak, bu özelliği ayarlarken GET HTTP yöntemi dahil edilmemelidir.
- Bu özellik yalnızca POST HTTP yöntemini destekler. Bu özelliği ' ye `POST`ayarlayarak POST yanıt önbelleğe alma özelliğini etkinleştirin.
- Varsayılan olarak, yalnızca gövdesi 14 Kb'dan küçük olan istekler önbelleğe alınır. Maksimum istek gövde boyutunu ayarlamak için Önbellek İste Gövde Boyutu Özelliğini kullanın.

**Varsayılan Davranış:** Yalnızca GET yanıtları önbelleğe alınır.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="cacheable-request-body-size"></a>Önbelleğe Uygun İstek Gövde Boyutu

**Amaç:** POST yanıtının önbelleğe alınıp alınamayacağını belirlemek için eşiği tanımlar.

Bu eşik, maksimum istek gövde boyutu belirtilerek belirlenir. Daha büyük bir istek gövdesi içeren istekler önbelleğe alınmaz.

Önemli bilgiler:

- Bu Özellik yalnızca POST yanıtları önbelleğe almaya uygun olduğunda geçerlidir. POST isteği önbelleğe alma özelliğini etkinleştirmek için Önbelleğe Alan HTTP Yöntemleri Özelliğini kullanın.
- İstek organı aşağıdaki ler için dikkate alınır:
    - x-www-form-urlencoded değerleri
    - Benzersiz bir önbellek anahtarı sağlama
- Büyük bir maksimum istek gövde boyutu tanımlanması veri teslim performansını etkileyebilir.
    - **Önerilen Değer:** 14 Kb
    - **Minimum Değer:** 1 Kb

**Varsayılan Davranış:** 14 Kb

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="cache-control-header-treatment"></a>Önbellek-Kontrol Başlığı Tedavisi

**Amaç:** Harici Max-Age Özelliği etkin olduğunda ÜSTBILGI oluşturmayı `Cache-Control` POP tarafından denetler.

Bu tür bir yapılandırmayı elde etmenin en kolay yolu, Harici Max-Age ve Önbellek Kontrol Üstbilgi Tedavisi özelliklerini aynı ifadeye yerleştirmektir.

Değer|Sonuç
--|--
Üzerine yaz|Aşağıdaki eylemlerin gerçekleşmesini sağlar:<br/> - Kaynak `Cache-Control` sunucusu tarafından oluşturulan üstbilginin üzerine yazar. <br/>- Yanıta `Cache-Control` Harici Max-Age özelliği tarafından üretilen üstbilgi eklenir.
Geç|Harici Max-Age özelliği tarafından üretilen üstbilginin `Cache-Control` yanıta hiçbir zaman eklenmemesini sağlar. <br/> Kaynak sunucu bir `Cache-Control` üstbilgi üretirse, son kullanıcıya geçer. <br/> Kaynak sunucu bir `Cache-Control` üstbilgi oluşturmuyorsa, bu seçenek yanıt üstbilginin `Cache-Control` üstbilgi içermemesine neden olabilir.
Eksikse Ekle|Başlangıç `Cache-Control` sunucusundan bir üstbilgi alınmadıysa, bu `Cache-Control` seçenek Harici Max-Age özelliği tarafından üretilen üstbilgiyi ekler. Bu seçenek, tüm varlıkların bir `Cache-Control` üstbilgi atanmış olmasını sağlamak için yararlıdır.
Kaldır| Bu seçenek, üstbilginin `Cache-Control` üstbilgi yanıtına dahil edilmemesini sağlar. Üstbilgi `Cache-Control` zaten atanmışsa, üstbilgi yanıtı kaldırılır.

**Varsayılan Davranış:** Üzerine.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="cache-key-query-string"></a>Önbellek Anahtarı Sorgu Dizesi

**Amaç:** Önbellek anahtarının istekle ilişkili sorgu dize parametrelerini işeyleyip içermediğini veya dışlayıp kapsamadığını belirler.

Önemli bilgiler:

- Bir veya daha fazla sorgu dize parametresi adlarını belirtin ve her parametre adını tek bir boşlukla ayırın.
- Bu özellik, sorgu dize parametrelerinin önbellek anahtarının dahil edilip edilip edilemeyeceğini veya dışlanıp dışlanmayacağını belirler. Aşağıdaki tablodaki her seçenek için ek bilgi sağlanmaktadır.

Tür|Açıklama
--|--
 Şunları Dahil Et:|  Belirtilen her parametrenin önbellek anahtarına dahil edilmesi gerektiğini gösterir. Bu özellikte tanımlanan sorgu dize parametresi için benzersiz bir değer içeren her istek için benzersiz bir önbellek anahtarı oluşturulur.
 Tümleri Dahil Et  |Benzersiz bir sorgu dizesi içeren bir varlık için her istek için benzersiz bir önbellek anahtarı oluşturulduğunu gösterir. Bu tür bir yapılandırma genellikle önerilmez, çünkü önbellek isabetlerinin küçük bir yüzdesine neden olabilir. Daha fazla istek sunması gerektiğinden, az sayıda önbellek isabeti kaynağı sunucusundaki yükü artırır. Bu yapılandırma, Sorgu-String Önbelleğe Alma sayfasında "benzersiz önbellek" olarak bilinen önbelleğe alma davranışını yineler.
 Exclude | Yalnızca belirtilen parametrenin önbellek anahtarının dışında olduğunu gösterir. Diğer tüm sorgu dize parametreleri önbellek anahtarına dahil edilir.
 Tümleri Hariç Tut  |Tüm sorgu dize parametrelerinin önbellek anahtarının dışında olduğunu gösterir. Bu yapılandırma, Sorgu-String Önbelleğe Alma sayfasındaki "standart önbellek" varsayılan önbelleğe alma davranışını yineler.  

Kurallar altyapısı, sorgu dize önbelleğe alma nın uygulanma biçimini özelleştirmenize olanak tanır. Örneğin, sorgu dize önbelleğe yalnızca belirli konumlarda veya dosya türlerinde gerçekleştirildiğini belirtebilirsiniz.

Sorgu-String Önbelleğe Alma sayfasındaki "önbellek yok" sorgu dizesini yinelemek için, BIR URL Sorgusu Joker eşleme durumu ve Bypass Önbellek özelliği içeren bir kural oluşturun. URL Sorgu Joker Karakter koşulunu yıldız işareti (*) olarak ayarlayın.

>[!IMPORTANT]
> Bu hesaptaki herhangi bir yol için belirteç yetkilendirmesi etkinleştirilmişse, sorgu dizesi önbelleğe alınması için kullanılabilecek tek mod standart önbellek modudur. Daha fazla bilgi için bkz. [Sorgu dizeleri içeren Azure CDN önbelleğe alma davranışını kontrol etme](cdn-query-string-premium.md).

#### <a name="sample-scenarios"></a>Örnek Senaryolar

Bu özellik için aşağıdaki örnek kullanımı bir örnek isteği ve varsayılan önbellek anahtarı sağlar:

- **Örnek istek:** http://wpc.0001.&lt;Domain&gt;/800001/Origin/folder/asset.htm?sessionid=1234&dili=EN&userid=01
- **Varsayılan önbellek anahtarı:** /800001/Origin/folder/asset.htm

##### <a name="include"></a>Şunları Dahil Et:

Örnek yapılandırma:

- **Türü:** Içerir
- **Parametre(ler):** dil

Bu tür yapılandırma, aşağıdaki sorgu dize parametre önbelleği-anahtar oluşturur:

    /800001/Origin/folder/asset.htm?language=EN

##### <a name="include-all"></a>Tümleri Dahil Et

Örnek yapılandırma:

- **Türü:** Tümleri Dahil Et

Bu tür yapılandırma, aşağıdaki sorgu dize parametre önbelleği-anahtar oluşturur:

    /800001/Origin/folder/asset.htm?sessionid=1234&language=EN&userid=01

##### <a name="exclude"></a>Exclude

Örnek yapılandırma:

- **Türü:** Dışlamak
- **Parametre(ler):** sessioned userid

Bu tür yapılandırma, aşağıdaki sorgu dize parametre önbelleği-anahtar oluşturur:

    /800001/Origin/folder/asset.htm?language=EN

##### <a name="exclude-all"></a>Tümleri Hariç Tut

Örnek yapılandırma:

- **Türü:** Tümleri Hariç Tut

Bu tür yapılandırma, aşağıdaki sorgu dize parametre önbelleği-anahtar oluşturur:

    /800001/Origin/folder/asset.htm

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="cache-key-rewrite"></a>Önbellek Anahtarı Yeniden Yazma

**Amaç:** Bir istekle ilişkili önbellek anahtarını yeniden yazar.

Önbellek anahtarı, önbelleğe alma amacıyla bir varlığı tanımlayan göreli yoldur. Başka bir deyişle, sunucular önbellek anahtarıtarafından tanımlanan yoluna göre bir varlığın önbelleğe alınmış sürümünü denetler.

Aşağıdaki seçeneklerin her ikisini de tanımlayarak bu özelliği yapılandırın:

Seçenek|Açıklama
--|--
Orijinal Yol| Önbellek anahtarı yeniden yazılmış istek türlerine göreli yolu tanımlayın. Göreceli bir yol, temel başlangıç yolu seçilip sonra normal bir ifade deseni tanımlayarak tanımlanabilir.
Yeni Yol|Yeni önbellek anahtarı için göreli yolu tanımlayın. Göreceli bir yol, temel başlangıç yolu seçilip sonra normal bir ifade deseni tanımlayarak tanımlanabilir. Bu göreli [yol, HTTP değişkenlerinin](cdn-http-variables.md)kullanımı yla dinamik olarak oluşturulabilir.

**Varsayılan Davranış:** Bir isteğin önbellek anahtarı, uri isteği tarafından belirlenir.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="comment"></a>Açıklama

**Amaç:** Bir notun kural içinde eklenmesine izin verir.

Bu özelliğin kullanımından biri, bir kuralın genel amacı veya kurala neden belirli bir eşleşme koşulu nun veya özelliğin ekedildiği hakkında ek bilgi sağlamaktır.

Önemli bilgiler:

- En fazla 150 karakter belirtilebilir.
- Yalnızca alfasayısal karakterler kullanın.
- Bu özellik kuralın davranışını etkilemez. Yalnızca gelecekteki başvuru için bilgi sağlayabileceğiniz veya kuralı niçin sorun gidermede yardımcı olabilecek bir alan sağlamak içindir.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="complete-cache-fill"></a>Önbellek Dolgusu Tamamlandı

**Amaç:** Bir istek POP'ta kısmi önbellek kaçırmayla sonuçlandığında ne olacağını belirler.

Kısmi önbellek miss, POP'a tam olarak indirilmeyen bir varlığın önbellek durumunu açıklar. Bir varlık pop'ta yalnızca kısmen önbelleğe alınmışsa, bu varlık için bir sonraki istek yeniden başlangıç sunucusuna iletilir.
<!---
This feature is not available for the ADN platform. The typical traffic on this platform consists of relatively small assets. The size of the assets served through these platforms helps mitigate the effects of partial cache misses, since the next request will typically result in the asset being cached on that POP.

--->
Kısmi önbellek kalıbı genellikle kullanıcı indirmeyi iptal ettikten sonra veya yalnızca HTTP aralığı istekleri kullanılarak istenen varlıklar için oluşur. Bu özellik, genellikle baştan sona indirilmeyen büyük varlıklar (örneğin, videolar) için en kullanışlı özelliktir. Sonuç olarak, bu özellik varsayılan olarak HTTP Büyük platformunda etkinleştirilir. Diğer tüm platformlarda devre dışı bırakılır.

Müşteri kaynağı sunucunuzdaki yükü azalttığı ndan ve müşterilerinizin içeriğinizi indirme hızını artırdığından, HTTP Büyük platformu için varsayılan yapılandırmayı koruyun.

Değer|Sonuç
--|--
Etkin|Varsayılan davranışı geri yükler. Varsayılan davranış, POP'u kaynağı sunucudan varlığın arka plan getirmesini başlatmaya zorlamaktır. Bundan sonra, varlık POP'un yerel önbelleğinde olacaktır.
Devre dışı|POP'un varlık için arka plan getirmesini engeller. Sonuç olarak, bu bölgeden gelen bir sonraki varlık isteği, bir POP'un bu varlığı müşteri kaynağı sunucudan istemesine neden olur.

**Varsayılan Davranış:** Etkin.

#### <a name="compatibility"></a>Uyumluluk

Önbellek ayarlarının izlenme biçimi nedeniyle, bu özellik aşağıdaki eşleşme koşullarıyla ilişkilendirilemez:

- AS Numarası
- İstemci IP Adresi
- Çerez Parametresi
- Çerez Parametresi Regex
- Ülke
- Cihaz
- Microsoft Edge Cname
- Yönlendiren Etki Alanı
- İstek Üstbilgi Literal
- İstek Üstbilgi Regex
- İstek Üstbilgi Joker
- Request Yöntemi
- İstek Şeması
- URL Sorgusu Literal
- URL Sorgusu Regex
- URL Sorgusu Joker
- URL Sorgu Parametresi

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="compress-file-types"></a>Dosya Türlerini Sıkıştır

**Amaç:** Sunucuda sıkıştırılmış dosyaların dosya biçimlerini tanımlar.

Dosya biçimi, Internet ortam türü (örneğin, İçerik Türü) kullanılarak belirtilebilir. Internet ortam türü, sunucuların belirli bir varlığın dosya biçimini tanımlamasına olanak tanıyan platformdan bağımsız meta verilerdir. Sık kullanılan Internet ortam türlerinin listesi aşağıda verilmiştir.

Internet Medya Türü|Açıklama
--|--
metin/düz|Metin dosyalarını düze
text/html| HTML dosyaları
text/css|Geçişli Stil Sayfaları (CSS)
uygulama/x-javascript|Javascript
uygulama/javascript|Javascript

Önemli bilgiler:

- Her birini tek bir alanla sınırlandırarak birden çok Internet ortam türü belirtin.
- Bu özellik yalnızca boyutu 1 MB'dan küçük varlıkları sıkıştırır. Daha büyük varlıklar sunucular tarafından sıkıştırılmış değildir.
- Görüntüler, video ve ses medya varlıkları (örneğin, JPG, MP3, MP4, vb.) gibi belirli içerik türleri zaten sıkıştırılmış durumdadır. Bu tür varlıklarüzerinde ek sıkıştırma önemli ölçüde dosya boyutunu azaltmadığından, bu varlıklar üzerinde sıkıştırma etkinleştirmeniz önerilir.
- Yıldız işaretleri gibi joker karakterler desteklenmez.
- Bu özelliği bir kurala eklemeden önce, bu kuralın uygulandığı platform için Sıkıştırma Devreden seçeneğini Sıkıştırma sayfasında ayarladığınızdan emin olun.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="custom-log-field-1"></a>Özel Günlük Alanı 1

**Amaç:** Ham günlük dosyasında özel günlük alanına atanacak biçimi ve içeriği belirler.

Bu özel alan, günlük dosyalarınızda hangi istek ve yanıt üstbilgi değerlerinin depolanabilen leri belirlemenize olanak tanır.

Varsayılan olarak, özel günlük alanı "x-ec_custom-1" olarak adlandırılır. Bu alanın adı Raw Log Ayarları sayfasından özelleştirilebilir.

İstek ve yanıt üstbilgisini belirtme biçimi aşağıdaki gibi tanımlanır:

Üstbilgi Türü|Biçimlendir|Örnekler
-|-|-
İstek Başlığı|`%{[RequestHeader]()}[i]()` | %{Kabul-Kodlama}i <br/> {Referrer}i <br/> %{Yetkilendirme}i
Yanıt Üst Bilgisi|`%{[ResponseHeader]()}[o]()`| %{Yaş}o <br/> %{İçerik Türü}o <br/> %{Çerez}o

Önemli bilgiler:

- Özel bir günlük alanı üstbilgi alanları ve düz metin herhangi bir birleşimi ni içerebilir.
- Bu alan için geçerli karakterler şunlardır: alfasayısal (0-9, a-z ve A-Z), tireler, üst üste, yarı-iki nokta, kesitler, virgüller, dönemler, alt çizerler, eşit işaretler, parantezler ve boşluklar. Yüzde simgesi ve kıvırcık ayraçlara yalnızca üstbilgi alanı belirtmek için kullanıldığında izin verilir.
- Belirtilen her üstbilgi alanının yazımı istenen istek/yanıt üstbilgi adı ile eşleşmelidir.
- Birden çok üstbilgi belirtmek istiyorsanız, her üstbilgi belirtmek için bir ayırıcı kullanın. Örneğin, her üstbilgi için bir kısaltma kullanabilirsiniz:
    - AE: %{Accept-Encoding}i A: %{Authorization}i CT: %{İçerik-Türü}o

**Varsayılan Değer:** -

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---
### <a name="debug-cache-response-headers"></a>Hata Ayıklama Yanıt Üstbilgi

**Amaç:** Yanıtın, istenen varlığın önbellek ilkesi hakkında bilgi sağlayan [X-EC-Hata Ayıklama yanıt üstbilgilerini](cdn-http-debug-headers.md)içerip içeremeyeceğini belirler.

Hata ayıklama önbellek yanıt üstbilgisi, aşağıdakilerden ikisi de doğru olduğunda yanıta eklenecektir:

- Hata Ayıklama Önbellek Yanıt Üstbilgi özelliği belirtilen istekte etkinleştirildi.
- Belirtilen istek, yanıta dahil edilecek hata ayıklama önbelleğe alım sayıltıcı üstbilgi kümesini tanımlar.

Hata ayıklama önbellek yanıt üstbilgiaşağıdaki üstbilgi ve istek te belirtilen yönergeleri ekleyerek istenebilir:

`X-EC-Debug: _&lt;Directive1&gt;_,_&lt;Directive2&gt;_,_&lt;DirectiveN&gt;_`

**Örnek:**

X-EC-Hata Ayıklama: x-ec-önbellek, x-ec-çek-önbellek,x-ec-cache-key,x-ec-cache-state

Değer|Sonuç
-|-
Etkin|Hata ayıklama önbelleğe doğuracak yanıt üstbilgitalepleri, X-EC-Hata Ayıklama üstbilgisini içeren bir yanıt döndürür.
Devre dışı|X-EC-Hata Ayıklama yanıtı üstbilgisi yanıtın dışında tutulur.

**Varsayılan Davranış:** Devre dışı.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---
### <a name="default-internal-max-age"></a>Varsayılan Dahili Max-Age

**Amaç:** POP'un başlangıç sunucusu önbelleği yeniden validemi için varsayılan maksimum yaş aralığını belirler. Başka bir deyişle, POP'tan önce geçecek süre, önbelleğe alınmış bir varlığın kaynak sunucuda depolanan varlıkla eşleşip eşleşmediğini denetler.

Önemli bilgiler:

- Bu eylem yalnızca üstbilgide `Cache-Control` `Expires` bir maksimum yaş göstergesi atamayan bir başlangıç sunucusundan gelen yanıtlar için gerçekleşir.
- Bu eylem, önbelleğe alınmayan varlıklar için gerçekleşmez.
- Bu eylem, tarayıcıdan POP önbellek revalidations etkilemez. Bu tür yeniden doğrulamalar, `Cache-Control` Harici `Expires` Max-Age özelliği ile özelleştirilebilen tarayıcıya gönderilen üstbilgi veya üstbilgi tarafından belirlenir.
- Bu eylemin sonuçları yanıt üstbilgileri üzerinde gözlemlenebilir bir etkiye sahip değildir ve içeriğiniz için PS'lerden döndürülen içerik, ancak POP'lardan başlangıç sunucunuza gönderilen yeniden geçersiz değerlendirme trafiği nin miktarı üzerinde etkili olabilir.
- Bu özelliği şu şekilde yapılandırın:
    - Varsayılan dahili maksimum yaşın uygulanabileceği durum kodunu seçme.
    - Bir tümsek değeri belirtilmesi ve ardından istenen zaman biriminin seçilmesi (örneğin, saniye, dakika, saat, vb.). Bu değer varsayılan iç maksimum yaş aralığını tanımlar.

- Zaman biriminin "Kapalı" olarak ayarlanması, üstbilgilerinde `Cache-Control` veya `Expires` üstbilgisinde maksimum yaş göstergesi atanmamış istekler için 7 günlük varsayılan bir dahili maksimum yaş aralığı atar.

**Varsayılan Değer:** 7 gün

#### <a name="compatibility"></a>Uyumluluk

Önbellek ayarlarının izlenme biçimi nedeniyle, bu özellik aşağıdaki eşleşme koşullarıyla ilişkilendirilemez:
- AS Numarası
- İstemci IP Adresi
- Çerez Parametresi
- Çerez Parametresi Regex
- Ülke
- Cihaz
- Kenar Adı
- Yönlendiren Etki Alanı
- İstek Üstbilgi Literal
- İstek Üstbilgi Regex
- İstek Üstbilgi Joker
- Request Yöntemi
- İstek Şeması
- URL Sorgusu Literal
- URL Sorgusu Regex
- URL Sorgusu Joker
- URL Sorgu Parametresi

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="deny-access-403"></a>Erişimi Reddet (403)

**Amaç**: Tüm isteklerin 403 Yasak yanıtla reddedilip reddedilmediğini belirler.

Değer | Sonuç
------|-------
Etkin| Eşleşen ölçütleri karşılayan tüm isteklerin 403 Yasak yanıtla reddedilmesine neden olur.
Devre dışı| Varsayılan davranışı geri yükler. Varsayılan davranış, kaynak sunucusunun döndürülecek yanıt türünü belirlemesine izin vermektir.

**Varsayılan Davranış**: Devre Dışı

> [!TIP]
   > Bu özelliğin olası kullanımlarından biri, içeriğinize satır satır bağlantıları kullanan HTTP yönlendiricilerine erişimi engellemek için bir İstek Üstbilgi eşleşmesi koşuluyla ilişkilendirmektir.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="expires-header-treatment"></a>Sona Erer Başlık Tedavisi

**Amaç:** Harici Max-Age özelliği etkin olduğunda `Expires` üstbilgi üretimini POP ile denetler.

Bu tür bir yapılandırmayı elde etmenin en kolay yolu, Harici Max-Age ve Sona Eren Üstbilgi Tedavisi özelliklerini aynı ifadeye yerleştirmektir.

Değer|Sonuç
--|--
Üzerine yaz|Aşağıdaki eylemlerin gerçekleşmesini sağlar:<br/>- Kaynak `Expires` sunucusu tarafından oluşturulan üstbilginin üzerine yazar.<br/>- Yanıta `Expires` Harici Max-Age özelliği tarafından üretilen üstbilgi eklenir.
Geç|Harici Max-Age özelliği tarafından üretilen üstbilginin `Expires` yanıta hiçbir zaman eklenmemesini sağlar. <br/> Kaynak sunucu bir `Expires` üstbilgi üretirse, son kullanıcıya geçer. <br/>Kaynak sunucu bir `Expires` üstbilgi oluşturmuyorsa, bu seçenek yanıt üstbilginin `Expires` üstbilgi içermemesine neden olabilir.
Eksikse Ekle| Başlangıç `Expires` sunucusundan bir üstbilgi alınmadıysa, bu `Expires` seçenek Dış Max-Age özelliği tarafından üretilen üstbilgiyi ekler. Bu seçenek, tüm varlıkların bir `Expires` üstbilgi atanmasını sağlamak için yararlıdır.
Kaldır| Üstbilginin `Expires` üstbilgi yanıtına dahil edilmemesini sağlar. Bir `Expires` üstbilgi zaten atanmışsa, üstbilgi yanıtı kaldırılır.

**Varsayılan Davranış:** Üzerine

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="external-max-age"></a>Dış Max-Age

**Amaç:** Tarayıcının POP önbelleği yeniden validasyonuna kadar olan maksimum yaş aralığını belirler. Başka bir deyişle, bir tarayıcı nın bir POP'tan bir varlığın yeni bir sürümünü denetlemeden önce geçecek süre.

Bu özelliği etkinleştirmek, ÇEKP'lerden başlıklar oluşturur `Cache-Control: max-age` ve `Expires` bunları HTTP istemcisine gönderir. Varsayılan olarak, bu üstbilgi, kaynak sunucusu tarafından oluşturulan bu üstbilginin üzerine yazar. Ancak, Önbellek Denetimi Üstbilgisi Tedavisi ve Süresi Dolan Üstbilgi Tedavisi özellikleri bu davranışı değiştirmek için kullanılabilir.

Önemli bilgiler:

- Bu eylem POP'u kaynak sunucu önbelleği yeniden geçersiz kakmasını etkilemez. Bu tür yeniden validemler, başlangıç sunucusundan alınan üstbilgi `Cache-Control` ve `Expires` üstbilgi tarafından belirlenir ve Varsayılan Dahili Max-Age ve Force Internal Max-Age özellikleriyle özelleştirilebilir.
- Bir tamsayı değeri belirterek ve istenen zaman birimini (örneğin saniye, dakika, saat vb.) seçerek bu özelliği yapılandırın.
- Bu özelliği negatif bir değere ayarlamak, `Cache-Control: no-cache` ÇEKP'lerin tarayıcıya verilen her yanıtla geçmişte ayarlanmış bir `Expires` süre göndermesine neden olur. Bir HTTP istemcisi yanıtı önbelleğe almayacak olsa da, bu ayar, ANA VAROlan sunucudan gelen yanıtı önbelleğe almak için PS'lerin yeteneğini etkilemez.
- Zaman biriminin "Kapalı" olarak ayarlanması bu özelliği devre dışı bırakacaktır. `Cache-Control` Ve başlangıç sunucusunun yanıtı ile önbelleğe alan üstbilgi tarayıcıya `Expires` geçer.

**Varsayılan Davranış:** Kapalı

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="follow-redirects"></a>Yönlendirmeleri takip et

**Amaç:** İsteklerin müşteri kökenli bir sunucu tarafından döndürülen Konum üstbilgisinde tanımlanan ana bilgisayar ada mı yönlendirilemeyeceğini belirler.

Önemli bilgiler:

- İstekler yalnızca aynı platforma karşılık gelen kenar CNAME'lere yönlendirilebilir.

Değer|Sonuç
-|-
Etkin|İstekler yeniden yönlendirilebilir.
Devre dışı|İstekler yeniden yönlendirilmeyecektir.

**Varsayılan Davranış:** Devre dışı.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="force-internal-max-age"></a>Kuvvet İç Max-Age

**Amaç:** POP'un başlangıç sunucusu önbelleği yeniden validemi için maksimum yaş aralığını belirler. Başka bir deyişle, pop önbelleğe alınmış bir varlığın kaynak sunucuda depolanan varlıkla eşleşip eşleşmediğini kontrol etmeden önce geçecek süre.

Önemli bilgiler:

- Bu özellik, bir başlangıç sunucusundan `Cache-Control` oluşturulan `Expires` veya üstbilgide tanımlanan maksimum yaş aralığını geçersiz kılar.
- Bu özellik, tarayıcıdan POP önbellek revalidations etkilemez. Bu tür yeniden doğrulamalar tarayıcıya gönderilen üstbilgi `Cache-Control` veya `Expires` üstbilgi tarafından belirlenir.
- Bu özelliğin, BIR POP tarafından istekte bulunulan yanıt üzerinde gözlemlenebilir bir etkisi yoktur. Ancak, POP'lardan başlangıç sunucusuna gönderilen yeniden doğrulama trafiği nin miktarı üzerinde bir etkisi olabilir.
- Bu özelliği şu şekilde yapılandırın:
    - Dahili bir maksimum yaşın uygulanacağı durum kodunu seçme.
    - Bir tümsek değeri belirtme ve istenen zaman birimini (örneğin, saniye, dakika, saat, vb.) seçmek. Bu değer, isteğin maksimum yaş aralığını tanımlar.

- Zaman birimini "Kapalı" olarak ayarlamak bu özelliği devre dışı kılabilir. İstenen varlıklara dahili bir maksimum yaş aralığı atanmayacaktır. Orijinal üstbilgi önbelleğe alma yönergeleri içermiyorsa, kıymet Varsayılan İç Max-Age özelliğindeki etkin ayara göre önbelleğe alınacaktır.

**Varsayılan Davranış:** Kapalı

#### <a name="compatibility"></a>Uyumluluk

Önbellek ayarlarının izlenme biçimi nedeniyle, bu özellik aşağıdaki eşleşme koşullarıyla ilişkilendirilemez:
- AS Numarası
- İstemci IP Adresi
- Çerez Parametresi
- Çerez Parametresi Regex
- Ülke
- Cihaz
- Kenar Adı
- Yönlendiren Etki Alanı
- İstek Üstbilgi Literal
- İstek Üstbilgi Regex
- İstek Üstbilgi Joker
- Request Yöntemi
- İstek Şeması
- URL Sorgusu Literal
- URL Sorgusu Regex
- URL Sorgusu Joker
- URL Sorgu Parametresi

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="h264-support-http-progressive-download"></a>H.264 Desteği (HTTP Aşamalı İndir)

**Amaç:** İçerik akışı için kullanılabilecek H.264 dosya biçimlerinin türlerini belirler.

Önemli bilgiler:

- Dosya Uzantıları seçeneğinde, izin verilen H.264 dosya adı uzantılarıkümesini sınırlandırılmış bir alan kümesi tanımlayın. Dosya Uzantıları seçeneği varsayılan davranışı geçersiz kılar. Bu seçeneği ayarlarken bu dosya adı uzantılarını ekleyerek MP4 ve F4V desteğini koruyun.
- Her dosya adı uzantısı (örneğin, _.mp4_, _.f4v)_ belirttiğiniz bir dönem ekleyin.

**Varsayılan Davranış:** HTTP Progressive Download varsayılan olarak MP4 ve F4V ortamını destekler.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="honor-no-cache-request"></a>Onur No-Önbellek İsteği

**Amaç:** HTTP istemcisinin önbellek yok isteklerinin kaynak sunucuya iletilip iletilmeyeceğini belirler.

ÖNbellek yok isteği, HTTP istemcisi `Cache-Control: no-cache` HTTP isteğine bir ve/veya `Pragma: no-cache` üstbilgi gönderdiğinde oluşur.

Değer|Sonuç
--|--
Etkin|BIR HTTP istemcisinin önbellek yok isteklerinin başlangıç sunucusuna iletilmesine izin verir ve başlangıç sunucusu yanıt üstbilgilerini ve gövdeyi POP aracılığıyla HTTP istemcisine geri döndürer.
Devre dışı|Varsayılan davranışı geri yükler. Varsayılan davranış, önbellek yok isteklerinin kaynak sunucuya iletilmesini önlemektir.

Tüm üretim trafiği için, bu özelliği varsayılan devre dışı bırakmaönerilir. Aksi takdirde, başlangıç sunucuları, web sayfalarını yenilerken yanlışlıkla birçok önbellek yok isteğini tetikleyebilecek son kullanıcılardan veya her video isteğiyle önbellek yok üstbilgi göndermek üzere kodlanan birçok popüler medya oynatıcıdan korunamaz. Bununla birlikte, bu özellik, yeni içeriğin kaynak sunucudan isteğe bağlı olarak çekilmesine izin vermek için belirli üretim dışı evreleme veya test dizinlerine başvurmak için yararlı olabilir.

Bu özellik nedeniyle bir başlangıç sunucusuna iletilebilir bir istek için bildirilen `TCP_Client_Refresh_Miss`önbellek durumu. Çekirdek raporlama modülünde bulunan Önbellek Durumları raporu, önbellek durumuna göre istatistiksel bilgiler sağlar. Bu rapor, bu özellik nedeniyle bir başlangıç sunucusuna iletilen isteklerin sayısını ve yüzdesini izlemenize olanak tanır.

**Varsayılan Davranış:** Devre dışı.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="ignore-origin-no-cache"></a>Kaynak Yokyokla No-Önbellek

**Amaç:** CDN'nin bir kaynak sunucusundan sunulan aşağıdaki yönergeleri yoksayıp yoksaymayacağını belirler:

- `Cache-Control: private`
- `Cache-Control: no-store`
- `Cache-Control: no-cache`
- `Pragma: no-cache`

Önemli bilgiler:

- Yukarıdaki yönergelerin yoksayılacak olacağı durum kodlarının alanı sınırlı bir listesini tanımlayarak bu özelliği yapılandırın.
- Bu özellik için geçerli durum kodları kümesi şunlardır: 200, 203, 300, 301, 302, 305, 307, 400, 401, 402, 403, 404, 405, 406, 407, 408, 409, 410, 411, 412, 413, 414, 415, 416, 417, 500, 501, 502, 503, 504 ve 505.
- Bu özelliği boş bir değere ayarlayarak devre dışı düşürün.

**Varsayılan Davranış:** Varsayılan davranış, yukarıdaki yönergeleri onurlandırmaktır.

#### <a name="compatibility"></a>Uyumluluk

Önbellek ayarlarının izlenme biçimi nedeniyle, bu özellik aşağıdaki eşleşme koşullarıyla ilişkilendirilemez:
- AS Numarası
- İstemci IP Adresi
- Çerez Parametresi
- Çerez Parametresi Regex
- Ülke
- Cihaz
- Kenar Adı
- Yönlendiren Etki Alanı
- İstek Üstbilgi Literal
- İstek Üstbilgi Regex
- İstek Üstbilgi Joker
- Request Yöntemi
- İstek Şeması
- URL Sorgusu Literal
- URL Sorgusu Regex
- URL Sorgusu Joker
- URL Sorgu Parametresi

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="ignore-unsatisfiable-ranges"></a>Satatisfiable Aralıkları Yoksay

**Amaç:** Bir istek 416 İstenen Aralık Değil Tatmin Edilebilir durum kodu oluşturduğunda istemcilere döndürülecek yanıtı belirler.

Varsayılan olarak, belirtilen bayt aralığı isteği POP tarafından karşılanamadığında ve If-Range istek üstbilgi alanı belirtilmediğinde bu durum kodu döndürülür.

Değer|Sonuç
-|-
Etkin|416 İstenen Aralık Değil Tatmin Edilebilir durum koduyla geçersiz bir bayt aralığı isteğine yanıt veren SAHTE LERin inmesini önler. Bunun yerine sunucular istenen varlığı teslim edecek ve istemciye 200 Ok döndürecek.
Devre dışı|Varsayılan davranışı geri yükler. Varsayılan davranış, 416 İstenen Aralık Değil Tatmin Edilebilir durum kodunu onurlandırmaktır.

**Varsayılan Davranış:** Devre dışı.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="internal-max-stale"></a>Dahili Max-Bayat

**Amaç:** POP önbelleğe alınan varlığı niçin kaynağı sunucuyla yeniden doğrulayamadığında, önbelleğe alınmış bir varlığın bir POP'tan ne kadar süreyle hizmet verebileceğini denetler.

Normalde, bir varlığın maksimum yaş süresi sona erdiğinde, POP başlangıç sunucusuna bir yeniden doğrulama isteği gönderir. Kaynak sunucu daha sonra, POP'a önbelleğe alınmış kıymetüzerinde yeni bir kira sağlamak için 304 Değiştirilmemiş veya POP'a önbelleğe alınmış varlığın güncelleştirilmiş bir sürümünü sağlamak için 200 OK ile yanıt verecektir.

POP böyle bir yeniden validasyon çalışırken başlangıç sunucusuyla bağlantı kuramıyorsa, bu Internal Max-Bayat özelliği pop'un artık eskimiş varlığa hizmet etmeye devam edip edemeyeceğini ve ne kadar süreyle hizmet verebileceğini denetler.

Bu zaman aralığının, başarısız yeniden validasyon gerçekleştiğinde değil, varlığın maksimum yaşının dolduğunda başladığını unutmayın. Bu nedenle, bir varlığın başarılı bir yeniden validasyon olmadan sunulabileceği maksimum süre, maksimum yaş artı maksimum bayat kombinasyonutarafından belirtilen süredir. Örneğin, bir kıymet en fazla 30 dakikalık ve en yüksek 15 dakikalık bir tarihle 9:00'da önbelleğe alınmışsa, 9:44'te başarısız bir yeniden doğrulama denemesi son kullanıcının eski önbelleğe alınmış kıymeti almasına neden olurken, 9:46'da başarısız bir yeniden doğrulama denemesi son kullanıcının 504 Ağ Geçidi Zaman Ayarı almasına neden olur.

Bu özellik için yapılandırılan herhangi bir `Cache-Control: must-revalidate` değer, kaynak sunucusundan alınan üstbilgi veya `Cache-Control: proxy-revalidate` üstbilgi tarafından geçersiz yapılır. Bu üstallardan biri bir varlık başlangıçta önbelleğe alındığı zaman başlangıç sunucusundan alınırsa, POP eski önbelleğe alınmış bir varlığa hizmet etmez. Böyle bir durumda, varlığın maksimum yaş aralığı sona erdiğinde POP kaynağıyla yeniden doğrulayamıyorsa, POP 504 Ağ Geçidi Zaman Aşımı hatası döndürür.

Önemli bilgiler:

- Bu özelliği şu şekilde yapılandırın:
    - Bir max-bayat uygulanacak durum kodu seçimi.
    - Bir tümsek değeri belirtilmesi ve ardından istenen zaman biriminin seçilmesi (örneğin, saniye, dakika, saat, vb.). Bu değer uygulanacak dahili max-bayat tanımlar.

- Zaman biriminin "Kapalı" olarak ayarlanması bu özelliği devre dışı bırakacaktır. Önbelleğe alınmış bir varlık normal son kullanma süresinin ötesinde hizmet verilmeyecektir.

**Varsayılan Davranış:** İki dakika

#### <a name="compatibility"></a>Uyumluluk

Önbellek ayarlarının izlenme biçimi nedeniyle, bu özellik aşağıdaki eşleşme koşullarıyla ilişkilendirilemez:
- AS Numarası
- İstemci IP Adresi
- Çerez Parametresi
- Çerez Parametresi Regex
- Ülke
- Cihaz
- Kenar Adı
- Yönlendiren Etki Alanı
- İstek Üstbilgi Literal
- İstek Üstbilgi Regex
- İstek Üstbilgi Joker
- Request Yöntemi
- İstek Şeması
- URL Sorgusu Literal
- URL Sorgusu Regex
- URL Sorgusu Joker
- URL Sorgu Parametresi

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="log-query-string"></a>Günlük Sorgu Dizesi

**Amaç:** Erişim günlüklerinde URL ile birlikte bir sorgu dizesi depolanıp depolanmayacağını belirler.

Değer|Sonuç
-|-
Etkin|URL'leri bir erişim günlüğüne kaydederken sorgu dizelerinin depolanmasını sağlar. Bir URL sorgu dizesi içermiyorsa, bu seçeneğin bir etkisi olmaz.
Devre dışı|Varsayılan davranışı geri yükler. Varsayılan davranış, url'leri bir erişim günlüğüne kaydederken sorgu dizelerini yok saymaktır.

**Varsayılan Davranış:** Devre dışı.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="maximum-keep-alive-requests"></a>Maksimum Tutma-Canlı İstekleri

**Amaç:** Kapanmadan önce Keep-Alive bağlantısı için en fazla istek sayısını tanımlar.

En fazla istek sayısının düşük bir değere ayarlanması önerilmez ve performans düşüşüne neden olabilir.

Önemli bilgiler:

- Bu değeri bir bütün tamsayı olarak belirtin.
- Belirtilen değere virgül veya dönemler eklemeyin.

**Varsayılan Değer:** 10.000 istek

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="modify-client-request-header"></a>İstemci İstek Üstbilgisini Değiştir

**Amaç:** Her istek, onu açıklayan bir dizi istek üstbilgiiçerir. Bu özellik aşağıdakileri yapabilir:

- İstek üstbilgisine atanan değeri ekle veya üzerine yazın. Belirtilen istek üstbilgisi yoksa, bu özellik isteğe ekler.
- İstekten bir istek üstbilgisini silin.

Bir kaynak sunucusuna iletilen istekler, bu özellik tarafından yapılan değişiklikleri yansıtır.

Aşağıdaki eylemlerden biri bir istek üstbilgiüzerinde gerçekleştirilebilir:

Seçenek|Açıklama|Örnek
-|-|-
Ekle|Belirtilen değer, varolan istek üstbilgi değerinin sonuna eklenir.|**İstek üstbilgi değeri (istemci):**<br/>Değer1<br/>**İstek üstbilgi değeri (kural motoru):**<br/>Değer2 <br/>**Yeni istek üstbilgi değeri:** <br/>Değer1Değer2
Üzerine yaz|İstek üstbilgi değeri belirtilen değere ayarlanır.|**İstek üstbilgi değeri (istemci):**<br/>Değer1<br/>**İstek üstbilgi değeri (kural motoru):**<br/>Değer2<br/>**Yeni istek üstbilgi değeri:**<br/> Değer2 <br/>
Sil|Belirtilen istek üstbilgisini siler.|**İstek üstbilgi değeri (istemci):**<br/>Değer1<br/>**İstemci istek üstbilgi yapılandırması değiştirin:**<br/>Söz konusu istek üstbilgisini silin.<br/>**Sonuç:**<br/>Belirtilen istek üstbilgisi kaynak sunucusuna iletilmeyecek.

Önemli bilgiler:

- Ad seçeneğinde belirtilen değerin istenen istek üstbilgisi için tam olarak eşleşdiğinden emin olun.
- Servis talebi, bir üstbilginin tanımlanması amacıyla dikkate alınmaz. Örneğin, `Cache-Control` üstbilgi adının aşağıdaki varyasyonlarından herhangi biri tanımlamak için kullanılabilir:
    - önbellek denetimi
    - ÖNBELLEK DENETIMI
    - cachE-Kontrol
- Üstbilgi adı belirtirken yalnızca alfasayısal karakterler, tireler veya alt çizgi ler kullanın.
- Üstbilginin silmesi, üstbilginin BIR başlangıç sunucusuna, POP'lar tarafından iletilmesine engel olur.
- Aşağıdaki üstbilgi ayrılmıştır ve bu özellik tarafından değiştirilemez:
    - Iletilen
    - konak
    - Via
    - warning
    - x-forwarded-for
    - "x-ec" ile başlayan tüm üstbilgi adları ayrılmıştır.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="modify-client-response-header"></a>İstemci Yanıt Üstbilgisini Değiştir

Her yanıt, onu açıklayan bir yanıt üstbilgi kümesi içerir. Bu özellik aşağıdakileri yapabilir:

- Yanıt üstbilgisine atanan değeri ekle veya üzerine yazın. Belirtilen yanıt üstbilgisi yoksa, bu özellik yanıta ekler.
- Yanıt üstbilgisini yanıttan silin.

Varsayılan olarak, yanıt üstbilgi değerleri bir başlangıç sunucusu ve POP'lar tarafından tanımlanır.

Aşağıdaki eylemlerden biri yanıt üstbilginde gerçekleştirilebilir:

Seçenek|Açıklama|Örnek
-|-|-
Ekle|Belirtilen değer, varolan yanıt üstbilgi değerinin sonuna eklenir.|**Yanıt üstbilgi değeri (istemci):**<br />Değer1<br/>**Yanıt üstbilgi değeri (kurallar motoru):**<br/>Değer2<br/>**Yeni yanıt üstbilgi değeri:**<br/>Değer1Değer2
Üzerine yaz|Yanıt üstbilgi değeri belirtilen değere ayarlanır.|**Yanıt üstbilgi değeri (istemci):**<br/>Değer1<br/>**Yanıt üstbilgi değeri (kurallar motoru):**<br/>Değer2 <br/>**Yeni yanıt üstbilgi değeri:**<br/>Değer2 <br/>
Sil|Belirtilen yanıt üstbilgisini siler.|**Yanıt üstbilgi değeri (istemci):**<br/>Değer1<br/>**İstemci yanıt üstbilgi yapılandırması değiştirin:**<br/>Söz konusu yanıt üstbilgisini silin.<br/>**Sonuç:**<br/>Belirtilen yanıt üstbilgisi istekte bulundurucuya iletilmeyecektir.

Önemli bilgiler:

- Ad seçeneğinde belirtilen değerin istenen yanıt üstbilgisi için tam bir eşleşme olduğundan emin olun.
- Servis talebi, bir üstbilginin tanımlanması amacıyla dikkate alınmaz. Örneğin, `Cache-Control` üstbilgi adının aşağıdaki varyasyonlarından herhangi biri tanımlamak için kullanılabilir:
    - önbellek denetimi
    - ÖNBELLEK DENETIMI
    - cachE-Kontrol
- Üstbilginin silmesi, üstbilginin istekte bulunulmasını engeller.
- Aşağıdaki üstbilgi ayrılmıştır ve bu özellik tarafından değiştirilemez:
    - kabul-kodlama
    - yaş
    - bağlantı
    - içerik kodlama
    - içerik uzunluğu
    - içerik aralığı
    - date
    - sunucu
    - Römork
    - aktarım-kodlama
    - yükseltme
    - Değiş
    - Via
    - warning
    - "x-ec" ile başlayan tüm üstbilgi adları ayrılmıştır.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="partial-cache-sharing"></a>Kısmi Önbellek Paylaşımı

**Amaç:** Bir isteğin kısmen önbelleğe alınmış içerik oluşturup oluşturamayacağını belirler.

Bu kısmi önbellek, istenen içerik tamamen önbelleğe alınıncaya kadar bu içerik için yeni istekleri yerine getirmek için kullanılabilir.

Değer|Sonuç
-|-
Etkin|İstekler kısmen önbelleğe alınmış içerik oluşturabilir.
Devre dışı|İstekler yalnızca istenen içeriğin tam olarak önbelleğe alınmış bir sürümünü oluşturabilir.

**Varsayılan Davranış:** Devre dışı.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="prevalidate-cached-content"></a>Önbelleğe Alınmış İçeriği Önceden Yoklayın

**Amaç:** Önbelleğe alınmış içeriğin TTL süresi dolmadan önce erken yeniden validasyon için uygun olup olmayacağını belirler.

İstenen içeriğin TTL'sinin sona ermesinden önceki süreyi tanımlayın ve bu süre boyunca erken yeniden validasyona hak kazanacaktır.

Önemli bilgiler:

- Önbelleğe alınmış içeriğin TTL'si sona erdikten sonra yeniden doğrulamayı gerektiren zaman birimi olarak "Kapalı" seçeneğini seçmek. Zaman belirtilmemelidir ve yoksayılır.

**Varsayılan Davranış:** Tamam, tamam. Yeniden doğrulama yalnızca önbelleğe alınmış içeriğin TTL'sinin süresi dolduktan sonra gerçekleşebilir.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="proxy-special-headers"></a>Proxy Özel Başlıklar

**Amaç:** Pop'tan bir başlangıç sunucusuna iletilecek [Verizon'a özgü HTTP istek üstbilgilerini](cdn-verizon-http-headers.md) tanımlar.

Önemli bilgiler:

- Bu özellikte tanımlanan HER CDN'ye özgü istek üstbilgisi bir başlangıç sunucusuna iletilir. Dışlanan üstbilgi iletilmez.
- CDN'ye özgü bir istek üstbilgisinin iletilmesini önlemek için, üstbilgi listesi alanındaki boşluk ayrılmış listeden kaldırın.

Aşağıdaki HTTP üstbilgileri varsayılan listeye dahildir:
- Via
- X-Forwarded-For
- X-İlli-Proto
- X-Host
- X-Midgress
- X-Ağ Geçidi Listesi
- X-EC Adı
- Host

**Varsayılan Davranış:** CDN'ye özgü tüm istek üstbilgisi başlangıç sunucusuna iletilir.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="refresh-zero-byte-cache-files"></a>Sıfır Bayt Önbellek Dosyalarını Yenile

**Amaç:** BIR HTTP istemcisinin 0 baytönbellek varlığı için isteğinin POP'lar tarafından nasıl işleneceğini belirler.

Geçerli değerler:

Değer|Sonuç
--|--
Etkin|POP'un varlığı kaynak sunucudan yeniden getirmesine neden olur.
Devre dışı|Varsayılan davranışı geri yükler. Varsayılan davranış, istek üzerine geçerli önbellek varlıklarını sunmaktır.

Bu özellik doğru önbelleğe alma ve içerik teslimi için gerekli değildir, ancak geçici çözüm olarak yararlı olabilir. Örneğin, kaynak sunucularında bulunan dinamik içerik üreteçleri yanlışlıkla 0 baytyanıtlarının POP'lara gönderilmesine neden olabilir. Bu tür yanıtlar genellikle POP'lar tarafından önbelleğe alınır. 0 bayt yanıtının bu tür içerikler için hiçbir zaman geçerli bir yanıt olmadığını biliyorsanız, bu özellik bu tür varlıkların müşterilerinize sunulmasını engelleyebilir.

**Varsayılan Davranış:** Devre dışı.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="set-cacheable-status-codes"></a>Önbelleğe Alan Durum Kodlarını Ayarlama

**Amaç:** Önbelleğe alınmış içeriğe neden olabilecek durum kodları kümesini tanımlar.

Varsayılan olarak, önbelleğe alma yalnızca 200 Ok yanıtı için etkinleştirilir.

İstenilen durum kodlarının boşluk sınırlandırım kümesini tanımlayın.

Önemli bilgiler:

- Önbellek Yoksay özelliğini etkinleştirin. Bu özellik etkinleştirilmezse, 200 Ok olmayan yanıtönbelleğe alınmayabilir.
- Bu özellik için geçerli durum kodları kümesi şunlardır: 203, 300, 301, 302, 305, 307, 400, 401, 402, 403, 404, 405, 406, 407, 408, 409, 410, 411, 412, 413, 414, 415, 416, 417, 500, 501, 502, 503, 504 ve 505.
- Bu özellik, 200 Tamam durum kodu oluşturan yanıtlar için önbelleğe alma devre dışı kalamaz.

**Varsayılan Davranış:** Önbelleğe alma yalnızca 200 OK durum kodu oluşturan yanıtlar için etkinleştirilir.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="set-client-ip-custom-header"></a>İstemci IP Özel Üstbilgi Ayarla

**Amaç:** İstek için IP adresine göre isteyen istemciyi tanımlayan özel bir üstbilgi ekler.

Üstbilgi adı seçeneği, istemcinin IP adresinin depolandığı özel istek üstbilgisinin adını tanımlar.

Bu özellik, müşteri kökenli bir sunucunun istemci IP adreslerini özel bir istek üstbilgisi aracılığıyla bulmasına olanak tanır. İstek önbellekten sunulursa, başlangıç sunucusu istemcinin IP adresi hakkında bilgilendirilmez. Bu nedenle, bu özelliğin önbelleğe alınamayan varlıklarla kullanılması önerilir.

Belirtilen üstbilgi adının aşağıdaki adlardan herhangi biri ile eşleşmediğinden emin olun:

- Standart istek üstbilgi adları. Standart üstbilgi adlarının listesi [RFC 2616'da](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)bulunabilir.
- Ayrılmış üstbilgi adları:
    - iletilen-for
    - konak
    - Değiş
    - Via
    - warning
    - x-forwarded-for
    - "x-ec" ile başlayan tüm üstbilgi adları ayrılmıştır.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="stale-content-delivery-on-error"></a>Hata üzerine Bayat İçerik Teslimi

**Amaç:** Önbellek yeniden validemi sırasında veya müşteri kaynağı sunucusundan istenen içeriği alırken bir hata oluştuğunda süresi dolan önbelleğe alınmış içeriğin teslim edilip edilmeyeceğini belirler.

Değer|Sonuç
-|-
Etkin|Bir başlangıç sunucusuna bağlantı sırasında bir hata oluştuğunda, eski içerik istekte bulunana sunulur.
Devre dışı|Kaynak sunucunun hatası istekte bulunana iletilir.

**Varsayılan Davranış:** Devre dışı

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="stale-while-revalidate"></a>Bayat Yeniden Geçersiz Kılın

**Amaç:** Yeniden validasyon gerçekleşirken, SAHTE'lerin istekte bulunarak eski içeriği talep edene sunmasını sağlayarak performansı artırır.

Önemli bilgiler:

- Bu özelliğin davranışı seçilen zaman birimine göre değişir.
    - **Zaman Birimi:** Bir süre belirtin ve eski içerik teslimine izin vermek için bir zaman birimi (örneğin, Saniye, Dakika, Saat, vb.) seçin. Bu tür bir kurulum, CDN'nin aşağıdaki formüle göre doğrulama gerektirmeden önce içeriği sunabileceği süreyi uzatmasına olanak tanır: **TTL** + **Bayat Süre Yi**
    - **Kapalı:** Eski içerik isteği sunulmadan önce yeniden validasyon gerektirecek "Kapalı"yı seçin.
        - Uygulanamaz olduğundan ve yoksayılacağına göre bir süre belirtmeyin.

**Varsayılan Davranış:** Tamam, tamam. İstenen içeriğin sunulması için yeniden doğrulama yapılmalıdır.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="token-auth"></a>Token Auth

**Amaç:** Bir istekiçin Belirteç Tabanlı Kimlik Doğrulama'nın uygulanıp uygulanmayacağını belirler.

Belirteç Tabanlı Kimlik Doğrulama etkinleştirilirse, yalnızca şifreli bir belirteç sağlayan ve bu belirteç tarafından belirtilen gereksinimlere uygun istekleri yerine getirilecektir.

Belirteç değerlerini şifrelemek ve çözmek için kullanılan şifreleme anahtarı, Token Auth sayfasındaki Birincil Anahtar ve Yedek Anahtar seçenekleri tarafından belirlenir. Şifreleme anahtarlarının platforma özel olduğunu unutmayın.

**Varsayılan Davranış:** Devre dışı.

Bu özellik, URL Yeniden Yazma özelliği dışında çoğu özellikten daha önceliklidir.

Değer | Sonuç
------|---------
Etkin | İstenilen içeriği Token Tabanlı Kimlik Doğrulama ile korur. Yalnızca geçerli bir belirteç sağlayan ve gereksinimlerini karşılayan istemcilerden gelen istekler yerine getirilecektir. FTP hareketleri Token Tabanlı Kimlik Doğrulama'nın dışındadır.
Devre dışı| Varsayılan davranışı geri yükler. Varsayılan davranış, bir isteğin güvenli olup olmadığını belirlemek için Belirteç Tabanlı Kimlik Doğrulama yapılandırmanıza izin vermektir.

#### <a name="compatibility"></a>Uyumluluk

Token Auth'u Her Zaman maç koşuluyla kullanmayın.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="token-auth-denial-code"></a>Token Auth İnkar Kodu

**Amaç:** Belirteç tabanlı kimlik doğrulaması nedeniyle istek reddedildiğinde kullanıcıya döndürülecek yanıt türünü belirler.

Kullanılabilir yanıt kodları aşağıdaki tabloda listelenmiştir.

Yanıt Kodu|Yanıt Adı|Açıklama
-------------|-------------|--------
301|Kalıcı Olarak Taşındı|Bu durum kodu, yetkisiz kullanıcıları Konum üstbilgisinde belirtilen URL'ye yönlendirir.
302|Bulundu|Bu durum kodu, yetkisiz kullanıcıları Konum üstbilgisinde belirtilen URL'ye yönlendirir. Bu durum kodu, yeniden yönlendirme gerçekleştirmenin endüstri standardı yöntemidir.
307|Geçici Yönlendirme|Bu durum kodu, yetkisiz kullanıcıları Konum üstbilgisinde belirtilen URL'ye yönlendirir.
401|Yetkisiz|Bu durum kodunu WWW-Authenticate yanıt üstbilgisi ile birleştirmek, bir kullanıcıdan kimlik doğrulaması için istekte etmenizi sağlar.
403|Yasak|Bu ileti, yetkisiz bir kullanıcının korumalı içeriğe erişmeye çalışırken göreceği standart 403 Yasak durum iletisidir.
404|Dosya Bulunamadı|Bu durum kodu, HTTP istemcisinin sunucuyla iletişim kurabildiğini, ancak istenen içeriğin bulunamadığını gösterir.

#### <a name="compatibility"></a>Uyumluluk

Token Auth Denial Code'u Her Zaman eşleşen bir koşulla kullanmayın. Bunun yerine, **Yönet** portalının **Token Auth** sayfasındaki **Özel Reddi İşleme** bölümünü kullanın. Daha fazla bilgi için bkz: [Azure CDN varlıklarını belirteç kimlik doğrulamasıyla güvence altına](cdn-token-auth.md)alma.

#### <a name="url-redirection"></a>URL Yeniden Yönlendirme

Bu özellik, 3xx durum kodu döndürecek şekilde yapılandırıldığında kullanıcı tanımlı bir URL'ye URL yönlendirmesini destekler. Bu kullanıcı tanımlı URL aşağıdaki adımları gerçekleştirerek belirtilebilir:

1. Token Auth Reddi Kodu özelliği için bir 3xx yanıt kodu seçin.
2. İsteğe Bağlı Üstbilgi Adı seçeneğinden "Konum" seçeneğini belirleyin.
3. İsteğe Bağlı Üstbilgi Değeri seçeneğini istenen URL'ye ayarlayın.

Bir URL 3xx durum kodu için tanımlanmamışsa, 3xx durum kodu için standart yanıt sayfası kullanıcıya döndürülür.

URL yeniden yönlendirmesi yalnızca 3xx yanıt kodları için geçerlidir.

İsteğe Bağlı Üstbilgi Değeri seçeneği alfasayısal karakterleri, tırnak işaretlerini ve boşlukları destekler.

#### <a name="authentication"></a>Kimlik Doğrulaması

Bu özellik, Belirteç Tabanlı Kimlik Doğrulama tarafından korunan içerik için yetkisiz bir taleme yanıt verirken WWW-Authenticate üstbilgisini ekleme özelliğini destekler. WWW-Authenticate üstbilgisi yapılandırmanızda "temel" olarak ayarlanmışsa, yetkisiz kullanıcıdan hesap kimlik bilgileri istenir.

Yukarıdaki yapılandırma aşağıdaki adımları gerçekleştirerek elde edilebilir:

1. Token Auth Reddi Kodu özelliğinin yanıt kodu olarak "401"i seçin.
2. İsteğe Bağlı Üstbilgi Adı seçeneğinden "WWW-Authenticate" seçeneğini belirleyin.
3. İsteğe Bağlı Üstbilgi Değeri seçeneğini "temel" olarak ayarlayın.

WWW-Authenticate üstbilgisi yalnızca 401 yanıt kodları için geçerlidir.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="token-auth-ignore-url-case"></a>Token Auth Göz ardı URL Örneği

**Amaç:** Token Tabanlı Kimlik Doğrulama tarafından yapılan URL karşılaştırmalarının büyük/küçük harf duyarlı olup olmadığını belirler.

Bu özellikten etkilenen parametreler şunlardır:

- ec_url_allow
- ec_ref_allow
- ec_ref_deny

Geçerli değerler:

Değer|Sonuç
---|----
Etkin|Token Tabanlı Kimlik Doğrulama parametreleri için URL'leri karşılaştırırken POP'un büyük/küçük harf yok sayılmasına neden olur.
Devre dışı|Varsayılan davranışı geri yükler. Varsayılan davranış, Token Kimlik Doğrulaması'nın büyük/küçük harf duyarlı olması için URL karşılaştırmaları içindir.

**Varsayılan Davranış:** Devre dışı.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="token-auth-parameter"></a>Token Auth Parametresi

**Amaç:** Belirteç Tabanlı Kimlik Doğrulama sorgu dize parametresinin yeniden adlandırılıp adlandırılmayacağını belirler.

Önemli bilgiler:

- Değer seçeneği, bir belirteç belirtilebileceği sorgu dize parametre adını tanımlar.
- Değer seçeneği "ec_token" olarak ayarlanamaz.
- Değer seçeneğinde tanımlanan adın yalnızca geçerli URL karakterleri içerdiğinden emin olun.

Değer|Sonuç
----|----
Etkin|Değer seçeneği, belirteçlerin tanımlanması gereken sorgu dize parametre adını tanımlar.
Devre dışı|Belirteç, istek URL'sinde tanımlanmamış bir sorgu dize parametresi olarak belirtilebilir.

**Varsayılan Davranış:** Devre dışı. Belirteç, istek URL'sinde tanımlanmamış bir sorgu dize parametresi olarak belirtilebilir.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="url-redirect"></a>URL Yönlendirme

**Amaç:** İstekleri Konum üstbilgisi üzerinden yönlendirir.

Bu özelliğin yapılandırması, aşağıdaki seçenekleri ayarlamayı gerektirir:

Seçenek|Açıklama
-|-
Kod|İstekçiye döndürülecek yanıt kodunu seçin.
Kaynak & Deseni| Bu ayarlar, yeniden yönlendirilebilecek istek türünü tanımlayan bir istek URI deseni tanımlar. Yalnızca URL'si aşağıdaki ölçütlerin her ikisini de karşılayan istekler yeniden yönlendirilir: <br/> <br/> **Kaynak (veya içerik erişim noktası):** Bir kaynak sunucusu tanımlayan göreli bir yol seçin. Bu yol _/XXXX/_ bölümü ve bitiş noktası adınızdır. <br/><br/> **Kaynak (desen):** İstemli yola göre istekleri tanımlayan bir desen tanımlanmalıdır. Bu normal ifade deseni, daha önce seçili içerik erişim noktasından hemen sonra başlayan bir yol tanımlamalıdır (yukarıya bakın). <br/> - Uri'nin daha önce tanımladığı istek URI ölçütlerini (yani Kaynak & Desen) bu özellik için tanımlanan eşleşme koşullarıyla çakışmadığından emin olun. <br/> - Bir desen belirtin; desen olarak boş bir değer kullanırsanız, tüm dizeleri eşleşir.
Hedef| Yukarıdaki isteklerin yönlendirileceği URL'yi tanımlayın. <br/><br/> Bu URL'yi dinamik olarak şu şekilde oluşturur: <br/> - Düzenli bir ifade deseni <br/>- [HTTP değişkenleri](cdn-http-variables.md) <br/><br/> Kaynak deseninde yakalanan değerleri , n'nin yakalandığı sıraya göre bir değer _tanımladığı_ $_n_ kullanarak hedef desenine değiştirin. Örneğin, 1 TL kaynak desende yakalanan ilk değeri, 2 TL ise ikinci değeri temsil eder. <br/>

Mutlak bir URL kullanılması şiddetle tavsiye edilir. Göreli bir URL'nin kullanımı CDN URL'lerini geçersiz bir yola yönlendirebilir.

**Örnek Senaryo**

Bu örnek, bu temel CDN URL'sine çözen bir kenar CNAME\/URL'sini nasıl yeniden yönlendireceğini gösterir: http: /marketing.azureedge.net/brochures

Eleme istekleri bu temel kenar CNAME URL'ye yönlendirilir: http:\//cdn.mydomain.com/resources

Bu URL yönlendirmesi aşağıdaki yapılandırma ![ile elde edilebilir: URL yönlendirme](./media/cdn-rules-engine-reference/cdn-rules-engine-redirect.png)

**Önemli noktalar:**

- URL Yönlendirme özelliği, yeniden yönlendirilecek istek URL'lerini tanımlar. Sonuç olarak, ek maç koşulları gerekli değildir. Eşleşme koşulu "Her zaman" olarak tanımlanmış olsa da, yalnızca "pazarlama" müşteri kaynağındaki "broşürler" klasörüne işaret eden istekler yönlendirilir.
- Eşleşen tüm istekler, Hedef seçeneğinde tanımlanan kenar CNAME URL'sine yönlendirilir.
    - Örnek senaryo #1:
        - Örnek istek (CDN URL): http:\//marketing.azureedge.net/brochures/widgets.pdf
        - İstek URL 'i (yönlendirmeden sonra): http:\//cdn.mydomain.com/resources/widgets.pdf  
    - Örnek senaryo #2:
        - Örnek istek (Edge CNAME\/URL): http: /marketing.mydomain.com/brochures/widgets.pdf
        - İstek URL'si (yeniden\/yönlendirdikten sonra): http: /cdn.mydomain.com/resources/widgets.pdf Örnek senaryo
    - Örnek senaryo #3:
        - Örnek istek (Edge CNAME\/URL): http: /brochures.mydomain.com/campaignA/final/productC.ppt
        - İstek URL 'i (yönlendirmeden sonra): http:\//cdn.mydomain.com/resources/campaignA/final/productC.ppt 
- İstek Şeması (%{scheme}) değişkeni, yeniden yönlendirmeden sonra isteğin düzeninin değişmemesini sağlayan Hedef seçeneğinde yararlanılır.
- İstekten yakalanan URL segmentleri yeni URL'ye "$1" ile eklenir.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="url-rewrite"></a>URL Yeniden Yazma

**Amaç:** İstek URL'sini yeniden yazar.

Önemli bilgiler:

- Bu özelliğin yapılandırması, aşağıdaki seçenekleri ayarlamayı gerektirir:

Seçenek|Açıklama
-|-
 Kaynak & Deseni | Bu ayarlar, yeniden yazılabilecek istek türünü tanımlayan bir istek URI deseni tanımlar. Yalnızca URL'si aşağıdaki ölçütlerin her ikisini de karşılayan istekler yeniden yazılır: <br/><br/>  - **Kaynak (veya içerik erişim noktası):** Bir kaynak sunucusu tanımlayan göreli bir yol seçin. Bu yol _/XXXX/_ bölümü ve bitiş noktası adınızdır. <br/><br/> - **Kaynak (desen):** İstemli yola göre istekleri tanımlayan bir desen tanımlanmalıdır. Bu normal ifade deseni, daha önce seçili içerik erişim noktasından hemen sonra başlayan bir yol tanımlamalıdır (yukarıya bakın). <br/> Uri isteği ölçütlerinin (diğer bir başkası Kaynak & Deseni) daha önce tanımlanan bu özellik için tanımlanan eşleşme koşullarıyla çakışmadığını doğrulayın. Bir desen belirtin; desen olarak boş bir değer kullanırsanız, tüm dizeleri eşleşir.
 Hedef  |Yukarıdaki isteklerin aşağıdakiler tarafından yeniden yazılalıolarak göreli URL'yi tanımlayın: <br/>    1. Bir kaynak sunucusu tanımlayan bir içerik erişim noktası seçmek. <br/>    2. Göreceli bir yolu kullanarak tanımlama: <br/>        - Düzenli bir ifade deseni <br/>        - [HTTP değişkenleri](cdn-http-variables.md) <br/> <br/> Kaynak deseninde yakalanan değerleri , n'nin yakalandığı sıraya göre bir değer _tanımladığı_ $_n_ kullanarak hedef desenine değiştirin. Örneğin, 1 TL kaynak desende yakalanan ilk değeri, 2 TL ise ikinci değeri temsil eder.

 Bu özellik, SAHTE LIS'lerin geleneksel bir yönlendirme yapmadan URL'yi yeniden yazmalarına olanak tanır. Diğer bir de, istekte bulundurucu, yeniden yazılan URL istenmiş gibi aynı yanıt kodunu alır.

**Örnek Senaryo 1**

Bu örnek, bu temel CDN URL'sine çözen bir kenar CNAME\/URL'sini nasıl yönlendireceğini gösterir: http: /marketing.azureedge.net/brochures/

Eleme istekleri bu temel kenar CNAME URL'ye yönlendirilir: http:\//MyOrigin.azureedge.net/resources/

Bu URL yönlendirmesi aşağıdaki yapılandırma ![ile elde edilebilir: URL yönlendirme](./media/cdn-rules-engine-reference/cdn-rules-engine-rewrite.png)

**Örnek Senaryo 2**

Bu örnek, normal ifadeleri kullanarak kenar CNAME URL'nin KÜÇÜK HARFTEN KÜÇÜK HARFE nasıl yönlendirileni göstermektedir.

Bu URL yönlendirmesi aşağıdaki yapılandırma ![ile elde edilebilir: URL yönlendirme](./media/cdn-rules-engine-reference/cdn-rules-engine-to-lowercase.png)

**Önemli noktalar:**

- URL Yeniden Yazma özelliği, yeniden yazılacak istek URL'lerini tanımlar. Sonuç olarak, ek maç koşulları gerekli değildir. Eşleşme koşulu "Her zaman" olarak tanımlanmış olsa da, yalnızca "pazarlama" müşteri kaynağındaki "broşürler" klasörüne işaret eden istekler yeniden yazılır.

- İstekten yakalanan URL segmentleri yeni URL'ye "$1" ile eklenir.

#### <a name="compatibility"></a>Uyumluluk

Bu özellik, bir isteğe uygulanabilmesi için önce karşılanması gereken eşleşen ölçütleri içerir. Çakışan eşleşme ölçütleri ayarlamayı önlemek için, bu özellik aşağıdaki eşleşme koşullarıyla uyumsuzdur:

- AS Numarası
- CDN Menşei
- İstemci IP Adresi
- Müşteri Menşei
- İstek Şeması
- URL Yol Dizini
- URL Yolu Uzantısı
- URL Yolu Dosya Adı
- URL Yolu Literal
- URL Yolu Regex
- URL Yolu Joker
- URL Sorgusu Literal
- URL Sorgu Parametresi
- URL Sorgusu Regex
- URL Sorgusu Joker

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

---

### <a name="user-variable"></a>Kullanıcı Değişkeni

**Amaç:** Yalnızca dahili kullanım için.

[Başa dön](#azure-cdn-from-verizon-premium-rules-engine-features)

</br>

## <a name="next-steps"></a>Sonraki adımlar

- [Kural altyapısı başvurusu](cdn-verizon-premium-rules-engine-reference.md)
- [Kural altyapısı koşullu ifadeleri](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Kural altyapısı eşleştirme koşulları](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [Kurallar altyapısını kullanarak HTTP davranışını geçersiz kılma](cdn-verizon-premium-rules-engine.md)
- [Azure CDN'ye genel bakış](cdn-overview.md)