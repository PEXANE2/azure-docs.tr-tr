---
title: Oturum açma & tek sayfalık uygulama oturum açma - Microsoft kimlik platformu | Azure
description: Tek sayfalı bir uygulama oluşturmayı öğrenin (oturum açma)
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
ms.openlocfilehash: 7e809def048c95b6688a13ac99783615eb045d11
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885198"
---
# <a name="single-page-application-sign-in-and-sign-out"></a>Tek sayfauygulaması: Oturum açma ve Oturum Açma

Tek sayfalı uygulamanızın koduna nasıl oturum açma ekleyeceğinizi öğrenin.

Uygulamanızdaki API'lere erişmek için belirteçleri alabilmeniz için kimlik doğrulaması yapılan bir kullanıcı bağlamına ihtiyacınız vardır. Kullanıcıları uygulamanızda MSAL.js'de iki şekilde oturum açabilirsiniz:

* [Açılan pencere](#sign-in-with-a-pop-up-window), `loginPopup` yöntemi kullanarak
* [Yönlendirme](#sign-in-with-redirect), `loginRedirect` yöntemi kullanarak

Ayrıca isteğe bağlı olarak, oturum açma sırasında kullanıcının rızasını alması gereken API'lerin kapsamlarını geçirebilirsiniz.

> [!NOTE]
> Uygulamanız zaten kimlik doğrulaması kullanıcı bağlamına veya kimlik belirtecine erişebiliyorsa, oturum açma adımını atlayabilir ve doğrudan belirteçler edinebilirsiniz. Ayrıntılar için, [MSAL.js giriş olmadan SSO](msal-js-sso.md#sso-without-msaljs-login)bakın.

## <a name="choosing-between-a-pop-up-or-redirect-experience"></a>Açılır pencere veya yeniden yönlendirme deneyimi arasında seçim yapmak

Uygulamanızda hem açılır pencere hem de yeniden yönlendirme yöntemlerini kullanamazsınız. Açılır pencere veya yeniden yönlendirme deneyimi arasındaki seçim, uygulama akışınıza bağlıdır:

* Kimlik doğrulama sırasında kullanıcıların ana uygulama sayfanızdan uzaklaşmasını istemiyorsanız, açılır pencere yöntemini öneririz. Kimlik doğrulama yönlendirmesi açılır pencerede gerçekleştiğinden, ana uygulamanın durumu korunur.

* Kullanıcıların açılır pencerelerin devre dışı bırakıldığı tarayıcı kısıtlamaları veya ilkeleri varsa, yeniden yönlendirme yöntemini kullanabilirsiniz. Internet Explorer'da [açılır pencerelerle ilgili bilinen sorunlar](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser)olduğundan, Internet Explorer tarayıcısıyla yönlendirme yöntemini kullanın.

## <a name="sign-in-with-a-pop-up-window"></a>Açılır pencereyle oturum açma

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const loginRequest = {
    scopes: ["https://graph.microsoft.com/User.ReadWrite"]
}

userAgentApplication.loginPopup(loginRequest).then(function (loginResponse) {
    //login success
    let idToken = loginResponse.idToken;
}).catch(function (error) {
    //login failure
    console.log(error);
});
```

# <a name="angular"></a>[Angular](#tab/angular)

MSAL Açısal sarıcı, rota tanımına ekleyerek `MsalGuard` uygulamanızda belirli yolları güvence altına almanızı sağlar. Bu koruma, bu rotaya erişildiğinde oturum açma yöntemini çağırır.

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

Açılır pencere deneyimi için yapılandırma `popUp` seçeneğini etkinleştirin. Ayrıca, onay gerektiren kapsamları aşağıdaki gibi geçirebilirsiniz:

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
            consentScopes: ["https://graph.microsoft.com/User.ReadWrite"]
        })
    ]
})
```
---

## <a name="sign-in-with-redirect"></a>Yeniden yönlendirme ile oturum açma

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Yönlendirme yöntemleri, ana uygulamadan uzaklaşmanedeniyle bir sözü döndürmez. İade edilen belirteçleri işlemek ve erişmek için, yönlendirme yöntemlerini aramadan önce başarı ve hata geri aramalarını kaydetmeniz gerekir.

```javascript
function authCallback(error, response) {
    //handle redirect response
}

userAgentApplication.handleRedirectCallback(authCallback);

const loginRequest = {
    scopes: ["https://graph.microsoft.com/User.ReadWrite"]
}

userAgentApplication.loginRedirect(loginRequest);
```

# <a name="angular"></a>[Angular](#tab/angular)

Buradaki kod, açılır pencereyle oturum açma ile ilgili bölümde daha önce açıklananla aynıdır. Varsayılan akış yeniden yönlendirilir.

> [!NOTE]
> Kimlik belirteci, onay verilen kapsamları içermez ve yalnızca kimlik doğrulaması yapılan kullanıcıyı temsil eder. İzin verilen kapsamlar, bir sonraki adımda elde acağınız erişim belirtecinde döndürülür.

---

## <a name="sign-out"></a>Oturumu kapatma

MSAL kitaplığı, `logout` tarayıcı depolamasındaki önbelleği temizleyen ve Azure Etkin Dizini'ne (Azure AD) oturum açma isteği gönderen bir yöntem sağlar. Oturum sonla'dan sonra kitaplık varsayılan olarak uygulama başlangıç sayfasına geri yönlendirir.

Uri'yi, oturum dışı böldükten sonra yeniden yönlendirmesi gereken URI'yi ayarlayarak `postLogoutRedirectUri`yapılandırabilirsiniz. Bu URI ayrıca başvuru kaydınızda giriş URI olarak da kaydedilmelidir.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const config = {
    auth: {
        clientId: 'your_app_id',
        redirectUri: "your_app_redirect_uri", //defaults to application start page
        postLogoutRedirectUri: "your_app_logout_redirect_uri"
    }
}

const userAgentApplication = new UserAgentApplication(config);
userAgentApplication.logout();

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
