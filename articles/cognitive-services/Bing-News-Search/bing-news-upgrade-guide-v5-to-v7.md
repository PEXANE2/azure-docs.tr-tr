---
title: Bing Haber Arama API v5'i v7'ye yükseltin
titleSuffix: Azure Cognitive Services
description: Sürüm 7'yi kullanmak için uygulamanızın güncelleştirmeniz gereken bölümlerini tanımlar.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: scottwhi
ms.openlocfilehash: bad0ef849af7c94e63f1dfbebda7f47caef9947d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294377"
---
# <a name="news-search-api-upgrade-guide"></a>Haber Arama API yükseltme kılavuzu

Bu yükseltme kılavuzu, Bing Haber Arama API'sının sürüm 5 ve sürüm 7 arasındaki değişiklikleri tanımlar. Sürüm 7'yi kullanmak üzere güncelleştirmeniz gereken uygulamanızın bölümlerini belirlemenize yardımcı olmak için bu kılavuzu kullanın.

## <a name="breaking-changes"></a>Yeni değişiklikler

### <a name="endpoints"></a>Uç Noktalar

- Bitiş noktasının sürüm numarası v5'ten v7'ye değiştirildi. Örneğin, `https://api.cognitive.microsoft.com/bing/v7.0/news/search`.

### <a name="error-response-objects-and-error-codes"></a>Hata yanıt nesneleri ve hata kodları

- Tüm başarısız istekleri artık `ErrorResponse` yanıt gövdesinde bir nesne içermelidir.

- Nesneye `Error` aşağıdaki alanlar eklendi.  
  - `subCode`&mdash;Mümkünse hata kodunu ayrık kovalara bölümler
  - `moreDetails`&mdash;`message` Alanda açıklanan hata hakkında ek bilgi

- V5 hata kodlarını aşağıdaki olası `code` `subCode` ve değerlerle değiştirdi.

|Kod|Subcode|Açıklama
|-|-|-
|SunucuHatası|Beklenmeyen Hata<br/>Kaynak Hatası<br/>Uygulanmadı|Bing, alt kod koşullarından herhangi biri oluştuğunda ServerError'ı döndürür. HTTP durum kodu 500 ise yanıt bu hataları içerir.
|Geçersizİstek|ParametreEksik<br/>ParametreGeçersiz Değer<br/>httpİzin verilen<br/>Engellendi|Bing, isteğin herhangi bir bölümü geçerli olmadığında Geçersiz İstek'i döndürür. Örneğin, gerekli bir parametre eksik veya bir parametre değeri geçerli değil.<br/><br/>Hata ParameterMissing veya ParameterGeçersizDeğer ise, HTTP durum kodu 400'dür.<br/><br/>Hata httpNotAllowed ise, HTTP durum kodu 410.
|RateLimitAşıldı||Bing, sorgularınızı saniyede (QPS) veya aylık sorgularınızı (QPM) kotanızı aştığınızda RateLimitExceeded'i döndürür.<br/><br/>QPS'yi aştıysanız Bing, HTTP durum kodu 429'u ve QPM'yi aştıysanız 403'ü döndürür.
|Geçersiz Yetkilendirme|Yetkilendirme Eksik<br/>YetkilendirmeRedundancy|Bing, arayan kişinin kimliğini doğrulayamıyorsa Geçersiz Yetkilendirme'yi döndürür. Örneğin, `Ocp-Apim-Subscription-Key` üstbilgi eksik veya abonelik anahtarı geçerli değil.<br/><br/>Birden fazla kimlik doğrulama yöntemi belirtirseniz artıklık oluşur.<br/><br/>Hata Geçersiz Yetkilendirme ise, HTTP durum kodu 401'dir.
|Yetersiz Yetkilendirme|YetkilendirmeDevre Dışı<br/>Yetkilendirme Süresi Doldu|Arayan kaynağa erişmek için izinleri yoksa Bing Yetersiz Yetkilendirme döndürür. Bu, abonelik anahtarı devre dışı bırakılmışsa veya süresi dolmuşsa oluşabilir. <br/><br/>Hata Yetersiz Yetkilendirme ise, HTTP durum kodu 403'dür.

