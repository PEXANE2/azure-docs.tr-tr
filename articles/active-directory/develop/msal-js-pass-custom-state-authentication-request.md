---
title: Kimlik doğrulama isteklerinde özel durum geçirme (JavaScript için Microsoft kimlik doğrulama kitaplığı)
titleSuffix: Microsoft identity platform
description: JavaScript için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL. js) kullanarak kimlik doğrulama isteğinde özel durum parametre değeri geçirme hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/29/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8d2fa7d7d294d38d29ce8ace744e13bd1bf2d533
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72803040"
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
    state: “page_url”
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