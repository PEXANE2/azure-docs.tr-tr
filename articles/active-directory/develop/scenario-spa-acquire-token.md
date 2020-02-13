---
title: Bir Web API 'SI çağırmak için belirteç alma (tek sayfalı uygulamalar)-Microsoft Identity platform | Mavisi
description: Tek sayfalı uygulama oluşturmayı öğrenin (bir API çağırmak için belirteç alma)
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
ms.openlocfilehash: d5d48a2fc7aca184cf8b6e7761584a8800ca5151
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77160075"
---
# <a name="single-page-application-acquire-a-token-to-call-an-api"></a>Tek sayfalı uygulama: API 'YI çağırmak için belirteç alma

MSAL. js ile API 'Ler için belirteçleri alma deseninin, `acquireTokenSilent` yöntemi kullanılarak sessiz bir belirteç isteği denemesi gerekir. Bu yöntem çağrıldığında, kitaplık önce geçerli bir belirtecin mevcut olup olmadığını görmek için tarayıcı depolamada önbelleği denetler ve döndürür. Önbellekte geçerli bir belirteç yoksa, gizli bir iframe 'den Azure Active Directory (Azure AD) için sessiz bir belirteç isteği gönderir. Bu yöntem, kitaplığın belirteçleri yenilemesini de sağlar. Azure AD 'de çoklu oturum açma oturumu ve belirteç yaşam süresi değerleri hakkında daha fazla bilgi için bkz. [belirteç yaşam süreleri](active-directory-configurable-token-lifetimes.md).

Azure AD 'ye yönelik sessiz Belirteç istekleri, zaman aşımına uğradı bir Azure AD oturumu veya parola değişikliği gibi nedenlerle başarısız olabilir. Bu durumda, bir etkileşimli yöntemden birini çağırabilirsiniz (kullanıcıdan, belirteçleri, belirteç almasına izin verir).

* `acquireTokenPopup` kullanarak [açılır pencere](#acquire-a-token-with-a-pop-up-window)
* `acquireTokenRedirect` kullanarak [yeniden yönlendirme](#acquire-a-token-with-a-redirect)

## <a name="choose-between-a-pop-up-or-redirect-experience"></a>Bir açılır pencere veya yeniden yönlendirme deneyimi arasında seçim yapın

 Uygulamanızda hem açılır hem de yeniden yönlendirme yöntemlerini kullanamazsınız. Açılır veya yeniden yönlendirme deneyimi arasındaki seçim, uygulama akışınıza bağlıdır:

* Kimlik doğrulama sırasında kullanıcıların ana uygulama sayfanız dışında hareket etmemenizi istemiyorsanız, açılır yöntemi öneririz. Kimlik doğrulama yeniden yönlendirmesi bir açılır pencerede olduğundan, ana uygulamanın durumu korunur.

* Kullanıcılar, tarayıcı kısıtlamalarına veya açılır pencerelere ait pencerelerin devre dışı bırakıldığı ilkelere sahip ise, yeniden yönlendirme yöntemini kullanabilirsiniz. [Internet Explorer 'da açılır pencereler ile ilgili bilinen sorunlar](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser)olduğundan, Internet Explorer tarayıcısı ile yeniden yönlendirme yöntemini kullanın.

Erişim belirtecinin, erişim belirteci isteği oluştururken dahil olmasını istediğiniz API kapsamlarını ayarlayabilirsiniz. Tüm istenen kapsamların erişim belirtecinde verilmediğini unutmayın. Bu, kullanıcının onay durumuna bağlıdır.

## <a name="acquire-a-token-with-a-pop-up-window"></a>Açılır pencere ile belirteç alma

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki kod, daha önce açıklanan kalıbı bir açılır deneyim yöntemleriyle birleştirir:

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

# <a name="angulartabangular"></a>[Angular](#tab/angular)

MSAL angular sarmalayıcısı, erişim belirteçlerini sessizce otomatik olarak alacak ve bunları API 'lere HTTP isteklerine ekleyecek HTTP yakalayıcısını sağlar.

API 'Leri için kapsamları `protectedResourceMap` yapılandırma seçeneğinde belirtebilirsiniz. `MsalInterceptor`, belirteçleri otomatik olarak alırken bu kapsamları ister.

```javascript
//In app.module.ts
@NgModule({
  imports: [ MsalModule.forRoot({
                clientID: 'your_app_id',
                protectedResourceMap: {"https://graph.microsoft.com/v1.0/me", ["user.read", "mail.send"]}
            })]
         })

providers: [ ProductService, {
        provide: HTTP_INTERCEPTORS,
        useClass: MsalInterceptor,
        multi: true
    }
   ],
```

Sessiz belirteç alma başarısızlığının başarısı ve başarısızlığı için MSAL angular, abone olabileceğiniz geri çağrılar sağlar. Aboneliğinizi kaldırmak da önemlidir.

```javascript
// In app.component.ts
 ngOnInit() {
    this.subscription=  this.broadcastService.subscribe("msal:acquireTokenFailure", (payload) => {
    });
}

ngOnDestroy() {
   this.broadcastService.getMSALSubject().next(1);
   if(this.subscription) {
     this.subscription.unsubscribe();
   }
 }
```

Alternatif olarak, çekirdek MSAL. js kitaplığı 'nda açıklandığı gibi Acquire-Token yöntemlerini kullanarak açıkça belirteç alabilirsiniz.

---

## <a name="acquire-a-token-with-a-redirect"></a>Yeniden yönlendirme ile belirteç alma

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki model daha önce açıklanmamaktadır, ancak belirteçleri etkileşimli olarak almak için bir yeniden yönlendirme yöntemiyle gösteriliyor. Yeniden yönlendirme geri aramasını daha önce belirtildiği gibi kaydetmeniz gerekir.

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

## <a name="request-optional-claims"></a>İsteğe bağlı talepler isteme

Aşağıdaki amaçlar için isteğe bağlı talepler kullanabilirsiniz:

- Uygulamanıza yönelik belirteçlere ek talepler ekleyin.
- Azure AD 'nin belirteçlerde döndürdüğü belirli taleplerin davranışını değiştirin.
- Uygulamanız için özel talepler ekleyin ve erişin. 

`IdToken`isteğe bağlı talepler istemek için, bir strıngiingclaim nesnesini `AuthenticationParameters.ts` sınıfının `claimsRequest` alanına gönderebilirsiniz.

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

Daha fazla bilgi için bkz. [Isteğe bağlı talepler](active-directory-optional-claims.md).

# <a name="angulartabangular"></a>[Angular](#tab/angular)

Bu kod, daha önce açıklananla aynıdır.

---

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API 'SI çağırma](scenario-spa-call-api.md)
