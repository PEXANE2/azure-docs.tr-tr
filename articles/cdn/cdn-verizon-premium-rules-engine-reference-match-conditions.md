---
title: Verizon Premium kuralları motor maç koşulları azure CDN | Microsoft Dokümanlar
description: Verizon Premium kuralları motor maç koşulları ndan Azure İçerik Teslim Ağı için başvuru belgeleri.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus
ms.openlocfilehash: 1660dca34b2f128ef5889145fcdeed0d2523b9bb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67593211"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-match-conditions"></a>Verizon Premium kuralları motor maç koşulları Azure CDN

Bu makalede, Verizon Premium [kuralları altyapısından](cdn-verizon-premium-rules-engine.md)Azure İçerik Dağıtım Ağı (CDN) için kullanılabilir eşleşme koşullarının ayrıntılı açıklamaları listelenmektedir.

Kuralın ikinci bölümü maç koşuludur. Eşmatch koşulu, bir dizi özelliğin gerçekleştirileceği belirli istek türlerini tanımlar.

Örneğin, bir eşleşme koşulu kullanabilirsiniz:

- İçerik isteklerini belirli bir konumda filtreleyin.
- Belirli bir IP adresinden veya ülke/bölgeden oluşturulan istekleri filtreleyin.
- Üstbilgi bilgilerine göre istekleri filtreleyin.

## <a name="always-match-condition"></a>Her zaman eşleşin koşul

Her Zaman eşleşme koşulu, tüm isteklere varsayılan bir özellik kümesi uygular.

