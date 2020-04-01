---
title: Web API(tek sayfalı uygulamalar) aramak için bir belirteç edinin - Microsoft kimlik platformu | Azure
description: Tek sayfalı bir uygulama oluşturmayı öğrenin (API çağırmak için bir belirteç edinin)
services: active-directory
documentationcenter: dev-center-name
author: negoe
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/20/2019
ms.author: negoe
ms.custom: aaddev
ms.openlocfilehash: 393c3a06a2366a7d6947faf8bbfe038d6c5982fc
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80419656"
---
# <a name="single-page-application-acquire-a-token-to-call-an-api"></a>Tek sayfalı uygulama: API çağırmak için bir belirteç edinin

MSAL.js ile API'ler için belirteçleri edinme deseni ilk `acquireTokenSilent` yöntemi kullanarak sessiz bir belirteç isteği girişimidir. Bu yöntem çağrıldığında, kitaplık önce tarayıcı depolama alanında geçerli bir belirteç olup olmadığını görmek için önbelleği denetler ve döndürür. Önbellekte geçerli bir belirteç yoksa, gizli bir iframe'den Azure Etkin Dizin'e (Azure AD) sessiz bir belirteç isteği gönderir. Bu yöntem, kitaplığın belirteçleri yenilemesine de olanak tanır. Azure AD'de tek oturum oturumu ve belirteç yaşam süresi değerleri hakkında daha fazla bilgi için [bkz.](active-directory-configurable-token-lifetimes.md)

Azure AD'ye yönelik sessiz belirteç istekleri, süresi dolmuş Azure REKLAM oturumu veya parola değişikliği gibi nedenlerden dolayı başarısız olabilir. Bu durumda, belirteçleri elde etmek için etkileşimli yöntemlerden birini (kullanıcıyı ister) çağırabilirsiniz:

