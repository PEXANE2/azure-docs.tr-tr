---
title: Sayfa yeniden yüklemeden kaçının (JavaScript için Microsoft kimlik doğrulama kitaplığı) | Mavisi
description: JavaScript için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL. js) kullanarak belirteçleri sessizce alırken sayfa yeniden yüklemeden kaçınmanın nasıl yapılacağını öğrenin.
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
ms.openlocfilehash: c382c78cf631def74272768b78ee489e49820d04
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69532825"
---
# <a name="avoid-page-reloads-when-acquiring-and-renewing-tokens-silently-using-msaljs"></a>MSAL. js kullanarak belirteçleri sessizce alırken ve yenilerken sayfa yeniden yüklemeden kaçının
JavaScript için Microsoft kimlik doğrulama kitaplığı (msal. js), `iframe` arka planda belirteçleri sessizce almak ve yenilemek için gizli öğeleri kullanır. Azure AD belirteci, belirteç isteğinde belirtilen kayıtlı redirect_uri öğesine geri döndürür (varsayılan olarak uygulamanın kök sayfasıdır). Yanıt bir 302 olduğundan, ' `redirect_uri` `iframe`de yüklenen almaya karşılık gelen HTML ile sonuçlanır. Genellikle uygulama `redirect_uri` kök sayfasıdır ve bu, yeniden yüklenmesine neden olur.

Diğer durumlarda, uygulamanın kök sayfasına gidildiğinde kimlik doğrulaması yapılması gerekiyorsa, iç içe geçmiş `iframe` öğelere veya `X-Frame-Options: deny` hataya yol açabilir.

MSAL. js, Azure AD tarafından verilen 302 'yı kapata, ve döndürülen belirteci işlemek için gerekli olduğundan, `redirect_uri` `iframe`içinde yüklenmesini engellemez.

Uygulamanın tamamının yeniden yüklenmesi veya bunun nedeniyle oluşan diğer hataların olmaması için lütfen bu geçici çözümleri izleyin.

## <a name="specify-different-html-for-the-iframe"></a>İframe için farklı HTML belirtme

Config üzerinde `redirect_uri` özelliğini, kimlik doğrulaması gerektirmeyen basit bir sayfaya ayarlayın. Azure Portal `redirect_uri` kayıtlı ile eşleştiğinden emin olun. Bu, kullanıcının oturum açma işlemini başlatması ve oturum açma işlemi tamamlandıktan sonra tam konuma yeniden yönlendirdiği için MSAL olarak kullanıcının oturum açma deneyimini etkilemez.

## <a name="initialization-in-your-main-app-file"></a>Ana uygulama dosyanızda başlatma

Uygulamanız, uygulamanın başlatma, Yönlendirme ve diğer öğeleri tanımlayan bir merkezi JavaScript dosyası gibi yapılandırılmış ise, uygulama modüllerinizi uygulamanın bir `iframe` uygulamasına yüklenip yüklenmemesine göre koşullu olarak yükleyebilirsiniz. Örneğin:

AngularJS içinde: App. js

```javascript
// Check that the window is an iframe and not popup
if (window !== window.parent && !window.opener) {
angular.module('todoApp', ['ui.router', 'MsalAngular'])
    .config(['$httpProvider', 'msalAuthenticationServiceProvider','$locationProvider', function ($httpProvider, msalProvider,$locationProvider) {
        msalProvider.init(
            // msal configuration
        );

        $locationProvider.html5Mode(false).hashPrefix('');
    }]);
}
else {
    angular.module('todoApp', ['ui.router', 'MsalAngular'])
        .config(['$stateProvider', '$httpProvider', 'msalAuthenticationServiceProvider', '$locationProvider', function ($stateProvider, $httpProvider, msalProvider, $locationProvider) {
            $stateProvider.state("Home", {
                url: '/Home',
                controller: "homeCtrl",
                templateUrl: "/App/Views/Home.html",
            }).state("TodoList", {
                url: '/TodoList',
                controller: "todoListCtrl",
                templateUrl: "/App/Views/TodoList.html",
                requireLogin: true
            })

            $locationProvider.html5Mode(false).hashPrefix('');

            msalProvider.init(
                // msal configuration
            );
        }]);
}
```

Angular içinde: App. Module. TS

```javascript
// Imports...
@NgModule({
  declarations: [
    AppComponent,
    MsalComponent,
    MainMenuComponent,
    AccountMenuComponent,
    OsNavComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    HttpClientModule,
    ServiceWorkerModule.register('ngsw-worker.js', { enabled: environment.production }),
    MsalModule.forRoot(environment.MsalConfig),
    SuiModule,
    PagesModule
  ],
  providers: [
    HttpServiceHelper,
    {provide: HTTP_INTERCEPTORS, useClass: MsalInterceptor, multi: true},
    AuthService
  ],
  entryComponents: [
    AppComponent,
    MsalComponent
  ]
})
export class AppModule {
  constructor() {
    console.log('APP Module Constructor!');
  }

  ngDoBootstrap(ref: ApplicationRef) {
    if (window !== window.parent && !window.opener)
    {
      console.log("Bootstrap: MSAL");
      ref.bootstrap(MsalComponent);
    }
    else
    {
    //this.router.resetConfig(RouterModule);
      console.log("Bootstrap: App");
      ref.bootstrap(AppComponent);
    }
  }
}
```

MsalComponent:

```javascript
import { Component} from '@angular/core';
import { MsalService } from '@azure/msal-angular';

// This component is used only to avoid Angular reload
// when doing acquireTokenSilent()

@Component({
  selector: 'app-root',
  template: '',
})
export class MsalComponent {
  constructor(private Msal: MsalService) {
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar
MSAL. js kullanarak [tek sayfalı uygulama (Spa) oluşturma](scenario-spa-overview.md) hakkında daha fazla bilgi edinin.