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
ms.date: 05/29/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: a4cb0f3d054f9afd0c606f80fd6fc5d553eff806
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74916324"
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
};
```

Örnek:

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