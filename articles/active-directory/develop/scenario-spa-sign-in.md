---
title: Tek sayfalı uygulama oturum açma & oturum açma-Microsoft kimlik platformu | Mavisi
description: Tek sayfalı bir uygulama (oturum açma) oluşturmayı öğrenin
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 02/11/2020
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 53a84bd970d564411ec9a56b54159e5a96717a6e
ms.sourcegitcommit: 964af22b530263bb17fff94fd859321d37745d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84558757"
---
# <a name="single-page-application-sign-in-and-sign-out"></a>Tek sayfalı uygulama: oturum açma ve oturum kapatma

Tek sayfalı uygulamanız için koda oturum açma eklemeyi öğrenin.

Uygulamanızdaki API 'Lere erişim belirteçleri alabilmeniz için önce kimliği doğrulanmış bir Kullanıcı bağlamına ihtiyacınız vardır. MSAL. js ' de uygulamanızdaki kullanıcılara iki şekilde oturum açabilirsiniz:

* [Açılır pencere](#sign-in-with-a-pop-up-window), `loginPopup` yöntemini kullanarak
* Yöntemini kullanarak [yeniden yönlendirin](#sign-in-with-redirect) `loginRedirect`

İsteğe bağlı olarak, kullanıcının oturum açma sırasında izin vermesini gerektiren API 'lerin kapsamlarını da geçirebilirsiniz.

> [!NOTE]
> Uygulamanızın kimliği doğrulanmış bir Kullanıcı bağlamına veya KIMLIK belirtecine zaten erişimi varsa, oturum açma adımını atlayabilir ve belirteçleri doğrudan elde edebilirsiniz. Ayrıntılar için bkz. [msal. js oturum açma olmadan SSO](msal-js-sso.md#sso-without-msaljs-login).

## <a name="choosing-between-a-pop-up-or-redirect-experience"></a>Bir açılır pencere veya yeniden yönlendirme deneyimi arasında seçim yapma

Uygulamanızda hem açılır hem de yeniden yönlendirme yöntemlerini kullanamazsınız. Açılır veya yeniden yönlendirme deneyimi arasındaki seçim, uygulama akışınıza bağlıdır:

* Kimlik doğrulama sırasında kullanıcıların ana uygulama sayfasından uzakta geçiş yapmak istemiyorsanız, açılır yöntemi öneririz. Kimlik doğrulama yeniden yönlendirmesi bir açılır pencerede olduğundan, ana uygulamanın durumu korunur.

* Kullanıcıların, açılır pencerelerin devre dışı bırakıldığı tarayıcı kısıtlamaları veya ilkeleri varsa, yeniden yönlendirme yöntemini kullanabilirsiniz. [Internet Explorer 'da açılır pencereler ile ilgili bilinen sorunlar](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser)olduğundan, Internet Explorer tarayıcısı ile yeniden yönlendirme yöntemini kullanın.

## <a name="sign-in-with-a-pop-up-window"></a>Açılır pencere ile oturum açma

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript

const config = {
    auth: {
        clientId: 'your_app_id',
        redirectUri: "your_app_redirect_uri", //defaults to application start page
        postLogoutRedirectUri: "your_app_logout_redirect_uri"
    }
}

const loginRequest = {
    scopes: ["User.ReadWrite"]
}

const myMsal = new userAgentApplication(config);

myMsal.loginPopup(loginRequest)
    .then(function (loginResponse) {
        //login success
        let idToken = loginResponse.idToken;
    }).catch(function (error) {
        //login failure
        console.log(error);
    });
```

# <a name="angular"></a>[Angular](#tab/angular)

MSAL angular sarmalayıcısı, yol tanımına ekleyerek uygulamanızdaki belirli yolları güvenli hale getirmeye olanak tanır `MsalGuard` . Bu koruma, bu rotaya erişildiğinde oturum açma yöntemini çağırır.

```javascript
// In app-routing.module.ts
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';
import { ProfileComponent } from './profile/profile.component';
import { MsalGuard } from '@azure/msal-angular';
import { HomeComponent } from './home/home.component';

const routes: Routes = [
  {
    path: 'profile',
    component: ProfileComponent,
    canActivate: [
      MsalGuard
    ]
  },
  {
    path: '',
    component: HomeComponent
  }
];

@NgModule({
  imports: [RouterModule.forRoot(routes, { useHash: false })],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

Açılır pencere deneyimi için `popUp` yapılandırma seçeneğini etkinleştirin. Ayrıca, onay gerektiren kapsamları aşağıdaki gibi geçirebilirsiniz:

```javascript
// In app.module.ts
@NgModule({
    imports: [
        MsalModule.forRoot({
            auth: {
                clientId: 'your_app_id',
            }
        }, {
            popUp: true,
            consentScopes: ["User.ReadWrite"]
        })
    ]
})
```
---

## <a name="sign-in-with-redirect"></a>Yeniden yönlendirme ile oturum açma

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Yeniden yönlendirme yöntemleri, ana uygulamadan uzağa geçiş nedeniyle Promise döndürmez. Döndürülen belirteçleri işlemek ve erişmek için, yeniden yönlendirme yöntemlerini çağırmadan önce başarı ve hata geri çağırmaları kaydetmeniz gerekir.

```javascript

const config = {
    auth: {
        clientId: 'your_app_id',
        redirectUri: "your_app_redirect_uri", //defaults to application start page
        postLogoutRedirectUri: "your_app_logout_redirect_uri"
    }
}

const loginRequest = {
    scopes: ["User.ReadWrite"]
}

const myMsal = new userAgentApplication(config);

function authCallback(error, response) {
    //handle redirect response
}

myMsal.handleRedirectCallback(authCallback);

myMsal.loginRedirect(loginRequest);
```

# <a name="angular"></a>[Angular](#tab/angular)

Buradaki kod, açılır pencere ile oturum açma hakkında bölümünde daha önce açıklananla aynıdır. Varsayılan akış yeniden yönlendirme 'dir.

> [!NOTE]
> KIMLIK belirteci, onaylı kapsamları içermez ve yalnızca kimliği doğrulanmış kullanıcıyı temsil eder. Verilen kapsamlar, bir sonraki adımda elde ettiğiniz erişim belirtecinde döndürülür.

---

## <a name="sign-out"></a>Oturumu kapatma

MSAL kitaplığı, `logout` tarayıcı depolamada önbelleği temizleyen ve Azure Active Directory (Azure AD) için bir oturum kapatma isteği gönderen bir yöntem sağlar. Kaydolduktan sonra, kitaplık varsayılan olarak uygulama başlatma sayfasına yeniden yönlendirilir.

' İ ayarlayarak, oturum kapatıldıktan sonra yeniden yönlendirileceği URI 'yi yapılandırabilirsiniz `postLogoutRedirectUri` . Bu URI Ayrıca uygulama kaydlarınızın oturum kapatma URI 'SI olarak kaydedilmelidir.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const config = {
    auth: {
        clientId: 'your_app_id',
        redirectUri: "your_app_redirect_uri", //defaults to application start page
        postLogoutRedirectUri: "your_app_logout_redirect_uri"
    }
}

const myMsal = new UserAgentApplication(config);

myMsal.logout();
```

# <a name="angular"></a>[Angular](#tab/angular)

```javascript
//In app.module.ts
@NgModule({
    imports: [
        MsalModule.forRoot({
            auth: {
                clientId: 'your_app_id',
                postLogoutRedirectUri: "your_app_logout_redirect_uri"
            }
        })
    ]
})

// In app.component.ts
this.authService.logout();
```

---

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama için belirteç alma](scenario-spa-acquire-token.md)
