---
title: Kimlik doğrulama isteklerinde özel durumu geçir (MSAL.js) | Azure
titleSuffix: Microsoft identity platform
description: JavaScript için Microsoft Kimlik Doğrulama Kitaplığı'nı (MSAL.js) kullanarak kimlik doğrulama isteğinde özel bir durum parametre değerini nasıl geçeceğinizi öğrenin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 01/16/2020
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 1c05956f83ad3a6491627be8916fac2c8be2b7ff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77084941"
---
# <a name="pass-custom-state-in-authentication-requests-using-msaljs"></a>MSAL.js kullanarak kimlik doğrulama isteklerinde özel durumu geçme

OAuth 2.0 tarafından tanımlanan *durum* parametresi, bir kimlik doğrulama isteğine dahildir ve ayrıca siteler arası sahteciliği önlemek için belirteç yanıtında döndürülür. Varsayılan olarak, JavaScript için Microsoft Kimlik Doğrulama Kitaplığı (MSAL.js) kimlik doğrulama isteklerinde rasgele oluşturulan benzersiz *durum* parametre değerini geçer.

Durum parametresi, yeniden yönlendirmeden önce uygulamanın durumuyla ilgili bilgileri kodlamak için de kullanılabilir. Bu parametreye giriş olarak, kullanıcının uygulamadaki durumunu(örneğin, sayfa veya üzerinde oldukları görünüm) geçirebilirsiniz. MSAL.js kitaplığı, nesnedeki durum parametresi olarak `Request` özel durumunuzu geçirmenize olanak tanır:

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
> Önbelleğe alınmış bir belirteci atlayıp sunucuya gitmek istiyorsanız, lütfen `forceRefresh` giriş/belirteç isteği nde kullanılan AuthenticationParameters nesnesine boolean'ı geçirin.
> `forceRefresh`uygulamanız üzerindeki performans etkisi nedeniyle varsayılan olarak kullanılmamalıdır.
> Önbelleğe güvenmek kullanıcılarınıza daha iyi bir deneyim sağlar.
> Önbelleği atlamak yalnızca önbelleğe alınmış verilerin güncel bilgilere sahip olmadığını bildiğiniz senaryolarda kullanılmalıdır.
> Güncelleştirilmiş rollerle yeni bir belirteç alması gereken bir kullanıcıya rol ekleyen bir Yönetici aracı gibi.

Örnek:

```javascript
let loginRequest = {
    scopes: ["user.read", "user.write"],
    state: "page_url"
}

myMSALObj.loginPopup(loginRequest);
```

Durum içinde geçirilen istek gönderirken MSAL.js tarafından belirlenen benzersiz GUID eklenir. Yanıt döndürüldüğünde, MSAL.js bir durum eşleşmesi için denetler ve `Response` sonra `accountState`nesnede geçen özel idurum'u döndürür.

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

Daha fazla bilgi için MSAL.js kullanarak [tek sayfalık bir uygulama (SPA) oluşturma](scenario-spa-overview.md) hakkında bilgi edinin.