Adı | Amaç
-----|--------
[Her zaman](#always) | Tüm isteklere varsayılan bir özellik kümesi uygular.

## <a name="device-match-condition"></a>Aygıt eşleşmesi durumu

Cihaz eşleştirme koşulu, özelliklerine göre bir mobil cihazdan gelen istekleri tanımlar.  

Adı | Amaç
-----|--------
[Cihaz](#device) | Bir mobil cihazdan gelen istekleri özelliklerine göre tanımlar.

## <a name="location-match-conditions"></a>Konum eşleşmekoşulları

Konum eşleştirme koşulları, istekte bulunanın konumuna bağlı olarak istekleri tanımlar.

Adı | Amaç
-----|--------
[AS Numarası](#as-number) | Belirli bir ağdan kaynaklanan istekleri tanımlar.
[Ülke](#country) | Belirtilen ülkelerden/bölgelerden kaynaklanan istekleri tanımlar.

## <a name="origin-match-conditions"></a>Başlangıç eşleşmesi koşulları

Origin eşleşme koşulları, İçerik Teslim Ağı depolamasını veya müşteri kökenli bir sunucuyu işaret eden istekleri tanımlar.

Adı | Amaç
-----|--------
[CDN Menşei](#cdn-origin) | İçerik Dağıtım Ağı depolama alanında depolanan içerik isteklerini tanımlar.
[Müşteri Menşei](#customer-origin) | Belirli bir müşteri kaynağı sunucusunda depolanan içerik isteklerini tanımlar.

## <a name="request-match-conditions"></a>Maç koşulları isteme

İstek eşleşmekoşulları, istekleri özelliklerine göre tanımlar.

Adı | Amaç
-----|--------
[İstemci IP Adresi](#client-ip-address) | Belirli bir IP adresinden kaynaklanan istekleri tanımlar.
[Çerez Parametresi](#cookie-parameter) | Belirtilen değer için her istekle ilişkili tanımlama bilgilerini denetler.
[Çerez Parametresi Regex](#cookie-parameter-regex) | Belirtilen normal ifade için her istekle ilişkili tanımlama bilgilerini denetler.
[Kenar Adı](#edge-cname) | Belirli bir kenar CNAME'yi işaret eden istekleri tanımlar.
[Yönlendiren Etki Alanı](#referring-domain) | Belirtilen ana bilgisayar adlarından yönlendirilen istekleri tanımlar.
[İstek Üstbilgi Literal](#request-header-literal) | Belirtilen üstbilgi kümesini içeren istekleri belirli bir değere tanımlar.
[İstek Üstbilgi Regex](#request-header-regex) | Belirtilen üstbilgi kümesini içeren istekleri, belirtilen normal ifadeyle eşleşen bir değere tanımlar.
[İstek Üstbilgi Joker](#request-header-wildcard) | Belirtilen üstbilgi kümesini içeren istekleri, belirtilen desenle eşleşen bir değere tanımlar.
[Request Yöntemi](#request-method) | İstekleri HTTP yöntemiyle tanımlar.
[İstek Şeması](#request-scheme) | İstekleri HTTP protokolüne göre tanımlar.

## <a name="url-match-conditions"></a>URL maç koşulları

URL eşleşme koşulları, URL'lerine göre istekleri tanımlar.

Adı | Amaç
-----|--------
[URL Yol Dizini](#url-path-directory) | İstekleri göreli yollarıyla tanımlar.
[URL Yolu Uzantısı](#url-path-extension) | İstekleri dosya adı uzantısına göre tanımlar.
[URL Yolu Dosya Adı](#url-path-filename) | İstekleri dosya adlarına göre tanımlar.
[URL Yolu Literal](#url-path-literal) | İsteğin göreli yolunu belirtilen değerle karşılaştırır.
[URL Yolu Regex](#url-path-regex) | İsteğin göreli yolunu belirtilen normal ifadeyle karşılaştırır.
[URL Yolu Joker](#url-path-wildcard) | İsteğin göreli yolunu belirtilen desenle karşılaştırır.
[URL Sorgusu Literal](#url-query-literal) | İsteğin sorgu dizesini belirtilen değerle karşılaştırır.
[URL Sorgu Parametresi](#url-query-parameter) | Belirtilen sorgu dize parametresini içeren istekleri, belirtilen desenle eşleşen bir değere ayarlar.
[URL Sorgusu Regex](#url-query-regex) | Belirtilen sorgu dize parametresini içeren istekleri, belirtilen normal ifadeyle eşleşen bir değere ayarlar.
[URL Sorgusu Joker](#url-query-wildcard) | Belirtilen değeri isteğin sorgu dizesi ile karşılaştırır.

## <a name="reference-for-rules-engine-match-conditions"></a>Kurallar motoru eşleştirme koşulları için başvuru

---

### <a name="always"></a>Her zaman

Her Zaman eşleşme koşulu, tüm isteklere varsayılan bir özellik kümesi uygular.

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="as-number"></a>AS Numarası

AS Numarası ağı, özerk sistem numarası (ASN) ile tanımlanır. 

**Eşleşmeler**/**Eşleşmez** seçeneği, AS Numarası eşleşmesi koşulunun hangi koşullaraltında karşılandığını belirler:

- **Eşleşmeler**: İstemci ağının ASN'sinin belirtilen ASN'lerden biriyle eşleşmesini gerektirir. 
- **Eşleşmez**: İstemci ağının ASN'sinin belirtilen ASN'lerin hiçbiriyle eşleşmemesi gerektiğini gerektirir.

Önemli bilgiler:

- Her birini tek bir boşlukla sınırlandırarak birden çok ASN belirtin. Örneğin, 64514 64515 64514 veya 64515 gelen isteklerini.
- Bazı istekler geçerli bir ASN döndürmeyebilir. Bir soru işareti (?) için geçerli bir ASN belirlenemeyen istekleri eşleşir.
- İstenilen ağ için ASN'nin tamamını belirtin. Kısmi değerler eşleşmez.
- Önbellek ayarlarının izlenme biçimi nedeniyle, bu eşleşme koşulu aşağıdaki özelliklerle uyumsuzdur:
  - Önbellek Dolgusu Tamamlandı
  - Varsayılan Dahili Max-Age
  - Kuvvet İç Max-Age
  - Kaynak Yokyokla No-Önbellek
  - Dahili Max-Bayat

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="cdn-origin"></a>CDN Menşei

CDN Origin eşleşmesi koşulu, aşağıdaki koşulların her ikisi de karşılandığında karşılanır:

- CDN depolamadan içerik istendi.
- Uri isteği, bu eşleşme koşulunda tanımlanan içerik erişim noktası türünü (örneğin, /000001) kullanır:
  - CDN URL'si: İstek URI seçili içerik erişim noktasını içermelidir.
  - Kenar CNAME URL'si: İlgili kenar CNAME yapılandırması seçili içerik erişim noktasını işaret etmelidir.
  
Önemli bilgiler:

- İçerik erişim noktası, istenen içeriğe hizmet etmesi gereken hizmeti tanımlar.
- Belirli eşleşme koşullarını birleştirmek için BIR AND IF deyimi kullanmayın. Örneğin, BIR CDN Origin eşleşmesi koşulunu Müşteri Kaynağı eşleşme koşuluyla birleştirmek, asla eşleştirilmeyecek bir eşleşme deseni oluşturur. Bu nedenle, iki CDN Origin eşleşme sart koşulu bir AND IF deyimi ile birleştirilemez.

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="client-ip-address"></a>İstemci IP Adresi

**Eşleşmeler**/**Eşleşmez** seçeneği, Istemci IP Adresi eşleşme koşulunun hangi koşullar altında karşılandığını belirler:

- **Eşleşmeler**: İstemcinin IP adresinin belirtilen IP adreslerinden biriyle eşleşmesini gerektirir. 
- **Eşleşmez**: İstemcinin IP adresinin belirtilen IP adreslerinin hiçbirine eşleşmemesi gerekmez. 

Önemli bilgiler:

- CIDR gösterimini kullanın.
- Her birini tek bir boşlukla sınırlandırarak birden çok IP adresi ve/veya IP adresi bloğu belirtin. Örnek:
  - **IPv4 örneği**: 1.2.3.4 10.20.30.40 adresten gelen isteklerle 1.2.3.4 veya 10.20.30.40 ile eşleşir.
  - **IPv6 örneği**: 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 her iki adresten gelen isteklerle eşleşir 1:2:3:4:5:6:7:8 veya 10:20:30:40:50:60:70:80.
- IP adresi bloğu sözdizimi, bir ileri eğik çizgi ve önek boyutu ardından temel IP adresidir. Örnek:
  - **IPv4 örneği**: 5.5.5.64/26, 5.5.5.64 ile 5.5.5.127 adreslerinden gelen tüm isteklerle eşleşir.
  - **IPv6 örneği**: 1:2:3:/48 adreslerinden gelen tüm isteklerle eşleşir 1:2:3:0:0:0:0 0 ile 1:2:3:ffff:ffff:ffff:ffff:ffff.ffff.
- Önbellek ayarlarının izlenme biçimi nedeniyle, bu eşleşme koşulu aşağıdaki özelliklerle uyumsuzdur:
  - Önbellek Dolgusu Tamamlandı
  - Varsayılan Dahili Max-Age
  - Kuvvet İç Max-Age
  - Kaynak Yokyokla No-Önbellek
  - Dahili Max-Bayat

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="cookie-parameter"></a>Çerez Parametresi

**Eşleşmeler**/**Eşleşmez** seçeneği, Çerez Parametresi eşleşme sn koşulunun hangi koşullar altında karşılandığını belirler.

- **Eşleşmeler**: Bu eşleşme koşulunda tanımlanan değerlerden en az biriyle eşleşen bir değere sahip belirtilen çerezi içerme isteği gerektirir.
- **Eşleşmez**: İsteğin aşağıdaki ölçütlerden birini karşılamasını gerektirir:
  - Belirtilen çerezi içermez.
  - Belirtilen çerezi içerir, ancak değeri bu eşleşme koşulunda tanımlanan değerlerin hiçbiriyle eşleşmez.
  
Önemli bilgiler:

- Çerez adı:
  - Yıldız işaretleri (*) dahil olmak üzere joker karakter değerleri, bir çerez adı belirtirken desteklenmedığından, yalnızca tam çerez adı eşleşmeleri karşılaştırma için uygundur.
  - Bu eşleşme koşulunun örneğine göre yalnızca tek bir çerez adı belirtilebilir.
  - Çerez adı karşılaştırmaları büyük/küçük harf duyarsızdır.
- Çerez değeri:
  - Her birini tek bir boşlukla sınırlandırarak birden çok çerez değeri belirtin.
  - Çerez değeri [joker karakter değerlerinden](cdn-verizon-premium-rules-engine-reference.md#wildcard-values)yararlanabilir.
  - Joker karakter değeri belirtilmemişse, yalnızca tam bir eşleşme bu eşleşme koşulunu karşılar. Örneğin, "Değer" belirtmek "Değer" ile eşleşir, ancak "Değer1" veya "Değer2" ile eşleşmez.
  - İsteğin çerez değerine karşı büyük/küçük harf duyarlı bir karşılaştırma yapılıp yapılmadığını denetlemek için **Büyük/Küçük Harf Yoksay** seçeneğini kullanın.
- Önbellek ayarlarının izlenme biçimi nedeniyle, bu eşleşme koşulu aşağıdaki özelliklerle uyumsuzdur:
  - Önbellek Dolgusu Tamamlandı
  - Varsayılan Dahili Max-Age
  - Kuvvet İç Max-Age
  - Kaynak Yokyokla No-Önbellek
  - Dahili Max-Bayat

[Başa dön](#reference-for-rules-engine-match-conditions)
</br>

---

### <a name="cookie-parameter-regex"></a>Çerez Parametresi Regex

Çerez Parametresi Regex maç koşulu bir çerez adı ve değeri tanımlar. İstenilen çerez değerini tanımlamak için [normal ifadeler](cdn-verizon-premium-rules-engine-reference.md#regular-expressions) kullanabilirsiniz.

**Eşleşmeler**/**Eşleşmez** seçeneği, Çerez Parametresi Regex eşleşme koşulunun hangi koşullar altında karşılandığını belirler.

- **Eşleşmeler**: Belirtilen normal ifadeyle eşleşen bir değere sahip belirtilen çerezi içerme isteği gerektirir.
- **Eşleşmez**: İsteğin aşağıdaki ölçütlerden birini karşılamasını gerektirir:
  - Belirtilen çerezi içermez.
  - Belirtilen çerezi içerir, ancak değeri belirtilen normal ifadeyle eşleşmez.
  
Önemli bilgiler:

- Çerez adı:
  - Yıldız işaretleri (*) dahil olmak üzere normal ifadeler ve joker karakter değerleri, çerez adı belirtirken desteklenmedığından, yalnızca tam çerez adı eşleşmeleri karşılaştırma için uygundur.
  - Bu eşleşme koşulunun örneğine göre yalnızca tek bir çerez adı belirtilebilir.
  - Çerez adı karşılaştırmaları büyük/küçük harf duyarsızdır.
- Çerez değeri:
  - Çerez değeri normal ifadelerden yararlanabilir.
  - İsteğin çerez değerine karşı büyük/küçük harf duyarlı bir karşılaştırma yapılıp yapılmadığını denetlemek için **Büyük/Küçük Harf Yoksay** seçeneğini kullanın.
- Önbellek ayarlarının izlenme biçimi nedeniyle, bu eşleşme koşulu aşağıdaki özelliklerle uyumsuzdur:
  - Önbellek Dolgusu Tamamlandı
  - Varsayılan Dahili Max-Age
  - Kuvvet İç Max-Age
  - Kaynak Yokyokla No-Önbellek
  - Dahili Max-Bayat

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="country"></a>Ülke

Bir ülkeyi ülke koduyla belirtebilirsiniz. 

**Eşleşmeler**/**Eşleşmez** seçeneği, Ülke eşleşmesi koşulunun hangi koşullaraltında karşılandığını belirler:

- **Eşleşmeler**: Belirtilen ülke kodu değerlerini içerme isteğini gerektirir. 
- **Eşleşmez**: İsteğin belirtilen ülke kodu değerlerini içermemesini gerektirir.

Önemli bilgiler:

- Her birini tek bir alanla sınırlandırarak birden çok ülke kodu belirtin.
- Ülke kodu belirtirken joker karakterler desteklenmez.
- "AB" ve "AP" ülke kodları bu bölgelerdeki tüm IP adreslerini kapsamaz.
- Belirli istekler geçerli bir ülke kodu döndürmeyebilir. Soru işareti (?) için geçerli bir ülke kodu belirlenemeyen istekleri eşleşir.
- Ülke kodları büyük/küçük harf duyarlıdır.
- Önbellek ayarlarının izlenme biçimi nedeniyle, bu eşleşme koşulu aşağıdaki özelliklerle uyumsuzdur:
  - Önbellek Dolgusu Tamamlandı
  - Varsayılan Dahili Max-Age
  - Kuvvet İç Max-Age
  - Kaynak Yokyokla No-Önbellek
  - Dahili Max-Bayat

#### <a name="implementing-country-filtering-by-using-the-rules-engine"></a>Kurallar altyapısını kullanarak Ülke Filtreleme uygulaması

Bu eşleşme koşulu, bir isteğin kaynaklandigi konuma göre çok sayıda özelleştirme gerçekleştirmenize olanak tanır. Örneğin, Ülke Filtreleme özelliğinin davranışı aşağıdaki yapılandırma aracılığıyla çoğaltılabilir:

- URL Path Wildcard eşleşmesi: [URL Path Wildcard maç koşulunu](#url-path-wildcard) güvenli olacak dizine ayarlayın. 
    Tüm çocuklarına erişimin bu kuralla kısıtlanmasından emin olmak için göreli yolun sonuna bir yıldız işareti ekin.

- Ülke maçı: Ülke maç koşulunu istenilen ülkeler kümesine ayarlayın.
  - İzin Ver: Ülke eşleşmesi koşulunu, yalnızca belirtilen ülkelerin URL Yolu Joker Kart eşleşmesi koşuluyla tanımlanan konumda depolanan içeriğe erişmesine izin vermek için **Eşleşmiyor** olarak ayarlayın.
  - Blok: Belirtilen ülkelerin URL Yolu Joker Kart eşleşmesi koşuluyla tanımlanan konumda depolanan içeriğe erişmelerini engellemek için Ülke maç koşulunu **Maçlar** olarak ayarlayın.

- Access'i Reddet (403) Özelliği: Ülke Filtreleme özelliğinin izin veya blok kısmını çoğaltmak için [Access'i Reddet (403) özelliğini](cdn-verizon-premium-rules-engine-reference-features.md#deny-access-403) etkinleştirin.

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="customer-origin"></a>Müşteri Menşei

Önemli bilgiler:

- Müşteri Kaynağı eşleşme sn. Içerik CDN URL'si veya seçili müşteri kaynağını gösteren bir kenar CNAME URL'si aracılığıyla istenip istenmediğine bakılmaksızın karşılanır.
- Kuralla başvurulan müşteri kökenli yapılandırma, Müşteri Kaynağı sayfasından silinemez. Bir müşteri kaynağı yapılandırmasını silmeyi denemeden önce, aşağıdaki yapılandırmaların bu yapılandırmaya başvurmadığından emin olun:
  - Müşteri Kaynağı eşleşme koşulu
  - Kenar CNAME yapılandırması
- Belirli eşleşme koşullarını birleştirmek için BIR AND IF deyimi kullanmayın. Örneğin, Müşteri Kaynağı eşleşme koşulunu CDN Origin eşleşme sn. koşuluyla birleştirmek, asla eşleştirilmeyecek bir eşleşme deseni oluşturur. Bu nedenle, iki Müşteri Kaynağı eşleşme koşulu bir AND IF deyimi ile birleştirilemez.

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="device"></a>Cihaz

Cihaz eşleştirme koşulu, özelliklerine göre bir mobil cihazdan gelen istekleri tanımlar. Mobil cihaz algılama [WURFL](http://wurfl.sourceforge.net/)ile elde edilir. 

**Eşler**/**Eşleşmiyor** seçeneği, Aygıt eşleşmesi koşulunun hangi koşullaraltında karşılandığını belirler:

- **Eşleşmeler**: İsteklinin cihazının belirtilen değerle eşleşmesini gerektirir. 
- **Eşleşmez**: İsteklinin cihazının belirtilen değerle eşleşmemesi gerekir.

Önemli bilgiler:

- Belirtilen değerin büyük/küçük harf duyarlı olup olmadığını belirtmek için **Büyük/Küçük/Sayma** seçeneğini kullanın.
- Önbellek ayarlarının izlenme biçimi nedeniyle, bu eşleşme koşulu aşağıdaki özelliklerle uyumsuzdur:
  - Önbellek Dolgusu Tamamlandı
  - Varsayılan Dahili Max-Age
  - Kuvvet İç Max-Age
  - Kaynak Yokyokla No-Önbellek
  - Dahili Max-Bayat

#### <a name="string-type"></a>Dize Türü

WURFL özelliği genellikle sayıların, harflerin ve sembollerin birleşimini kabul eder. Bu özelliğin esnek yapısı nedeniyle, bu eşleşme koşuluyla ilişkili değerin nasıl yorumlandığını seçmeniz gerekir. Aşağıdaki tabloda kullanılabilir seçenekler kümesi açıklanmaktadır:

Tür     | Açıklama
---------|------------
Literal  | Çoğu karakterin [gerçek değerlerini](cdn-verizon-premium-rules-engine-reference.md#literal-values)kullanarak özel anlam lar almasını önlemek için bu seçeneği belirleyin.
Joker | Tüm joker karakterlerden[(joker](cdn-verizon-premium-rules-engine-reference.md#wildcard-values)karakter] yararlanmak için bu seçeneği seçin.
Regex    | [Normal ifadeleri](cdn-verizon-premium-rules-engine-reference.md#regular-expressions)kullanmak için bu seçeneği seçin. Normal ifadeler, bir karakter deseni tanımlamak için yararlıdır.

#### <a name="wurfl-capabilities"></a>WURFL yetenekleri

WURFL özelliği, mobil cihazları açıklayan bir kategoriyi ifade eder. Seçili özellik, istekleri tanımlamak için kullanılan mobil aygıt açıklamasının türünü belirler.

Aşağıdaki tabloda WURFL yetenekleri ve kurallar motoru için değişkenleri listeleneb.rö'dur.

> [!NOTE]
> Aşağıdaki **değişkenler, İstemci İstek Üstbilgisini Değiştir** ve **İstemci Yanıtı Üstbilgi** özelliklerini değiştir'de desteklenir.

Özellik | Değişken | Açıklama | Örnek değerler
-----------|----------|-------------|----------------
Markası | %{wurfl_cap_brand_name} | Aygıtın marka adını gösteren bir dize. | Samsung
Cihaz İşletim Sistemi | %{wurfl_cap_device_os} | Aygıta yüklü işletim sistemini gösteren bir dize. | IOS
Cihaz İşletim Sistemi Sürümü | %{wurfl_cap_device_os_version} | Aygıta yüklenen işletim sisteminin sürüm numarasını gösteren dize. | 1.0.1
Çift Oryantasyon | %{wurfl_cap_dual_orientation} | Aygıtın çift yönlendirmeyi destekleyip desteklemediğini gösteren bir Boolean. | true
HTML Tercih Edilen DTD | %{wurfl_cap_html_preferred_dtd} | HTML içeriği için mobil aygıtın tercih edilen belge türü tanımını (DTD) gösteren dize. | yok<br/>xhtml_basic<br/>html5
Görüntü Inlining | %{wurfl_cap_image_inlining} | Aygıtın Base64 kodlanmış görüntüleri destekleyip desteklemediğini gösteren bir Boolean. | yanlış
Android mi | %{wurfl_vcap_is_android} | Cihazın Android işletim sistemi kullanıp kullanmadığını gösteren bir Boolean. | true
IOS olduğunu | %{wurfl_vcap_is_ios} | Aygıtın iOS kullanıp kullanmadığını gösteren bir Boolean. | yanlış
Akıllı TV mi | %{wurfl_cap_is_smarttv} | Aygıtın akıllı tv olup olmadığını gösteren bir Boolean. | yanlış
Akıllı Telefon mu | %{wurfl_vcap_is_smartphone} | Cihazın akıllı telefon olup olmadığını gösteren bir Boolean. | true
Tablet mi | %{wurfl_cap_is_tablet} | Aygıtın tablet olup olmadığını gösteren bir Boolean. Bu açıklama işletim sistemi bağımsızdır. | true
Kablosuz Aygıt mı | %{wurfl_cap_is_wireless_device} | Aygıtın kablosuz aygıt olarak kabul edilip edilemeyeceğini gösteren bir Boolean. | true
Pazarlama Adı | %{wurfl_cap_marketing_name} | Aygıtın pazarlama adını gösteren dize. | Böğürtlen 8100 İnci
Mobil Tarayıcı | %{wurfl_cap_mobile_browser} | Aygıttan içerik istemek için kullanılan tarayıcıyı gösteren dize. | Chrome
Mobil Tarayıcı Sürümü | %{wurfl_cap_mobile_browser_version} | Aygıttan içerik istemek için kullanılan tarayıcının sürümünü gösteren dize. | 31
Model Adı | %{wurfl_cap_model_name} | Aygıtın model adını gösteren bir dize. | s3
Aşamalı İndir | %{wurfl_cap_progressive_download} | Aygıtın hala indirilirken ses ve video çalmayı destekleyip desteklemediğini belirten bir Boolean. | true
Yayın Tarihi | %{wurfl_cap_release_date} | Aygıtın WURFL veritabanına eklendiği yılı ve ayı gösteren dize.<br/><br/>Biçim:`yyyy_mm` | 2013_december
Çözünürlük Yüksekliği | %{wurfl_cap_resolution_height} | Aygıtın piksel yüksekliğini gösteren bir sonsayı. | 768
Çözünürlük Genişliği | %{wurfl_cap_resolution_width} | Aygıtın piksel genişliğini gösteren bir sonsayı. | 1024

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="edge-cname"></a>Kenar Adı

Önemli bilgiler:

- Kullanılabilir kenar KNAM'larının listesi, kural altyapısının yapılandırıldığı platform için Kenar KNAMEs sayfasında yapılandırılan kenar KNAM'larla sınırlıdır.
- Bir kenar CNAME yapılandırmasını silmeyi denemeden önce, Kenar Cname eşleşmesi koşulunun bu yapılandırmaya başvurmadığından emin olun. Bir kuralda tanımlanan Kenar CNAME yapılandırmaları Kenar CNAMEs sayfasından silinemez.
- Belirli eşleşme koşullarını birleştirmek için BIR AND IF deyimi kullanmayın. Örneğin, Edge Cname eşleşmesi koşulunu Müşteri Kaynağı eşleşme koşuluyla birleştirmek, asla eşleştirilemeyecek bir eşleşme deseni oluşturur. Bu nedenle, iki Kenar Cname eşleşme sart koşulu bir AND IF deyimi ile birleştirilemez.
- Önbellek ayarlarının izlenme biçimi nedeniyle, bu eşleşme koşulu aşağıdaki özelliklerle uyumsuzdur:
  - Önbellek Dolgusu Tamamlandı
  - Varsayılan Dahili Max-Age
  - Kuvvet İç Max-Age
  - Kaynak Yokyokla No-Önbellek
  - Dahili Max-Bayat

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="referring-domain"></a>Yönlendiren Etki Alanı

İçeriğin istendiği referrer ile ilişkili ana bilgisayar adı, Yönlendiren Etki Alanı koşulunun karşılanıp karşılanmadığını belirler.

**Eşlemez**/**seçeneği,** Yönlendiren Etki Alanı eşleşmesi koşulunun hangi koşullaraltında karşılandığını belirler:

- **Eşleşmeler**: Belirtilen değerlerle eşleşecek şekilde başvuran ana bilgisayar adının olmasını gerektirir. 
- **Eşleşmez**: Başvuran ana bilgisayar adının belirtilen değerle eşleşmemesi için gerekli değildir.

Önemli bilgiler:

- Her birini tek bir boşlukla sınırlandırarak birden çok ana bilgisayar adı belirtin.
- Bu eşleşme koşulu [joker karakter değerlerini](cdn-verizon-premium-rules-engine-reference.md#wildcard-values)destekler.
- Belirtilen değer bir yıldız işareti içermiyorsa, başvuranın ana bilgisayar adı için tam bir eşleşme olmalıdır. Örneğin, "mydomain.com" belirtmek "www.mydomain.com" ile eşleşmez.
- Büyük/küçük harf duyarlı bir karşılaştırma yapılıp yapılmadığını denetlemek için **Büyük/Küçük/Küçük Harf Seçeneğini Zedele** seçeneğini kullanın.
- Önbellek ayarlarının izlenme biçimi nedeniyle, bu eşleşme koşulu aşağıdaki özelliklerle uyumsuzdur:
  - Önbellek Dolgusu Tamamlandı
  - Varsayılan Dahili Max-Age
  - Kuvvet İç Max-Age
  - Kaynak Yokyokla No-Önbellek
  - Dahili Max-Bayat

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---  

### <a name="request-header-literal"></a>İstek Üstbilgi Literal

**Eşleşmeler**/**Eşleşmez** seçeneği, İstek Üstbilgi Literal eşleşme koşulunun hangi koşullar altında karşılandığını belirler.

- **Eşleşmeler**: Belirtilen üstbilginin içermesi için istek gerektirir. Değeri, bu eşleşme koşulunda tanımlanan değerle eşleşmelidir.
- **Eşleşmez**: İsteğin aşağıdaki ölçütlerden birini karşılamasını gerektirir:
  - Belirtilen üstbilgi içermez.
  - Belirtilen üstbilgi içerir, ancak değeri bu eşleşme koşulunda tanımlanan ile eşleşmez.
  
Önemli bilgiler:

- Üstbilgi adı karşılaştırmaları her zaman büyük/küçük harf duyarsızdır. Üstbilgi değeri karşılaştırmalarının büyük/küçük harf duyarlılığını denetlemek için **Büyük/Küçük Harf** Seçeneğini kullanın.
- Önbellek ayarlarının izlenme biçimi nedeniyle, bu eşleşme koşulu aşağıdaki özelliklerle uyumsuzdur:
  - Önbellek Dolgusu Tamamlandı
  - Varsayılan Dahili Max-Age
  - Kuvvet İç Max-Age
  - Kaynak Yokyokla No-Önbellek
  - Dahili Max-Bayat

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---  

### <a name="request-header-regex"></a>İstek Üstbilgi Regex

**Eşleşmeler**/**Eşleşmez** seçeneği, İstek Başlığı Regex eşleşmesi koşulunun hangi koşullar altında karşılandığını belirler.

- **Eşleşmeler**: Belirtilen üstbilginin içermesi için istek gerektirir. Değeri, belirtilen [normal ifadede](cdn-verizon-premium-rules-engine-reference.md#regular-expressions)tanımlanan desenle eşleşmelidir.
- **Eşleşmez**: İsteğin aşağıdaki ölçütlerden birini karşılamasını gerektirir:
  - Belirtilen üstbilgi içermez.
  - Belirtilen üstbilgi içerir, ancak değeri belirtilen normal ifadeyle eşleşmez.

Önemli bilgiler:

- Üstbilgi adı:
  - Üstbilgi adı karşılaştırmaları büyük/küçük harf duyarsızdır.
  - Üstbilgi adındaki boşlukları "%20" ile değiştirin.
- Üstbilgi değeri:
  - Üstbilgi değeri normal ifadelerden yararlanabilir.
  - Üstbilgi değeri karşılaştırmalarının büyük/küçük harf duyarlılığını denetlemek için **Büyük/Küçük Harf** Seçeneğini kullanın.
  - Eşleşme koşulu, yalnızca bir üstbilgi değeri belirtilen desenlerden en az biriyle tam olarak eşleştiğinde karşılanır.
- Önbellek ayarlarının izlenme biçimi nedeniyle, bu eşleşme koşulu aşağıdaki özelliklerle uyumsuzdur:
  - Önbellek Dolgusu Tamamlandı
  - Varsayılan Dahili Max-Age
  - Kuvvet İç Max-Age
  - Kaynak Yokyokla No-Önbellek
  - Dahili Max-Bayat

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="request-header-wildcard"></a>İstek Üstbilgi Joker

**Eşleşmeler**/**Eşleşmez** seçeneği, İstek Başlığı Wildcard eşleşmesi koşulunun hangi koşullar altında karşılandığını belirler.

- **Eşleşmeler**: Belirtilen üstbilginin içermesi için istek gerektirir. Değeri, bu eşleşme koşulunda tanımlanan değerlerden en az biriyle eşleşmelidir.
- **Eşleşmez**: İsteğin aşağıdaki ölçütlerden birini karşılamasını gerektirir:
  - Belirtilen üstbilgi içermez.
  - Belirtilen üstbilgi içerir, ancak değeri belirtilen değerlerin hiçbiriyle eşleşmez.
  
Önemli bilgiler:

- Üstbilgi adı:
  - Üstbilgi adı karşılaştırmaları büyük/küçük harf duyarsızdır.
  - Üstbilgi adındaki boşluklar "%20" ile değiştirilmelidir. Üstbilgi değerindeki boşlukları belirtmek için bu değeri de kullanabilirsiniz.
- Üstbilgi değeri:
  - Üstbilgi değeri [joker karakter değerlerinden](cdn-verizon-premium-rules-engine-reference.md#wildcard-values)yararlanabilir.
  - Üstbilgi değeri karşılaştırmalarının büyük/küçük harf duyarlılığını denetlemek için **Büyük/Küçük Harf** Seçeneğini kullanın.
  - Bu eşleşme koşulu, bir üstbilgi değeri belirtilen desenlerden en az biriyle tam olarak eşleştiğinde karşılanır.
  - Her birini tek bir boşlukla delerek birden çok değer belirtin.
- Önbellek ayarlarının izlenme biçimi nedeniyle, bu eşleşme koşulu aşağıdaki özelliklerle uyumsuzdur:
  - Önbellek Dolgusu Tamamlandı
  - Varsayılan Dahili Max-Age
  - Kuvvet İç Max-Age
  - Kaynak Yokyokla No-Önbellek
  - Dahili Max-Bayat

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="request-method"></a>Request Yöntemi

İstek Yöntemi eşleşme sn koşulu yalnızca seçili istek yöntemi aracılığıyla varlıklar istendiğinde karşılanır. Kullanılabilir istek yöntemleri şunlardır:

- GET
- HEAD
- POST
- Seçenekler
- PUT
- DELETE
- TRACE
- Bağlamak

Önemli bilgiler:

- Varsayılan olarak, ağda önbelleğe alınmış içeriği yalnızca GET isteği yöntemi oluşturabilir. Diğer tüm istek yöntemleri ağ üzerinden tamamlanır.
- Önbellek ayarlarının izlenme biçimi nedeniyle, bu eşleşme koşulu aşağıdaki özelliklerle uyumsuzdur:
  - Önbellek Dolgusu Tamamlandı
  - Varsayılan Dahili Max-Age
  - Kuvvet İç Max-Age
  - Kaynak Yokyokla No-Önbellek
  - Dahili Max-Bayat

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="request-scheme"></a>İstek Şeması

İstek Şeması eşleşmesi koşulu yalnızca seçili protokol aracılığıyla varlıklar istendiğinde karşılanır. Kullanılabilir protokoller şunlardır:

- HTTP
- HTTPS

Önemli bilgiler:

- Önbellek ayarlarının izlenme biçimi nedeniyle, bu eşleşme koşulu aşağıdaki özelliklerle uyumsuzdur:
  - Önbellek Dolgusu Tamamlandı
  - Varsayılan Dahili Max-Age
  - Kuvvet İç Max-Age
  - Kaynak Yokyokla No-Önbellek
  - Dahili Max-Bayat

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-directory"></a>URL Yol Dizini

İstenilen varlığın dosya adını dışlayan göreli yolu ile bir isteği tanımlar.

**Eşleşmeler**/**Eşleşmez** seçeneği, URL Yol Dizini eşleşmesinin hangi koşullar altında karşılandığını belirler.

- **Eşleşmeler**: Belirtilen URL deseniyle eşleşen dosya adı hariç, göreli bir URL yolu içerme isteği gerektirir.
- **Eşleşmiyor**: Dosya adı hariç, belirtilen URL deseniyle eşleşmeyen göreli bir URL yolu içerme isteği gerektirir.

Önemli bilgiler:

- URL karşılaştırmasının içerik erişim noktasından önce mi yoksa sonra mı başlayacağını belirtmek **için Göreli** seçeneğini kullanın. İçerik erişim noktası, yolun Verizon CDN ana bilgisayar adı ile istenen varlığa göreli yol (örneğin, /800001/CustomerOrigin) arasında görünen bölümüdür. Bir konumu sunucu türüne (örneğin, CDN veya müşteri kaynağı) ve müşteri hesap numaranıza göre tanımlar.

   **Göreli** seçeneği için aşağıdaki değerler kullanılabilir:
  - **Root**: URL karşılaştırma noktasının CDN ana bilgisayar adından hemen sonra başladığını gösterir. 

  Örneğin: http:\//wpc.0001. &lt;etki&gt;/alanı**800001/myorigin/myfolder**/index.htm

  - **Başlangıç**: URL karşılaştırma noktasının içerik erişim noktasından sonra başladığını gösterir (örneğin, /000001 veya /800001/myorigin). \*.azureedge.net CNAME varsayılan olarak Verizon CDN ana bilgisayar adındaki kaynak dizine göre oluşturulduğundan, Azure CDN **kullanıcılarıNın Başlangıç** değerini kullanması gerekir. 

  Örneğin: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder**/index.htm 

  Bu URL aşağıdaki Verizon CDN hostname\/puan: http: /wpc.0001. &lt;etki&gt;alanı /800001/myorigin/**myfolder**/index.htm

- Kenarcname URL, URL karşılaştırması öncesinde BIR CDN URL'sine yeniden yazılır.

    Örneğin, aşağıdaki URL'lerin her ikisi de aynı varlığı işaret eder ve bu nedenle aynı URL yoluna sahiptir.
  - CDN URL:\/http: /wpc.0001. &lt;etki&gt;alanı /800001/CustomerOrigin/path/asset.htm
    
  - Kenar CNAME URL:\//&lt;http: bitiş noktası&gt;.azureedge.net/path/asset.htm
    
    Ek bilgiler:
  - Özel etki alanı: https:\//my.domain.com/path/asset.htm
    
    - URL yolu (köke göre): /800001/CustomerOrigin/path/
    
    - URL yolu (menşee göre): /yol/

- URL karşılaştırması için kullanılan URL bölümü, istenen varlığın dosya adından hemen önce sona erer. Bu tür yoldaki son karakter, ileriye doğru çizgi çizen son karakterdir.

- URL yolu desenindeki boşlukları "%20" ile değiştirin.

- Her URL yolu deseni, her yıldız işaretinin bir veya daha fazla karakterden oluşan bir diziyle eşleştiği bir veya daha fazla yıldız işareti (*) içerebilir.

- Her birini tek bir alanla sınırlandırarak desendeki birden çok URL yolunu belirtin.

    Örneğin: */satış/ */marketing/

- URL yol belirtimi [joker karakter değerlerinden](cdn-verizon-premium-rules-engine-reference.md#wildcard-values)yararlanabilir.

- Büyük/küçük harf duyarlı bir karşılaştırma yapılıp yapılmadığını denetlemek için **Büyük/Küçük/Küçük Harf Seçeneğini Zedele** seçeneğini kullanın.

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-extension"></a>URL Yolu Uzantısı

İstenen varlığın dosya uzantısı tarafından istekleri tanımlar.

**Eşleşmeler**/**Eşleşmez** seçeneği, URL Yol Uzantısı eşleşme koşulunun hangi koşullar altında karşılandığını belirler.

- **Eşleşmeler**: Belirtilen desenle tam olarak eşleşen bir dosya uzantısı içermek için isteğin URL'sini gerektirir.

   Örneğin, "htm", "htm" varlıkları eşleşir, ancak "html" varlıkları belirtirseniz.  

- **Eşleşmiyor**: URL isteğinin belirtilen desenle eşleşmeyen bir dosya uzantısı içermesini gerektirir.

Önemli bilgiler:

- **Değer** kutusunda eşleşecek dosya uzantılarını belirtin. Öncü bir dönemi dahil etmeyin; örneğin, .htm yerine htm kullanın.

- Büyük/küçük harf duyarlı bir karşılaştırma yapılıp yapılmadığını denetlemek için **Büyük/Küçük/Küçük Harf Seçeneğini Zedele** seçeneğini kullanın.

- Her uzantıyı tek bir alanla sınırlandırarak birden çok dosya uzantısı belirtin. 

    Örneğin: htm html

- Örneğin, "htm" belirtmek "htm" varlıklarıyla eşleşir, ancak "html" varlıklarıyla eşleşmez.

#### <a name="sample-scenario"></a>Örnek Senaryo

Aşağıdaki örnek yapılandırma, bir istek belirtilen uzantılardan biriyle eşleştiğinde bu eşleşme koşulunun karşılandığını varsayar.

Değer belirtimi: asp aspx php html

Bu eşleşme koşulu, aşağıdaki uzantılarla biten URL'leri bulduğunda karşılanır:

- Asp
- Aspx
- .php
- .html

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-filename"></a>URL Yolu Dosya Adı

İstenen varlığın dosya adına göre istekleri tanımlar. Bu eşleşme koşulunun amaçları doğrultusunda, bir dosya adı istenen varlığın adı, bir dönem ve dosya uzantısı (örneğin, index.html) oluşur.

**Eşleşmeler**/**Eşleşmez** seçeneği, URL Yolu Dosya Adı eşleşmesi koşulunun hangi koşullar altında karşılandığını belirler.

- **Eşleşmeler**: URL yolunda belirtilen desenle eşleşen bir dosya adı içerme isteği gerektirir.
- **Eşleşmiyor**: Url yolunda belirtilen desenle eşleşmeyen bir dosya adı içerme isteği gerektirir.

Önemli bilgiler:

- Büyük/küçük harf duyarlı bir karşılaştırma yapılıp yapılmadığını denetlemek için **Büyük/Küçük/Küçük Harf Seçeneğini Zedele** seçeneğini kullanın.

- Birden çok dosya uzantısı belirtmek için, her uzantıyı tek bir boşlukla ayırın.

    Örneğin: index.htm index.html

- Dosya adı değerindeki boşlukları "%20" ile değiştirin.

- Dosya adı değeri joker [karakter değerlerinden](cdn-verizon-premium-rules-engine-reference.md#wildcard-values)yararlanabilir. Örneğin, her dosya adı deseni, her yıldız işaretinin bir veya daha fazla karakterden oluşan bir diziyle eşleştiği bir veya daha fazla yıldız işaretinden (*) oluşabilir.

- Joker karakter belirtilmemişse, yalnızca tam bir eşleşme bu eşleşme koşulunu karşılar.

    Örneğin, "presentation.ppt" belirtmek "presentation.ppt" adlı bir varlıkla eşleşir, ancak "presentation.pptx" adlı bir varlıkla eşleşmez.

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-literal"></a>URL Yolu Literal

Dosya adı da dahil olmak üzere bir isteğin URL yolunu belirtilen değerle karşılaştırır.

**Eşleşmeler**/**Eşleşmez** seçeneği, URL Yolu Literal eşleşme koşulunun hangi koşullar altında karşılandığını belirler.

- **Eşleşmeler**: Belirtilen desenle eşleşen bir URL yolu içerme isteği gerektirir.
- **Eşleşmiyor**: Belirtilen desenle eşleşmeyen bir URL yolu içerme isteği gerektirir.

Önemli bilgiler:

- URL karşılaştırma noktasının içerik erişim noktasından önce mi yoksa sonra mı başladığını belirtmek **için Göreli** seçeneğini kullanın. 

    **Göreli** seçeneği için aşağıdaki değerler kullanılabilir:
  - **Root**: URL karşılaştırma noktasının CDN ana bilgisayar adından hemen sonra başladığını gösterir.

    Örneğin: http:\//wpc.0001. &lt;etki&gt;/alanı**800001/myorigin/myfolder/index.htm**

  - **Başlangıç**: URL karşılaştırma noktasının içerik erişim noktasından sonra başladığını gösterir (örneğin, /000001 veya /800001/myorigin). \*.azureedge.net CNAME varsayılan olarak Verizon CDN ana bilgisayar adındaki kaynak dizine göre oluşturulduğundan, Azure CDN **kullanıcılarıNın Başlangıç** değerini kullanması gerekir. 

    Örneğin: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

  Bu URL aşağıdaki Verizon CDN hostname\/puan: http: /wpc.0001. &lt;etki&gt;alanı /800001/myorigin/**myfolder/index.htm**

- Kenar cname URL'si, URL karşılaştırması öncesinde CDN URL'ye yeniden yazılır.

Örneğin, aşağıdaki URL'lerin her ikisi de aynı varlığa işaret eder ve bu nedenle aynı URL yoluna sahiptir:

- CDN URL:\/http: /wpc.0001. &lt;etki&gt;alanı /800001/CustomerOrigin/path/asset.htm
- Kenar CNAME URL:\//&lt;http: bitiş noktası&gt;.azureedge.net/path/asset.htm

    Ek bilgiler:
    
    - URL yolu (kökgöreli): /800001/CustomerOrigin/path/asset.htm
   
    - URL yolu (menşee göre): /path/asset.htm

- URL'deki sorgu dizeleri yoksayılır.
- Büyük/küçük harf duyarlı bir karşılaştırma yapılıp yapılmadığını denetlemek için **Büyük/Küçük/Küçük Harf Seçeneğini Zedele** seçeneğini kullanın.
- Bu eşleşme koşulu için belirtilen değer, istemci tarafından yapılan tam isteğin göreli yolu ile karşılaştırılır.

- Belirli bir dizinde yapılan tüm istekleri eşleştirmek için [URL Yol Dizini'ni](#url-path-directory) veya [URL Path Wildcard](#url-path-wildcard) eşleşme koşulunu kullanın.

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-regex"></a>URL Yolu Regex

İsteğin URL yolunu belirtilen normal [ifadeyle](cdn-verizon-premium-rules-engine-reference.md#regular-expressions)karşılaştırır.

**Eşleşmeler**/**Eşleşmez** seçeneği, URL Path Regex eşleşme koşulunun hangi koşullar altında karşılandığını belirler.

- **Eşleşmeler**: Belirtilen normal ifadeyle eşleşen bir URL yolu içerme isteği gerektirir.
- **Eşleşmiyor**: Belirtilen normal ifadeyle eşleşmeyen bir URL yolu içerme isteği gerektirir.

Önemli bilgiler:

- Kenar cname URL'si, URL karşılaştırması öncesinde BIR CDN URL'sine yeniden yazılır.

    Örneğin, her iki URL de aynı varlığa işaret eder ve bu nedenle aynı URL yoluna sahiptir.

     - CDN URL:\/http: /wpc.0001. &lt;etki&gt;alanı /800001/CustomerOrigin/path/asset.htm

     - Kenar CNAME URL:\/http: /my.domain.com/path/asset.htm

    Ek bilgiler:
    
     - URL yolu: /800001/CustomerOrigin/path/asset.htm

- URL'deki sorgu dizeleri yoksayılır.
    
- Büyük/küçük harf duyarlı bir karşılaştırma yapılıp yapılmadığını denetlemek için **Büyük/Küçük/Küçük Harf Seçeneğini Zedele** seçeneğini kullanın.
    
- URL yolundaki boşluklar "%20" ile değiştirilmelidir.

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-wildcard"></a>URL Yolu Joker

İsteğin göreli URL yolunu belirtilen joker karakter deseniyle karşılaştırır.

**Eşleşmeler**/**Eşleşmez** seçeneği, URL Yolu Joker Karakter eşleşmesinin hangi koşullar altında karşılandığını belirler.

- **Eşleşmeler**: Belirtilen joker karakter desenine uyan bir URL yolu içerme isteği gerektirir.
- **Eşleşmiyor**: Belirtilen joker karakter desenine uymayan bir URL yolu içerme isteği gerektirir.

Önemli bilgiler:

- **Seçene göre:** Bu seçenek, URL karşılaştırma noktasının içerik erişim noktasından önce mi yoksa sonra mı başlayacağını belirler.

   Bu seçenek aşağıdaki değerlere sahip olabilir:
     - **Root**: URL karşılaştırma noktasının CDN ana bilgisayar adından hemen sonra başladığını gösterir.

       Örneğin: http:\//wpc.0001. &lt;etki&gt;/alanı**800001/myorigin/myfolder/index.htm**

     - **Başlangıç**: URL karşılaştırma noktasının içerik erişim noktasından sonra başladığını gösterir (örneğin, /000001 veya /800001/myorigin). \*.azureedge.net CNAME varsayılan olarak Verizon CDN ana bilgisayar adındaki kaynak dizine göre oluşturulduğundan, Azure CDN **kullanıcılarıNın Başlangıç** değerini kullanması gerekir. 

       Örneğin: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

     Bu URL aşağıdaki Verizon CDN hostname\/puan: http: /wpc.0001. &lt;etki&gt;alanı /800001/myorigin/**myfolder/index.htm**

- Kenar cname URL'si, URL karşılaştırması öncesinde BIR CDN URL'sine yeniden yazılır.

    Örneğin, aşağıdaki URL'lerin her ikisi de aynı varlığa işaret eder ve bu nedenle aynı URL yoluna sahiptir:
     - CDN URL: http://wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
     - Kenar CNAME URL:\//&lt;http: bitiş noktası&gt;.azureedge.net/path/asset.htm
    
    Ek bilgiler:
    
     - URL yolu (kökgöreli): /800001/CustomerOrigin/path/asset.htm
    
     - URL yolu (menşee göre): /path/asset.htm
    
- Her birini tek bir alanla sınırlandırarak birden çok URL yolunu belirtin.

   Örneğin: /marketing/asset.* /sales/*.htm

- URL'deki sorgu dizeleri yoksayılır.
    
- Büyük/küçük harf duyarlı bir karşılaştırma yapılıp yapılmadığını denetlemek için **Büyük/Küçük/Küçük Harf Seçeneğini Zedele** seçeneğini kullanın.
    
- URL yolundaki boşlukları "%20" ile değiştirin.
    
- URL yolu için belirtilen değer [joker karakter değerlerinden](cdn-verizon-premium-rules-engine-reference.md#wildcard-values)yararlanabilir. Her URL yolu deseni, her yıldız işaretinin bir veya daha fazla karakterden oluşan bir diziyle eşleşebileceği bir veya daha fazla yıldız işareti (*) içerebilir.

#### <a name="sample-scenarios"></a>Örnek Senaryolar

Aşağıdaki tablodaki örnek yapılandırmalar, bir istek belirtilen URL deseniyle eşleştiğinde bu eşleşme koşulunun karşılandığını varsayar:

Değer                   | Göreli    | Sonuç 
------------------------|----------------|-------
*/test.html */test.php  | Kök veya Köken | Bu desen, herhangi bir klasördeki "test.html" veya "test.php" adlı varlıklar için istekle eşleşir.
/80ABCD/origin/text/*   | Root           | İstenen varlık aşağıdaki ölçütleri karşıladığında bu desen eşleşir: <br />- "Menşe" adı verilen bir müşteri kaynağında bulunmalıdır. <br />- Göreli yol "metin" adlı bir klasörle başlamalıdır. Diğer bir deyişle, istenen varlık "metin" klasöründe veya özyinelemeli alt klasörlerinden birinde bulunabilir.
*/css/* */js/*          | Kök veya Köken | Bu desen, css veya js klasörü içeren tüm CDN veya kenar CNAME URL'leri ile eşleşir.
*.jpg *.gif *.png       | Kök veya Köken | Bu desen, .jpg, .gif veya .png ile biten tüm CDN veya kenar CNAME URL'leri ile eşleşir. Bu deseni belirtmenin alternatif bir [yolu, URL Yol Uzantısı eşleşme koşuluyla](#url-path-extension)dır.
/images/* /media/*      | Kaynak         | Bu desen, göreli yolu "görüntüler" veya "ortam" klasörüyle başlayan CDN veya kenar CNAME URL'leri ile eşleşir. <br />- CDN URL:\/http: /wpc.0001. &lt;etki&gt;alanı /800001/myorigin/images/sales/event1.png<br />- Örnek kenar CNAME\/URL: http: /cdn.mydomain.com/images/sales/event1.png

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-query-literal"></a>URL Sorgusu Literal

İsteğin sorgu dizesini belirtilen değerle karşılaştırır.

**Eşleşmeler**/**Eşleşmez** seçeneği, URL Sorgu Literal eşleşme koşulunun hangi koşullar altında karşılandığını belirler.

- **Eşleşmeler**: Belirtilen sorgu dizesine uyan bir URL sorgu dizesi içerme isteği gerektirir.
- **Eşleşmiyor**: Belirtilen sorgu dizesine uymayan bir URL sorgu dizesi içerme isteği gerektirir.

Önemli bilgiler:

- Yalnızca tam sorgu dize eşleşmeleri bu eşleşme koşulunu karşılar.
    
- Sorgu dize karşılaştırmalarının büyük/küçük harf duyarlılığını denetlemek için **Büyük/Küçük Harf Yoksay** seçeneğini kullanın.
    
- Sorgu dizesi değer metnine satır aralığı (?) eklemeyin.
    
- Bazı karakterler URL kodlaması gerektirir. URL aşağıdaki karakterleri kodlamak için yüzde simgesini kullanın:

   Karakter | URL Kodlama
   ----------|---------
   Alan     | %20
   &         | %25

- Önbellek ayarlarının izlenme biçimi nedeniyle, bu eşleşme koşulu aşağıdaki özelliklerle uyumsuzdur:
   - Önbellek Dolgusu Tamamlandı
   - Varsayılan Dahili Max-Age
   - Kuvvet İç Max-Age
   - Kaynak Yokyokla No-Önbellek
   - Dahili Max-Bayat

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-query-parameter"></a>URL Sorgu Parametresi

Belirtilen sorgu dize parametresini içeren istekleri tanımlar. Bu parametre, belirtilen desenle eşleşen bir değere ayarlanır. İstek URL'sinde sorgu dize parametreleri (örneğin, parametre=değer) bu koşulun karşılanıp karşılanmadığını belirler. Bu eşleme koşulu, bir sorgu dize parametresini adına göre tanımlar ve parametre değeri için bir veya daha fazla değer kabul eder. 

**Eşleşmeler**/**Eşleşmez** seçeneği, URL Sorgu Parametresi eşleşme koşulunun hangi koşullar altında karşılandığını belirler.

- **Eşleşmeler**: Bu eşleşme koşulunda tanımlanan değerlerden en az biriyle eşleşen bir değere sahip belirtilen parametreyi içeren bir istek gerektirir.
- **Eşleşmez**: İsteğin aşağıdaki ölçütlerden birini karşılamasını gerektirir:
  - Belirtilen parametreyi içermez.
  - Belirtilen parametreyi içerir, ancak değeri bu eşleşme koşulunda tanımlanan değerlerin hiçbirinde eşleşmez.

Bu eşleşme koşulu, parametre adı/değer birleşimlerini belirtmek için kolay bir yol sağlar. Sorgu dize parametresi ile eşleşiyorsanız daha fazla esneklik için [URL Sorgu Joker Karakter](#url-query-wildcard) koşulunu kullanmayı düşünün.

Önemli bilgiler:

- Bu eşleşme koşulunun örneğine göre yalnızca tek bir URL sorgu parametresi adı belirtilebilir.
    
- Bir parametre adı belirtildiğinde joker karakter değerleri desteklenmedığından, karşılaştırma için yalnızca tam parametre adı eşleşmeleri uygundur.
- Parametre değeri(ler) [joker karakter değerlerini](cdn-verizon-premium-rules-engine-reference.md#wildcard-values)içerebilir.
   - Her parametre değer deseni, her yıldız işaretinin bir veya daha fazla karakterden oluşan bir diziyle eşleşebileceği bir veya daha fazla yıldız (*) olabilir.
   - Bazı karakterler URL kodlaması gerektirir. URL aşağıdaki karakterleri kodlamak için yüzde simgesini kullanın:

       Karakter | URL Kodlama
       ----------|---------
       Alan     | %20
       &         | %25

- Her birini tek bir boşlukla sınırlandırarak birden çok sorgu dizesi parametre değerlerini belirtin. Bu eşleşme koşulu, bir istek belirtilen ad/değer birleşimlerinden birini içerdiğinde karşılanır.

   - Örnek 1:

     - Yapılandırma:

       ValueA ValueB

     - Bu yapılandırma aşağıdaki sorgu dize parametreleri eşleşir:

       Parametre1=ValueA
    
       Parametre1=ValueB

   - Örnek 2:

     - Yapılandırma: 

        Değer%20A Değer%20B

     - Bu yapılandırma aşağıdaki sorgu dize parametreleri eşleşir:

       Parametre1=Değer%20A

       Parametre1=Değer%20B

- Bu eşleşme koşulu yalnızca belirtilen sorgu dize adı/değer birleşimlerinden en az biriyle tam eşleşme olduğunda karşılanır.

   Örneğin, önceki örnekte yapılandırmayı kullanırsanız, "Parameter1=ValueAdd" parametre adı/değer birleşimi eşleşme olarak kabul edilmez. Ancak, aşağıdaki değerlerden birini belirtirseniz, bu ad/değer birleşimi eşleşir:

   - ValueA ValueB ValueAdd
   - ValueA* ValueB

- Sorgu dize karşılaştırmalarının büyük/küçük harf duyarlılığını denetlemek için **Büyük/Küçük Harf Yoksay** seçeneğini kullanın.
    
- Önbellek ayarlarının izlenme biçimi nedeniyle, bu eşleşme koşulu aşağıdaki özelliklerle uyumsuzdur:
   - Önbellek Dolgusu Tamamlandı
   - Varsayılan Dahili Max-Age
   - Kuvvet İç Max-Age
   - Kaynak Yokyokla No-Önbellek
   - Dahili Max-Bayat

#### <a name="sample-scenarios"></a>Örnek senaryolar

Aşağıdaki örnek, bu seçeneğin belirli durumlarda nasıl çalıştığını gösterir:

Adı  | Değer |  Sonuç
------|-------|--------
Kullanıcı  | Abdullah   | İstenen BIR URL için sorgu dizesi "?user=joe" olduğunda bu desen eşleşir.
Kullanıcı  | *     | İstenen BIR URL'nin sorgu dizesi Kullanıcı parametresi içeriyorsa, bu desen eşleşir.
Email | Abdullah\* | İstenen BIR URL için sorgu dizesi "Joe" ile başlayan bir E-posta parametresi içerdiğinde bu desen eşleşir.

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-query-regex"></a>URL Sorgusu Regex

Belirtilen sorgu dize parametresini içeren istekleri tanımlar. Bu parametre, belirtilen [normal ifadeyle](cdn-verizon-premium-rules-engine-reference.md#regular-expressions)eşleşen bir değere ayarlanır.

**Eşleşmeler**/**Eşleşmez** seçeneği, URL Query Regex eşleşme koşulunun hangi koşullar altında karşılandığını belirler.

- **Eşleşmeler**: Belirtilen normal ifadeyle eşleşen bir URL sorgu dizesi içerme isteği gerektirir.
- **Eşleşmiyor**: Belirtilen normal ifadeyle eşleşmeyen bir URL sorgu dizesi içerme isteği gerektirir.

Önemli bilgiler:

- Yalnızca belirtilen normal ifadeyle tam eşleşmeler bu eşleşme koşulunu karşılar.
    
- Sorgu dize karşılaştırmalarının büyük/küçük harf duyarlılığını denetlemek için **Büyük/Küçük Harf Yoksay** seçeneğini kullanın.
    
- Bu seçeneğin amaçları için, sorgu dizesi sorgu dizesi için soru işareti (?) delimiter sonra ilk karakter ile bir sorgu dizesi başlar.
    
- Bazı karakterler URL kodlaması gerektirir. URL aşağıdaki karakterleri kodlamak için yüzde simgesini kullanın:

   Karakter | URL Kodlama | Değer
   ----------|--------------|------
   Alan     | %20          | \%20
   &         | %25          | \%25

   Yüzde sembollerinin kaçması gerektiğini unutmayın.

- Normal ifadeye bir ters eğik \^çizgi eklemek için çift kaçış özel normal ifade karakterleri (örneğin, $.+).

   Örnek:

   Değer | Olarak yorumlanır 
   ------|---------------
   \\+    | +
   \\\\+   | \\+

- Önbellek ayarlarının izlenme biçimi nedeniyle, bu eşleşme koşulu aşağıdaki özelliklerle uyumsuzdur:
   - Önbellek Dolgusu Tamamlandı
   - Varsayılan Dahili Max-Age
   - Kuvvet İç Max-Age
   - Kaynak Yokyokla No-Önbellek
   - Dahili Max-Bayat

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-query-wildcard"></a>URL Sorgusu Joker

Belirtilen değer(ler) isteğinin sorgu dizesi ile karşılaştırır.

**Eşleşmeler**/**Eşleşmez** seçeneği, URL Sorgu Joker Karakter koşulunun hangi koşullar altında karşılandığını belirler.

- **Eşleşmeler**: Belirtilen joker karakter değeriyle eşleşen bir URL sorgu dizesi içerme isteği gerektirir.
- **Eşleşmiyor**: Belirtilen joker karakter değeriyle eşleşmeyen bir URL sorgu dizesi içerme isteği gerektirir.

Önemli bilgiler:

- Bu seçeneğin amaçları için, sorgu dizesi sorgu dizesi için soru işareti (?) delimiter sonra ilk karakter ile bir sorgu dizesi başlar.
- Parametre değerleri [joker karakter değerlerini](cdn-verizon-premium-rules-engine-reference.md#wildcard-values)içerebilir:
   - Her parametre değer deseni, her yıldız işaretinin bir veya daha fazla karakterden oluşan bir diziyle eşleşebileceği bir veya daha fazla yıldız (*) olabilir.
   - Bazı karakterler URL kodlaması gerektirir. URL aşağıdaki karakterleri kodlamak için yüzde simgesini kullanın:

     Karakter | URL Kodlama
     ----------|---------
     Alan     | %20
     &         | %25

- Her birini tek bir boşlukla delerek birden çok değer belirtin.

   Örneğin: *Parametre1=ValueA ValueB* *ValueB* *Parametres1=ValueC&Parametresi2=ValueD*

- Yalnızca belirtilen sorgu dize desenlerinden en az biriyle tam eşleşmeler bu eşleşme koşulunu karşılar.
    
- Sorgu dize karşılaştırmalarının büyük/küçük harf duyarlılığını denetlemek için **Büyük/Küçük Harf Yoksay** seçeneğini kullanın.
    
- Önbellek ayarlarının izlenme biçimi nedeniyle, bu eşleşme koşulu aşağıdaki özelliklerle uyumsuzdur:
   - Önbellek Dolgusu Tamamlandı
   - Varsayılan Dahili Max-Age
   - Kuvvet İç Max-Age
   - Kaynak Yokyokla No-Önbellek
   - Dahili Max-Bayat

#### <a name="sample-scenarios"></a>Örnek senaryolar

Aşağıdaki örnek, bu seçeneğin belirli durumlarda nasıl çalıştığını gösterir:

 Adı                 | Açıklama
 ---------------------|------------
kullanıcı=joe              | İstenen BIR URL için sorgu dizesi "?user=joe" olduğunda bu desen eşleşir.
\*kullanıcı=\* \*optout=\* | BU desen, CDN URL sorgusu kullanıcı veya devre dışı bırakma parametresi içerdiğinde eşleşir.

[Başa dön](#reference-for-rules-engine-match-conditions)

</br>

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İçerik Dağıtım Ağı'na genel bakış](cdn-overview.md)
- [Kural altyapısı başvurusu](cdn-verizon-premium-rules-engine-reference.md)
- [Kural altyapısı koşullu ifadeleri](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Kural altyapısı özellikleri](cdn-verizon-premium-rules-engine-reference-features.md)
- [Kurallar altyapısını kullanarak varsayılan HTTP davranışını geçersiz kılma](cdn-verizon-premium-rules-engine.md)
