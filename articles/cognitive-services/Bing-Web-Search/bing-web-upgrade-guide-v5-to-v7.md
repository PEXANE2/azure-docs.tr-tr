---
title: API v5'ten v7'ye yükseltme - Bing Web Arama API'si
titleSuffix: Azure Cognitive Services
description: Uygulamanızın Bing Web Arama v7 API'lerini kullanmak için güncelleştirmeler gerektirdiğini belirleyin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: E8827BEB-4379-47CE-B67B-6C81AD7DAEB1
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: scottwhi
ms.openlocfilehash: 2133cd59c524112ae8a77c0a20cbce1d1336a38d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68881315"
---
# <a name="upgrade-from-bing-web-search-api-v5-to-v7"></a>Bing Web Arama API v5'ten v7'ye yükseltme

Bu yükseltme kılavuzu, Bing Web Arama API sürümü 5 ve sürüm 7 arasındaki değişiklikleri tanımlar. Sürüm 7'yi kullanmak üzere güncelleştirmeniz gereken uygulamanızın bölümlerini belirlemenize yardımcı olmak için bu kılavuzu kullanın.

## <a name="breaking-changes"></a>Yeni değişiklikler

### <a name="endpoints"></a>Uç Noktalar

- Bitiş noktasının sürüm numarası v5'ten v7'ye değiştirildi. Örneğin, https:\/\/api.cognitive.microsoft.com/bing/**v7.0**/search.

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
|Yetersiz Yetkilendirme|YetkilendirmeDevre Dışı<br/>Yetkilendirme Süresi Doldu|Arayan kaynağa erişmek için izinleri yoksa Bing Yetersiz Yetkilendirme döndürür. Abonelik anahtarı devre dışı bırakılmışsa veya süresi dolmuşsa, bu hata oluşabilir. <br/><br/>Hata Yetersiz Yetkilendirme ise, HTTP durum kodu 403'dür.

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


## <a name="non-breaking-changes"></a>Kırılmayan değişiklikler  

### <a name="headers"></a>Üst bilgiler

- İsteğe bağlı [Pragma](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#pragma) istek üstbilgisi eklendi. Varsayılan olarak, Bing önbelleğe alınmış içeriği (varsa) döndürür. Bing'in önbelleğe alınmış içeriği döndürmesini önlemek için, Pragma üst bilgisini no-cache olarak ayarlayın (örneğin, Pragma: no-cache).

### <a name="query-parameters"></a>Sorgu parametreleri

- [AnswerCount](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#answercount) sorgu parametresi eklendi. Yanıtın dahil etmesini istediğiniz yanıt sayısını belirtmek için bu parametreyi kullanın. Cevaplar sıralamaya göre seçilir. Örneğin, bu parametreyi üçe (3) ayarlarsanız, yanıt ilk üç sırada yer alan yanıtları içerir.  

- Sorgu yu [parametresi eklendi.](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#promote) Sıralamalarından bağımsız olarak, bir veya daha fazla yanıt türünü açıkça eklemek için bu parametreyi birlikte `answerCount` kullanın. Örneğin, video ve görüntüleri yanıta tanıtmak için, *videoları, görüntüleri*için teşvik ayarlamak istiyorum. Tanıtmak istediğiniz yanıtlistesi sınıra `answerCount` dahil edilmez. Örneğin, 2 `answerCount` ise `promote` ve *videolar, görüntüler*olarak ayarlanmışsa, yanıt web sayfaları, haberler, videolar ve görüntüler içerebilir.

### <a name="object-changes"></a>Nesne değişiklikleri

- `someResultsRemoved` [Alanı WebYanıt](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#webanswer) nesnesine ekledi. Alan, yanıtın web yanıtından bazı sonuçlar dışlayıp dışlanmadığını gösteren bir Boolean değeri içerir.  
