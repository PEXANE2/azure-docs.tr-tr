---
title: MSAL4J 'de hataları ve özel durumları işleme
titleSuffix: Microsoft identity platform
description: MSAL4J uygulamalarında hata ve özel durumları, koşullu erişim talep sorunlarını ve yeniden denemeleri nasıl işleyeceğinizi öğrenin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/27/2020
ms.author: marsma
ms.reviewer: saeeda, nacanuma
ms.custom: aaddev
ms.openlocfilehash: 8563804a736c37acc9a96eb4a186933507f34200
ms.sourcegitcommit: 3c3ec8cd21f2b0671bcd2230fc22e4b4adb11ce7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98760683"
---
# <a name="handle-errors-and-exceptions-in-msal-for-java"></a>Java için MSAL.js’de hataları ve özel durumları işleme

[!INCLUDE [Active directory error handling introduction](../../../includes/active-directory-develop-error-handling-introduction.md)]

## <a name="error-handling-in-msal-for-java"></a>Java için MSAL 'de hata işleme

Java için MSAL içinde üç özel durum türü vardır: `MsalClientException` , `MsalServiceException` , ve `MsalInteractionRequiredException` ; Tümü öğesinden devralınır `MsalException` .

- `MsalClientException` , kitaplıkta veya cihazda yerel olan bir hata oluştuğunda oluşturulur.
- `MsalServiceException` , güvenli belirteç hizmeti (STS) bir hata yanıtı döndürdüğünde veya başka bir ağ hatası oluştuğunda oluşturulur.
- `MsalInteractionRequiredException` , kimlik doğrulamasının başarılı olması için Kullanıcı arabirimi etkileşimi gerektiğinde oluşturulur.

### <a name="msalserviceexception"></a>MsalServiceException

`MsalServiceException` STS 'ye isteklerde döndürülen HTTP üstbilgilerini gösterir. Kullanarak bunlara erişin `MsalServiceException.headers()`

### <a name="msalinteractionrequiredexception"></a>Msalınteractionrequiredexception

Çağrılırken, Java için MSAL tarafından döndürülen yaygın durum kodlarından biridir `AcquireTokenSilently()` `InvalidGrantError` . Bu, bir kimlik doğrulama belirtecinin verilebilmesi için ek kullanıcı etkileşiminin gerekli olduğu anlamına gelir. Uygulamanız, kimlik doğrulama kitaplığını yeniden çağırmalıdır, ancak `AuthorizationCodeParameters` `DeviceCodeParameters` genel istemci uygulamaları göndererek veya için etkileşimli modda.

Çoğu zaman `AcquireTokenSilently` başarısız olduğunda, bunun nedeni belirteç önbelleğinde isteğinizle eşleşen bir belirteç yoktur. Erişim belirteçlerinin bir saatte bir süre sonu vardır ve `AcquireTokenSilently` yenileme belirtecine göre yeni bir tane almaya çalışır. OAuth2 koşullarında, bu yenileme belirteci akışdır. Bu akış, kiracı yöneticisinin daha sıkı oturum açma ilkelerini yapılandırdığında olduğu gibi çeşitli nedenlerle de başarısız olabilir.

Bu hatayla sonuçlanan bazı koşullar, kullanıcıların çözebilmesi için kolaydır. Örneğin, kullanım koşullarını kabul etmesi gerekebilir veya makinenin belirli bir kurumsal ağa bağlanması gerektiğinden istek geçerli yapılandırmayla yerine getirilemiyor.

MSAL `reason` , daha iyi bir kullanıcı deneyimi sağlamak için kullanabileceğiniz bir alan sunar. Örneğin, `reason` alanı kullanıcıya parolasının dolduğunu veya bazı kaynakları kullanmak için onay sağlamaları gerektiğini söyleyecektir. Desteklenen değerler numaralandırmanın bir parçasıdır  `InteractionRequiredExceptionReason` :

| Nedeni | Anlamı | Önerilen Işleme |
|---------|-----------|-----------------------------|
| `BasicAction` | Durum, etkileşimli kimlik doğrulama akışı sırasında Kullanıcı etkileşimi tarafından çözülebilir. | `acquireToken`Etkileşimli parametrelerle çağırın. |
| `AdditionalAction` | Koşul, etkileşimli kimlik doğrulama akışı dışındaki sistemle ek düzeltme etkileşimi tarafından çözülebilir. | `acquireToken`Gerçekleştirilecek düzeltici eylemi açıklayan bir ileti göstermek için etkileşimli parametrelerle çağırın. Çağıran uygulama, Kullanıcı düzeltici eylemi tamamlayamamasının olası olması beklenmez, ek eylem gerektiren akışları gizlemeyi tercih edebilir. |
| `MessageOnly` | Koşul şu anda çözümlenemiyor. Koşulu açıklayan bir ileti göstermek için etkileşimli kimlik doğrulama akışını başlatın. | `acquireToken`Koşulu açıklayan bir ileti göstermek için etkileşimli parametrelerle çağırın. `acquireToken``UserCanceled`Kullanıcı iletiyi okuduktan sonra hatayı döndürür ve pencereyi kapatır. Uygulama, kullanıcıdan iletiden daha fazla avantaj sağlamaması durumunda ileti ile sonuçlanan akışları gizlemeyi tercih edebilir. |
| `ConsentRequired`| Kullanıcı onayı eksik veya iptal edildi. |`acquireToken`Kullanıcının izin vermesi için etkileşimli parametrelerle çağırın. |
| `UserPasswordExpired` | Kullanıcının parolasının süresi doldu. | `acquireToken`Kullanıcının parolasını sıfırlayabilmesi için etkileşimli parametreyle çağırın. |
| `None` |  Daha fazla ayrıntı sağlanır. Bu koşul etkileşimli kimlik doğrulama akışı sırasında Kullanıcı etkileşimi tarafından çözülebilir. | `acquireToken`Etkileşimli parametrelerle çağırın. |

### <a name="code-example"></a>Kod Örneği

```java
        IAuthenticationResult result;
        try {
            PublicClientApplication application = PublicClientApplication
                    .builder("clientId")
                    .b2cAuthority("authority")
                    .build();

            SilentParameters parameters = SilentParameters
                    .builder(Collections.singleton("scope"))
                    .build();

            result = application.acquireTokenSilently(parameters).join();
        }
        catch (Exception ex){
            if(ex instanceof MsalInteractionRequiredException){
                // AcquireToken by either AuthorizationCodeParameters or DeviceCodeParameters
            } else{
                // Log and handle exception accordingly
            }
        }
```

[!INCLUDE [Active directory error handling claims challenges](../../../includes/active-directory-develop-error-handling-claims-challenges.md)]

[!INCLUDE [Active directory error handling retries](../../../includes/active-directory-develop-error-handling-retries.md)]

## <a name="next-steps"></a>Sonraki adımlar

Sorunları tanılamanıza ve hata ayıklamanıza yardımcı olmak üzere [Java IÇIN msal 'Te günlüğe kaydetmeyi](msal-logging-java.md) etkinleştirmeyi düşünün.
