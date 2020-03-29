---
title: Bing Yazım Denetimi API v5'i v7'ye yükseltin
titleSuffix: Azure Cognitive Services
description: Sürüm 7'yi kullanmak için uygulamanızın güncelleştirmeniz gereken bölümlerini tanımlar.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: scottwhi
ms.openlocfilehash: 545772a28a67310b12eb55cd1fb14e8d12a95a58
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68500813"
---
# <a name="spell-check-api-upgrade-guide"></a>Yazım Denetimi API yükseltme kılavuzu

Bu yükseltme kılavuzu, Bing Yazım Denetimi API'sinin sürüm 5 ve sürüm 7 arasındaki değişiklikleri tanımlar. Sürüm 7'yi kullanmak üzere güncelleştirmeniz gereken uygulamanızın bölümlerini belirlemenize yardımcı olmak için bu kılavuzu kullanın.

## <a name="breaking-changes"></a>Yeni değişiklikler

### <a name="endpoints"></a>Uç Noktalar

- Bitiş noktasının sürüm numarası v5'ten v7'ye değiştirildi. Örneğin, `https://api.cognitive.microsoft.com/bing/v7.0/spellcheck`.

### <a name="error-response-objects-and-error-codes"></a>Hata yanıt nesneleri ve hata kodları

- Tüm başarısız istekleri artık `ErrorResponse` yanıt gövdesinde bir nesne içermelidir.

- Nesneye `Error` aşağıdaki alanlar eklendi.  
  - `subCode`&mdash;Mümkünse hata kodunu ayrık kovalara bölümler
  - `moreDetails`&mdash;`message` Alanda açıklanan hata hakkında ek bilgi
   

- V5 hata kodlarını aşağıdaki olası `code` `subCode` ve değerlerle değiştirdi.  
  
|Kod|Subcode|Açıklama
|-|-|-
|SunucuHatası|Beklenmeyen Hata<br/>Kaynak Hatası<br/>Uygulanmadı|Alt kod koşullarından herhangi biri oluştuğunda Bing ServerError'ı döndürür. HTTP durum kodu 500 ise yanıt bu hataları içerir.
|Geçersizİstek|ParametreEksik<br/>ParametreGeçersiz Değer<br/>httpİzin verilen<br/>Engellendi|Bing, isteğin herhangi bir bölümü geçerli olmadığında Geçersiz İstek'i döndürür. Örneğin, gerekli bir parametre eksik veya bir parametre değeri geçerli değil.<br/><br/>Hata ParameterMissing veya ParameterGeçersizDeğer ise, HTTP durum kodu 400'dür.<br/><br/>Hata httpNotAllowed ise, HTTP durum kodu 410.
|RateLimitAşıldı||Bing, sorgularınızı saniyede (QPS) veya aylık sorgularınızı (QPM) kotanızı aştığınızda RateLimitExceeded'i döndürür.<br/><br/>QPS'yi aştıysanız Bing, HTTP durum kodu 429'u ve QPM'yi aştıysanız 403'ü döndürür.
|Geçersiz Yetkilendirme|Yetkilendirme Eksik<br/>YetkilendirmeRedundancy|Bing arayanın kimliğini doğrulayamadığınızda Bing Geçersiz Yetkilendirme'yi döndürür. Örneğin, `Ocp-Apim-Subscription-Key` üstbilgi eksik veya abonelik anahtarı geçerli değil.<br/><br/>Birden fazla kimlik doğrulama yöntemi belirtirseniz artıklık oluşur.<br/><br/>Hata Geçersiz Yetkilendirme ise, HTTP durum kodu 401'dir.
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

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kullanım ve görüntüleme gereksinimleri](./UseAndDisplayRequirements.md)
