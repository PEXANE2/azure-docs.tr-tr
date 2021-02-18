---
title: Azure ön kapısının Standart/Premium kural kümesi eylemlerini yapılandırma
description: Bu makale, Azure ön kapısının ayarlandığı çeşitli eylemlerin bir listesini sağlar.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: conceptual
ms.date: 02/18/2021
ms.author: yuajia
ms.openlocfilehash: c9995df0f292c5e528156a3280df5484db017fca
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "101100518"
---
# <a name="azure-front-door-standardpremium-rule-set-actions"></a>Azure ön kapı Standart/Premium kural kümesi eylemleri

> [!Note]
> Bu belge, Azure ön kapısının Standart/Premium (Önizleme) içindir. Azure ön kapısı hakkında bilgi mi arıyorsunuz? [Burada](../front-door-overview.md)görüntüleyin.

Bir Azure ön kapısının [kümesi](concept-rule-set.md) , eşleşen koşulların ve eylemlerin birleşiminden oluşan kurallardan oluşur. Bu makalede, bir kural kümesinde kullanabileceğiniz eylemlerin ayrıntılı bir açıklaması sağlanmaktadır. Eylem, eşleşme koşulunun tanımladığı bir istek türüne uygulanan davranışı tanımlar. Bir Azure ön kapısı kural kümesinde, bir kural en fazla beş eylem içerebilir. Sunucu değişkeni tüm eylemlerde desteklenir.

> [!IMPORTANT]
> Azure ön kapı Standart/Premium (Önizleme) Şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Aşağıdaki eylemler Azure ön kapı kural kümesi 'nde kullanılabilir.  

## <a name="cache-expiration"></a>Önbellek süre sonu

Kuralların eşleştiği talepler için uç noktanın yaşam süresi (TTL) değerinin üzerine yazmak için bu eylemi kullanın.

### <a name="required-fields"></a>Gerekli alanlar

Aşağıdaki açıklama, bu önbellek davranışları seçilirken ve kural eşleştiğinde geçerlidir:

Önbellek davranışı |  Description              
---------------|----------------
Atlama önbelleği | İçerik önbelleğe alınmamış.
Geçersiz kıl | Kaynaktan döndürülen TTL değeri, eylemde belirtilen değerle üzerine yazılır. Bu davranış yalnızca yanıtın önbelleklenebilir olması durumunda uygulanır. "No-Cache", "Private", "No-Store" değerlerine sahip Cache-Control yanıt üst bilgisi için, eylem geçerli olmayacaktır.
Eksikse ayarla | Kaynağından bir TTL değeri döndürülmezse, kural TTL 'yi eylemde belirtilen değere ayarlar. Bu davranış yalnızca yanıtın önbelleklenebilir olması durumunda uygulanır. "No-Cache", "Private", "No-Store" değerlerine sahip Cache-Control yanıt üst bilgisi için, eylem geçerli olmayacaktır.

### <a name="additional-fields"></a>Ek alanlar

Gün | Saat | Dakika | Saniye
-----|-------|---------|--------
int | int | int | int 

## <a name="cache-key-query-string"></a>Önbellek anahtarı sorgu dizesi

Sorgu dizelerine göre önbellek anahtarını değiştirmek için bu eylemi kullanın.

### <a name="required-fields"></a>Gerekli alanlar

Aşağıdaki açıklama, bu davranışlar seçilirken ve kural eşleştiğinde geçerlidir:

Davranış | Description
---------|------------
Şunları Dahil Et: | Parametrelerde belirtilen sorgu dizeleri, önbellek anahtarı oluşturulduğunda dahil edilir. 
Her benzersiz URL'yi önbelleğe al | Her benzersiz URL 'nin kendi önbellek anahtarı vardır. 
Exclude | Parametrelerde belirtilen sorgu dizeleri, önbellek anahtarı oluşturulduğunda hariç tutulur.
Sorgu dizelerini yoksay | Önbellek anahtarı oluşturulduğunda sorgu dizeleri dikkate alınmazlar. 

## <a name="modify-request-header"></a>İstek üst bilgisini Değiştir

Bu eylemi, kaynağına gönderilen isteklerde bulunan üst bilgileri değiştirmek için kullanın.

### <a name="required-fields"></a>Gerekli alanlar

Aşağıdaki açıklama, bu eylemler seçilirken ve kural eşleştiğinde geçerlidir:

Eylem | HTTP üst bilgi adı | Değer
-------|------------------|------
Ekle | **Üst bilgi adı** 'nda belirtilen üstbilgi, belirtilen değere sahip olan isteğe eklenmiş. Üst bilgi zaten mevcutsa, değer mevcut değere eklenir. | Dize
Üzerine yaz | **Üst bilgi adı** 'nda belirtilen üstbilgi, belirtilen değere sahip olan isteğe eklenmiş. Üst bilgi zaten mevcutsa, belirtilen değer varolan değerin üzerine yazar. | Dize
Sil | Kuralda belirtilen üst bilgi varsa, üst bilgi istekten silinir. | Dize

## <a name="modify-response-header"></a>Yanıt üst bilgisini Değiştir

İstemcilerinize döndürülen yanıtlarda bulunan üstbilgileri değiştirmek için bu eylemi kullanın.

### <a name="required-fields"></a>Gerekli alanlar

Aşağıdaki açıklama, bu eylemler seçilirken ve kural eşleştiğinde geçerlidir:

