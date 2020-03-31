---
title: Sayfa yeniden yüklerinden kaçının (MSAL.js) | Azure
titleSuffix: Microsoft identity platform
description: JavaScript için Microsoft Kimlik Doğrulama Kitaplığını (MSAL.js) kullanarak belirteçleri sessizce edinirken ve yenilerken sayfa yeniden yüklenmesini nasıl önleyeceğimiz öğrenin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/29/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 63944a5a9af34c2d4cf98eeb870a730df49654e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77084963"
---
# <a name="avoid-page-reloads-when-acquiring-and-renewing-tokens-silently-using-msaljs"></a>MSAL.js kullanarak sessizce belirteçleri edinirken ve yenilerken sayfa yeniden yüklenmesini önleyin
JavaScript için Microsoft Kimlik Doğrulama Kitaplığı (MSAL.js), arka planda sessizce belirteçleri elde etmek ve yenilemek için gizli `iframe` öğeleri kullanır. Azure AD belirteci, belirteç isteğinde belirtilen kayıtlı redirect_uri geri döndürür (varsayılan olarak bu uygulamanın kök sayfasıdır). Yanıt 302 olduğundan, HTML'nin 'de `redirect_uri` `iframe`yüklenmeye karşılık gelen Genellikle uygulamanın `redirect_uri` kök sayfasıdır ve bu da yeniden yüklenmesine neden olur.

Diğer durumlarda, uygulamanın kök sayfasında gezinmek kimlik doğrulaması gerektiriyorsa, `iframe` iç `X-Frame-Options: deny` içe geçen öğelere veya hataya neden olabilir.

MSAL.js, Azure AD tarafından verilen 302'yi kapatamadığından ve döndürülen `redirect_uri` belirteci işlemek `iframe`için gerekli olduğundan, 'nin .

Uygulamanın tamamının yeniden yüklenmesinden veya bunun neden olduğu diğer hataları önlemek için lütfen bu geçici çözümlerini izleyin.

## <a name="specify-different-html-for-the-iframe"></a>iframe için farklı HTML belirtin

Config `redirect_uri` üzerindeki özelliği kimlik doğrulaması gerektirmeyen basit bir sayfaya ayarlayın. Azure portalında `redirect_uri` kayıtlı olanla eşleştiğinden emin olmalısın. MSAL, kullanıcı oturum açma işlemine başladığında başlangıç sayfasını kaydeder ve oturum açma işlemi tamamlandıktan sonra tam konuma geri yönlendirir.

## <a name="initialization-in-your-main-app-file"></a>Ana uygulama dosyanızda başlatma

Uygulamanız, uygulamanın başlatılmasını, yönlendirmesini ve diğer şeyleri tanımlayan bir merkezi Javascript dosyası olacak şekilde yapılandırılmışsa, uygulamanın bir `iframe` yükleme yapıp yapmamaya bağlı olarak uygulama modüllerinizi koşullu olarak yükleyebilirsiniz. Örnek:

AngularJS olarak: app.js

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

Açısal olarak: app.module.ts

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
MSAL.js kullanarak [tek sayfalık bir uygulama (SPA) oluşturma](scenario-spa-overview.md) hakkında daha fazla bilgi edinin.