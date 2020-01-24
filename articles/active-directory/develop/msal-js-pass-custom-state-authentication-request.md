---
title: Kimlik doğrulama isteklerinde özel durum geçirme (MSAL. js) | Mavisi
titleSuffix: Microsoft identity platform
description: JavaScript için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL. js) kullanarak kimlik doğrulama isteğinde özel durum parametre değeri geçirme hakkında bilgi edinin.
services: active-directory
author: TylerMSFT
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 01/16/2020
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: be2dd887358aa00c87a4b5668a99c425d83b59dc
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76696036"
---
# <a name="pass-custom-state-in-authentication-requests-using-msaljs"></a>MSAL. js kullanarak kimlik doğrulama isteklerinde özel durum geçirme

OAuth 2,0 tarafından tanımlanan *durum* parametresi bir kimlik doğrulama isteğine dahildir ve ayrıca, siteler arası istek sahteciliği saldırılarını engellemek için belirteç yanıtında de döndürülür. Varsayılan olarak, JavaScript için Microsoft kimlik doğrulama kitaplığı (MSAL. js), kimlik doğrulama isteklerinde rastgele oluşturulmuş benzersiz bir *durum* parametresi değeri geçirir.

Durum parametresi, yeniden yönlendirmenin önüne uygulama durumunun bilgilerini kodlamak için de kullanılabilir. Bu parametreye giriş olarak kullanıcının durumunu uygulamada (sayfada bulunan sayfa veya görüntüleme gibi) geçirebilirsiniz. MSAL. js kitaplığı, özel durumlarınızı `Request` nesnesine durum parametresi olarak geçirmenize olanak sağlar:

```javascript
// Request type
export type AuthenticationParameters = {
    scopes?: Array<string>;
    extraScopesToConsent?: Array<string>;
    prompt?: string;
    extraQueryParameters?: QPDict;
    claimsRequest?: string;
    authority?: string;
    state?: string;
    correlationId?: string;
    account?: Account;
    sid?: string;
    loginHint?: string;
    forceRefresh?: boolean;
};
```

> [!Note]
> Önbelleğe alınmış bir belirteci atlayıp sunucuya gitmek istiyorsanız, lütfen bir oturum açma/belirteç isteği oluşturmak için kullanılan AuthenticationParameters nesnesine Boole `forceRefresh` geçirin.
> `forceRefresh`, uygulamanızdaki performans etkisi nedeniyle varsayılan olarak kullanılmamalıdır.
> Önbelleğe bağlı olarak kullanıcılarınıza daha iyi bir deneyim sağlar.
> Önbelleğin atlanması yalnızca şu anda önbelleğe alınan verilerin güncel bilgilere sahip olmadığını bildiğiniz senaryolarda kullanılmalıdır.
> Bir kullanıcıya, güncelleştirilmiş rollere sahip yeni bir belirteç alması gereken roller ekleyen bir yönetim aracı gibi.

Örneğin:

```javascript
let loginRequest = {
    scopes: ["user.read", "user.write"],
    state: "page_url"
}

myMSALObj.loginPopup(loginRequest);
```

Geçirilen durum, istek gönderilirken MSAL. js tarafından ayarlanan benzersiz GUID 'ye eklenir. Yanıt döndürüldüğünde, MSAL. js bir durum eşleşmesi denetler ve sonra `Response` nesnesinde özel olarak geçirilen durumu `accountState`olarak döndürür.

```javascript
export type AuthResponse = {
    uniqueId: string;
    tenantId: string;
    tokenType: string;
    idToken: IdToken;
    accessToken: string;
    scopes: Array<string>;
    expiresOn: Date;
    account: Account;
    accountState: string;
};
```

Daha fazla bilgi edinmek için MSAL. js kullanarak [tek sayfalı uygulama (Spa) oluşturma](scenario-spa-overview.md) hakkında bilgi edinin.