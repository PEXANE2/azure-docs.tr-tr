---
title: Bing Haber Arama API'si v5 'i v7 'ye yükseltme
titleSuffix: Azure Cognitive Services
description: Uygulamanızın 7 sürümünü kullanmak için güncelleştirmeniz gereken parçalarını tanımlar.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: scottwhi
ms.openlocfilehash: bad0ef849af7c94e63f1dfbebda7f47caef9947d
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80294377"
---
# <a name="news-search-api-upgrade-guide"></a>Haber Arama API Yükseltme Kılavuzu

Bu yükseltme Kılavuzu, sürüm 5 ve Bing Haber Arama API'si sürüm 7 arasındaki değişiklikleri tanımlar. Uygulamanızın 7 sürümünü kullanmak için güncelleştirmeniz gereken parçalarını belirlemenize yardımcı olması için bu kılavuzu kullanın.

## <a name="breaking-changes"></a>Yeni değişiklikler

### <a name="endpoints"></a>Uç Noktalar

- Uç noktanın sürüm numarası, V5 'ten v7 'e değişti. Örneğin, `https://api.cognitive.microsoft.com/bing/v7.0/news/search`.

### <a name="error-response-objects-and-error-codes"></a>Hata yanıtı nesneleri ve hata kodları

- Tüm başarısız istekler yanıt gövdesine bir `ErrorResponse` nesne içermelidir.

- `Error` Nesnesine aşağıdaki alanlar eklendi.  
  - `subCode`&mdash;Mümkünse hata kodunu farklı demetlere göre bölümler
  - `moreDetails`&mdash;`message` Alanda açıklanan hata hakkında ek bilgiler

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

### <a name="object-changes"></a>Nesne değişiklikleri

- `contractualRules` Alan, [newsarticle](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#newsarticle) nesnesine eklendi. `contractualRules` Alan, izlemeniz gereken kuralların bir listesini içerir (örneğin, makale attributıon). Kullanmak `contractualRules` `provider`yerine ' de sağlanmış olan attributıon öğesini uygulamanız gerekir. Makale yalnızca `contractualRules` [Web araması API](../bing-web-search/search-the-web.md) yanıtı bir haber yanıtı içerdiğinde içerir.

## <a name="non-breaking-changes"></a>Kırılamayan değişiklikler

### <a name="query-parameters"></a>Sorgu parametreleri

- [Kategori](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#category) sorgu parametresini ayarlayabileceğiniz olası bir değer olarak ürünler eklendi. [Kategorilere pazarlara göre](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference)bakın.

- En son ilk ile tarihe göre sıralanan popüler konuları döndüren [sortBy](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#sortby) sorgu parametresi eklendi.

- Belirtilen Unix dönem zaman damgasında veya sonrasında Bing tarafından bulunan popüler konuları döndüren [bu yana](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#since) sorgu parametresi eklendi.

### <a name="object-changes"></a>Nesne değişiklikleri

- `mentions` Alan, [newsarticle](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#newsarticle) nesnesine eklendi. Alan `mentions` , makalesinde bulunan varlıkların (kişiler veya konumlar) bir listesini içerir.

- `video` Alan, [newsarticle](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#newsarticle) nesnesine eklendi. Alan `video` , haber makalesiyle ilgili bir video içerir. Video, katıştırabilmeniz \<veya\> hareket küçük resmindeki bir iframe 'dir.

- `sort` Alan [haber](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#news) nesnesine eklendi. `sort` Alan, makalelerin sıralama sırasını gösterir. Örneğin, makaleler ilgiye (varsayılan) veya tarihe göre sıralanır.

- Sıralama düzenini tanımlayan [Sortvalue](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#sortvalue) nesnesi eklendi. `isSelected` Alan, yanıtın sıralama düzenini kullanıp kullanmadığını belirtir. **Doğru**ise, yanıt sıralama düzenini kullandı. `isSelected` **Yanlışsa**, farklı bir sıralama düzeni istemek için `url` alanındaki URL 'yi kullanabilirsiniz.