- Aşağıdaki önceki hata kodlarını yeni kodlara eşler. v5 hata kodlarına bağımlı bir şekilde ele geçirdiyseniz, kodunuzu buna göre güncelleştirin.

|Sürüm 5 kodu|Sürüm 7 code.subCode
|-|-
|İstekParametreEksik|Geçersizİstek.ParametreEksik
İstekParametreGeçersiz Değer|Geçersizİstek.ParametreGeçersiz Değer
Kaynak Erişim Reddedildi|Yetersiz Yetkilendirme
Aşan Hacim|RateLimitAşıldı
AşıldıQpsLimit|RateLimitAşıldı
Devre dışı|Yetersiz Yetkilendirme.YetkilendirmeDevre Dışı
Beklenmeyen Hata|ServerError.unexpectedError
DataSourceErrors|ServerError.ResourceError
Yetkilendirme Eksik|Geçersiz Yetkilendirme.YetkilendirmeEksik
httpİzin verilen|Geçersizİstek.HttpNotAllowed
UserAgentEksik|Geçersizİstek.ParametreEksik
Uygulanmadı|ServerError.NotImplemented
Geçersiz Yetkilendirme|Geçersiz Yetkilendirme
Geçersiz Yetkilendirme Yöntemi|Geçersiz Yetkilendirme
Çoklu Yetkilendirme Yöntemi|GeçersizYetkilendirme.YetkilendirmeReduncy
Süresi Dolmuş YetkilendirmeToken|Yetersiz Yetkilendirme.Yetkilendirme Süresi Doldu
Yetersiz Kapsam|Yetersiz Yetkilendirme
Engellendi|Geçersizİstek.Engellendi

### <a name="object-changes"></a>Nesne değişiklikleri

- `contractualRules` Alanı [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#newsarticle) nesnesine ekledi. Alan, `contractualRules` izlemeniz gereken kuralların bir listesini içerir (örneğin, makale atıfı). Kullanmak `contractualRules` yerine sağlanan atıf `provider`uygulamalısınız. Makale, `contractualRules` yalnızca [Web Arama API](../bing-web-search/search-the-web.md) yanıtı bir Haber yanıtı içeriyorsa içerir.

## <a name="non-breaking-changes"></a>Kırılmayan Değişiklikler

### <a name="query-parameters"></a>Sorgu parametreleri

- [Kategori](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#category) sorgu parametresini ayarlayabileceğiniz olası bir değer olarak Ürünler eklendi. [Pazarlara Göre Kategorilere](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference)Bakın.

- Tarihe göre sıralanmış en son ilk ile sıralanmış popüler konuları döndüren [SortBy](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#sortby) sorgu parametresi eklendi.

- Belirtilen Unix dönem zaman damgası üzerinde veya sonrasında Bing tarafından keşfedilen popüler konuları döndüren [Since](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#since) sorgu parametresi eklendi.

### <a name="object-changes"></a>Nesne değişiklikleri

- `mentions` Alanı [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#newsarticle) nesnesine ekledi. Alan, `mentions` makalede bulunan varlıkların (kişiler veya yerlerin) bir listesini içerir.

- `video` Alanı [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#newsarticle) nesnesine ekledi. Alan, `video` haber makalesiyle ilgili bir video içeriyor. Video, gömebileceğiniz \<bir\> iframe veya bir hareket küçük resmidir.

- `sort` Alan [Haber](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#news) nesnesine eklendi. Alan `sort` makalelerin sıralama sırasını gösterir. Örneğin, makaleler alaka düzeyine (varsayılan) veya tarihe göre sıralanır.

- Sıralama sırasını tanımlayan [SortValue](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#sortvalue) nesnesi eklendi. Alan, `isSelected` yanıtın sıralama sırasını kullanıp kullanmadığını gösterir. **Doğruysa,** yanıt sıralama sırasını kullandı. `isSelected` **Yanlışsa,** alandaki `url` URL'yi farklı bir sıralama sırası istemek için kullanabilirsiniz.