Eylem | HTTP üst bilgi adı | Değer
-------|------------------|------
Ekle | **Üst bilgi adı** 'nda belirtilen üstbilgi, belirtilen **değer** kullanılarak yanıta eklenir. Üst bilgi zaten mevcutsa, **değer** var olan değere eklenir. | Dize
Üzerine yaz | **Üst bilgi adı** 'nda belirtilen üstbilgi, belirtilen **değer** kullanılarak yanıta eklenir. Üst bilgi zaten mevcutsa, **değer** varolan değerin üzerine yazar. | Dize
Sil | Kuralda belirtilen üst bilgi varsa, üst bilgi yanıttan silinir. | Dize

## <a name="url-redirect"></a>URL yeniden yönlendirme

İstemcileri yeni bir URL 'ye yönlendirmek için bu eylemi kullanın. 

### <a name="required-fields"></a>Gerekli alanlar

Alan | Açıklama 
------|------------
Yeniden yönlendirme türü | İstek sahibine döndürülecek yanıt türünü seçin: bulunan (302), taşınan (301), geçici yeniden yönlendirme (307) ve kalıcı yeniden yönlendirme (308).
Yeniden yönlendirme protokolü | Match Isteği, HTTP, HTTPS.
Hedef konak | İsteğin yeniden yönlendirilmesini istediğiniz ana bilgisayar adını seçin. Gelen ana bilgisayarı korumak için boş bırakın.
Hedef yol | Yeniden Yönlendirmede kullanılacak yolu tanımlayın. Gelen yolu korumak için boş bırakın.  
Sorgu dizesi | Yeniden Yönlendirmede kullanılan sorgu dizesini tanımlayın. Gelen sorgu dizesini korumak için boş bırakın. 
Hedef parça | Yeniden Yönlendirmede kullanılacak parçayı tanımlayın. Gelen parçayı korumak için boş bırakın. 

## <a name="url-rewrite"></a>URL yeniden yazma

Bu eylemi, kaynağına yönlendiren bir isteğin yolunu yeniden yazmak için kullanın.

### <a name="required-fields"></a>Gerekli alanlar

Alan | Açıklama 
------|------------
Kaynak stili | Değiştirilecek URL yolundaki kaynak modelini tanımlayın. Şu anda, kaynak stili önek tabanlı eşleşme kullanıyor. Tüm URL yollarını eşleştirmek için, **/** kaynak model değeri olarak bir eğik çizgi () kullanın.
Hedef | Yeniden yazma sırasında kullanılacak hedef yolu tanımlayın. Hedef yol, kaynak deseninin üzerine yazar.
Eşleşmeyen yolu koru | **Evet** olarak ayarlanırsa, kaynak örüntüden sonraki kalan yol yeni hedef yoluna eklenir. 

## <a name="server-variable"></a>Sunucu değişkeni

### <a name="supported-variables"></a>Desteklenen değişkenler

| Değişken adı | Description                                                  |
| -------------------------- | :----------------------------------------------------------- |
| socket_ip                  | Azure ön kapısının doğrudan bağlantısının IP adresi. İstemci isteği göndermek için bir HTTP proxy veya yük dengeleyici kullandıysanız, Socketıp değeri proxy veya yük dengeleyicinin IP adresidir. |
| client_ip                  | Özgün isteği yapan istemcinin IP adresi. İstekte bir X-Iletilmiş-for üstbilgisi varsa, Istemci IP 'si aynı üzerinden çekilir. |
| client_port                | İsteği yapan istemcinin IP bağlantı noktası. |
| Konak                      | İstemciden gelen istekteki ana bilgisayar adı. |
| geo_country                     | İstek sahibinin ülke/bölge kodu üzerinden kaynak ülkesini/bölgesini gösterir. |
| http_method                | URL isteğini yapmak için kullanılan yöntem. Örneğin, GET veya POST. |
| http_version               | İstek Protokolü. Genellikle HTTP/1.0, HTTP/1.1 veya HTTP/2.0. |
| query_string               | İstenen URL 'de "?" öğesini izleyen değişken/değer çiftleri listesi. Örnek: istekte *http://contoso.com:8080/article.aspx?id=123&title=fabrikam* query_string değer *ID = 123&title = fabrikam* olacaktır |
| request_scheme             | İstek düzeni: http veya https. |
| request_uri                | Tüm özgün istek URI 'SI (bağımsız değişkenlerle). Örnek: istekte *http://contoso.com:8080/article.aspx?id=123&title=fabrikam* request_uri değeri */article.aspx olacak mı? ID = 123&title = fabrikam* |
| server_port                | Bir isteği kabul eden sunucunun bağlantı noktası. |
| ssl_protocol    | Kurulan bir TLS bağlantısının protokolü. |
| url_path                   | Web istemcisinin erişmek istediği konaktaki belirli kaynağı tanımlar. Bu, istek URI 'sinin bağımsız değişkenler olmadan bölümüdür. Örnek: istekte *http://contoso.com:8080/article.aspx?id=123&title=fabrikam* uri_path değer */article.aspx* olacaktır |

### <a name="server-variable-format"></a>Sunucu değişken biçimi    

**Biçim:** {değişken: konum}, {değişken: konum: uzunluk}, {değişken}

### <a name="supported-server-variable-actions"></a>Desteklenen sunucu değişkeni eylemleri

* İstek üst bilgisi
* Yanıt üst bilgisi
* Önbellek anahtarı sorgu dizesi
* URL yeniden yazma
* URL yeniden yönlendirme

## <a name="next-steps"></a>Sonraki adımlar

* [Azure ön kapı stanard/Premium kural kümesi](concept-rule-set.md)hakkında daha fazla bilgi edinin.
* [Kural kümesi eşleştirme koşulları](concept-rule-set-match-conditions.md)hakkında daha fazla bilgi edinin.
