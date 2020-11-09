---
title: Bing Video Arama API'si v5 'i v7 'ye yükseltme
titleSuffix: Azure Cognitive Services
description: Sürüm 7 ' yi kullanmak için güncelleştirmeniz gereken Bing Video Arama uygulamanızın parçalarını tanımlar.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: scottwhi
ms.openlocfilehash: 392bb46d481df1f872b9a61beffed9e5b46f4dc5
ms.sourcegitcommit: 8a1ba1ebc76635b643b6634cc64e137f74a1e4da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2020
ms.locfileid: "94379846"
---
# <a name="video-search-api-upgrade-guide"></a>Video Arama API Yükseltme Kılavuzu

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](https://aka.ms/cogsvcs/bingmove)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](https://aka.ms/cogsvcs/bingmigration).

Bu yükseltme Kılavuzu, sürüm 5 ve Bing Video Arama API'si sürüm 7 arasındaki değişiklikleri tanımlar. Uygulamanızın 7 sürümünü kullanmak için güncelleştirmeniz gereken parçalarını belirlemenize yardımcı olması için bu kılavuzu kullanın.

## <a name="breaking-changes"></a>Yeni değişiklikler

### <a name="endpoints"></a>Uç Noktalar

- Uç noktanın sürüm numarası, V5 'ten v7 'e değişti. Örneğin, `https://api.cognitive.microsoft.com/bing/v7.0/videos/search`.

### <a name="error-response-objects-and-error-codes"></a>Hata yanıtı nesneleri ve hata kodları

- Tüm başarısız istekler `ErrorResponse` Yanıt gövdesine bir nesne içermelidir.

- Nesnesine aşağıdaki alanlar eklendi `Error` .  
  - `subCode`&mdash;Mümkünse hata kodunu farklı demetlere göre bölümler
  - `moreDetails`&mdash;Alanda açıklanan hata hakkında ek bilgiler `message`
   

- V5 hata kodları aşağıdaki olası `code` ve `subCode` değerlerle değiştirilmiştir.

|Kod|Alt|Açıklama
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>NotImplemented|Bing, alt kod koşullarından herhangi biri gerçekleştiğinde ServerError döndürür. HTTP durum kodu 500 ise yanıt bu hataları içerir.
|Invalidrequest|ParameterMissing<br/>Parameterınvalidvalue<br/>HttpNotAllowed<br/>Engellendi|İsteğin herhangi bir bölümü geçerli değilse Bing, ınvalidrequest döndürüyor. Örneğin, gerekli bir parametre eksik veya bir parametre değeri geçerli değil.<br/><br/>Hata ParameterMissing veya Parameterınvalidvalue ise, HTTP durum kodu 400 ' dir.<br/><br/>Hataya HttpNotAllowed varsa, HTTP durum kodu 410 ' dir.
|Ratelimitexcebaşında||Bing, sorgu/saniye (QPS) veya aylık sorgu (QPM) kotası her aşışınızda Ratelimitexceden başına döndürür.<br/><br/>QPM 'yi aştıysanız Bing, QPS ve 403 değerini aştıysanız, 429 HTTP durum kodunu döndürür.
|Invalidauthorleştirme|AuthorizationMissing<br/>Authorizationartıklık|Bing çağıranın kimliğini doğrulayamayan Bing, ınvalidauthortıcıyla geri döndürür. Örneğin, `Ocp-Apim-Subscription-Key` üst bilgi eksik veya abonelik anahtarı geçerli değil.<br/><br/>Birden fazla kimlik doğrulama yöntemi belirtirseniz artıklık oluşur.<br/><br/>Hata eksik ise, HTTP durum kodu 401 ' dir.
|InsufficientAuthorization|AuthorizationDisabled<br/>Authorization, zaman aşımına uğradı|Çağıranın kaynağa erişim izni olmadığında Bing, InsufficientAuthorization döndürür. Abonelik anahtarı devre dışı bırakılmışsa veya süresi dolmuşsa bu durum oluşabilir. <br/><br/>Hata InsufficientAuthorization ise, HTTP durum kodu 403 ' dir.

- Aşağıda önceki hata kodları yeni kodlara eşlenir. V5 hata kodlarıyla bir bağımlılık yaptıysanız, kodunuzu uygun şekilde güncelleştirin.

|Sürüm 5 kodu|Sürüm 7 Code. alt kod
|-|-
|RequestParameterMissing yok|Invalidrequest. ParameterMissing yok
Requestparameterınvalidvalue|Invalidrequest. Parameterınvalidvalue
Resourceaccessreddedildi|InsufficientAuthorization
ExceededVolume|Ratelimitexcebaşında
ExceededQpsLimit|Ratelimitexcebaşında
Devre dışı|InsufficientAuthorization. AuthorizationDisabled
UnexpectedError|ServerError. UnexpectedError
DataSourceErrors|Sunucuhatası. ResourceError
AuthorizationMissing|Invalidauthorleştirme. AuthorizationMissing
HttpNotAllowed|Invalidrequest. HttpNotAllowed
UserAgentMissing|Invalidrequest. ParameterMissing yok
NotImplemented|ServerError. NotImplemented
Invalidauthorleştirme|Invalidauthorleştirme
Invalidauthorizationmethod|Invalidauthorleştirme
MultipleAuthorizationMethod|Invalidauthorasyon. Authorizationartıklık
ExpiredAuthorizationToken|InsufficientAuthorization. Authorization, zaman aşımına uğradı
InsufficientScope|InsufficientAuthorization
Engellendi|Invalidrequest. engellendi

### <a name="query-parameters"></a>Sorgu parametreleri

- `modulesRequested`Sorgu parametresi [modüller](/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#modulesrequested)olarak yeniden adlandırıldı.  

### <a name="object-changes"></a>Nesne değişiklikleri

- `nextOffsetAddCount` [Videoların](/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#videos) alanı olarak yeniden adlandırıldı `nextOffset` . Sapmayı kullanma yönteminiz de değişmiştir. Daha önce, [fark](/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#offset) sorgu parametresini `nextOffset` değere ve önceki fark değerine ve sonuç içindeki videoların sayısına göre ayarlamanız gerekir. Şimdi, `offset` sorgu parametresini değere ayarlamanız yeterlidir `nextOffset` .  
  
- Alanın veri türü, `relatedVideos` öğesinden `Video[]` [videosmodule](/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#videosmodule) olarak değiştirildi (bkz. [videodetails](/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#videodetails)).