* [Açılan pencere](#acquire-a-token-with-a-pop-up-window), kullanarak`acquireTokenPopup`
* [Yeniden yönlendirme](#acquire-a-token-with-a-redirect), kullanarak`acquireTokenRedirect`

## <a name="choose-between-a-pop-up-or-redirect-experience"></a>Açılır pencere veya yeniden yönlendirme deneyimi arasında seçim yapma

 Uygulamanızda hem açılır pencere hem de yeniden yönlendirme yöntemlerini kullanamazsınız. Açılır pencere veya yeniden yönlendirme deneyimi arasındaki seçim, uygulama akışınıza bağlıdır:

* Kimlik doğrulama sırasında kullanıcıların ana uygulama sayfanızdan uzaklaşmasını istemiyorsanız, açılır pencere yöntemini önerdik. Kimlik doğrulama yönlendirmesi açılır pencerede gerçekleştiğinden, ana uygulamanın durumu korunur.

* Kullanıcıların açılır pencerelerinin devre dışı bırakıldığı tarayıcı kısıtlamaları veya ilkeleri varsa, yeniden yönlendirme yöntemini kullanabilirsiniz. Internet Explorer'da [açılır pencerelerle ilgili bilinen sorunlar](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser)olduğundan, Internet Explorer tarayıcısıyla yönlendirme yöntemini kullanın.

Erişim belirteci isteğini yaparken dahil olmasını istediğiniz API kapsamlarını ayarlayabilirsiniz. Erişim belirtecinde istenen tüm kapsamların verilmeyebileceğini unutmayın. Bu, kullanıcının rızasına bağlıdır.

## <a name="acquire-a-token-with-a-pop-up-window"></a>Açılır pencereli bir belirteç edinme

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki kod, daha önce açıklanan deseni açılır pencere yöntemleriyle birleştirir:

```javascript
const accessTokenRequest = {
    scopes: ["user.read"]
}

userAgentApplication.acquireTokenSilent(accessTokenRequest).then(function(accessTokenResponse) {
    // Acquire token silent success
    // Call API with token
    let accessToken = accessTokenResponse.accessToken;
}).catch(function (error) {
    //Acquire token silent failure, and send an interactive request
    if (error.errorMessage.indexOf("interaction_required") !== -1) {
        userAgentApplication.acquireTokenPopup(accessTokenRequest).then(function(accessTokenResponse) {
            // Acquire token interactive success
        }).catch(function(error) {
            // Acquire token interactive failure
            console.log(error);
        });
    }
    console.log(error);
});
```

# <a name="angular"></a>[Angular](#tab/angular)

MSAL Açısal sarıcı, otomatik olarak erişim belirteçleri elde edecek ve API'lere HTTP isteklerine eklemek HTTP önleme sağlar.

`protectedResourceMap` Yapılandırma seçeneğinde API'lerin kapsamlarını belirtebilirsiniz. `MsalInterceptor`jetonları otomatik olarak alırken bu kapsamları talep edecektir.

```javascript
// app.module.ts
@NgModule({
  declarations: [
    // ...
  ],
  imports: [
    // ...
    MsalModule.forRoot({
      auth: {
        clientId: 'Enter_the_Application_Id_Here',
      }
    },
    {
      popUp: !isIE,
      consentScopes: [
        'user.read',
        'openid',
        'profile',
      ],
      protectedResourceMap: [
        ['https://graph.microsoft.com/v1.0/me', ['user.read']]
      ]
    })
  ],
  providers: [
    {
      provide: HTTP_INTERCEPTORS,
      useClass: MsalInterceptor,
      multi: true
    }
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

Sessiz belirteç ediniminin başarısı ve başarısızlığı için MSAL Açısal, abone olabileceğiniz geri aramalar sağlar. Aboneliği iptal etmeyi de unutmamak önemlidir.

```javascript
// In app.component.ts
 ngOnInit() {
    this.subscription=  this.broadcastService.subscribe("msal:acquireTokenFailure", (payload) => {
    });
}

ngOnDestroy() {
   this.broadcastService.getMSALSubject().next(1);
   if (this.subscription) {
     this.subscription.unsubscribe();
   }
 }
```

Alternatif olarak, çekirdek MSAL.js kitaplığında açıklandığı gibi edinme belirteç yöntemlerini kullanarak belirteçleri açıkça elde edebilirsiniz.

---

## <a name="acquire-a-token-with-a-redirect"></a>Yeniden yönlendirme ile belirteç edinme

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki desen daha önce açıklandığı gibi, ancak etkileşimli belirteçleri elde etmek için bir yönlendirme yöntemi ile gösterilir. Daha önce belirtildiği gibi yeniden yönlendirme geri aramasını kaydetmeniz gerekir.

```javascript
function authCallback(error, response) {
    // Handle redirect response
}

userAgentApplication.handleRedirectCallback(authCallback);

const accessTokenRequest: AuthenticationParameters = {
    scopes: ["user.read"]
}

userAgentApplication.acquireTokenSilent(accessTokenRequest).then(function(accessTokenResponse) {
    // Acquire token silent success
    // Call API with token
    let accessToken = accessTokenResponse.accessToken;
}).catch(function (error) {
    //Acquire token silent failure, and send an interactive request
    console.log(error);
    if (error.errorMessage.indexOf("interaction_required") !== -1) {
        userAgentApplication.acquireTokenRedirect(accessTokenRequest);
    }
});
```

## <a name="request-optional-claims"></a>İsteğe bağlı talep isteme

İsteğe bağlı talepleri aşağıdaki amaçlar la kullanabilirsiniz:

- Uygulamanız için belirteçlere ek talepler ekleyin.
- Azure AD'nin belirteçlerle döndürdettiği bazı iddiaların davranışını değiştirin.
- Uygulamanız için özel talepler ekleyin ve erişin.

İsteğe bağlı `IdToken`talepler istemek için, `AuthenticationParameters.ts` sınıfın alanına `claimsRequest` dizilmiş bir talep nesnesi gönderebilirsiniz.

```javascript
"optionalClaims":
   {
      "idToken": [
            {
                  "name": "auth_time",
                  "essential": true
             }
      ],

var request = {
    scopes: ["user.read"],
    claimsRequest: JSON.stringify(claims)
};

myMSALObj.acquireTokenPopup(request);
```

Daha fazla bilgi için [isteğe bağlı taleplere](active-directory-optional-claims.md)bakın.

# <a name="angular"></a>[Angular](#tab/angular)

Bu kod, daha önce açıklandığı gibi aynıdır.

---

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API'sini çağırma](scenario-spa-call-api.md)
