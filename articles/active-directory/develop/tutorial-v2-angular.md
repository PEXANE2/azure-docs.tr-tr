---
title: Angular tek sayfalı uygulama öğreticisi-Azure
titleSuffix: Microsoft identity platform
description: Angular SPA uygulamalarının Microsoft Identity platform uç noktasından erişim belirteçleri gerektiren bir API 'YI nasıl çağırabileceğinizi öğrenin.
services: active-directory
author: hahamil
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.workload: identity
ms.date: 03/05/2020
ms.author: hahamil
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: c645ab45711698e4a6f582678e2a850e15dea62a
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82181605"
---
# <a name="tutorial-sign-in-users-and-call-the-microsoft-graph-api-from-an-angular-single-page-application"></a>Öğretici: Kullanıcı oturum açma ve angular tek sayfalı uygulamadan Microsoft Graph API 'sini çağırma

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Önizlemeler, [ek kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma (GA) önce değişebilir.

Bu öğreticide, angular tek sayfalı uygulamanın (SPA) nasıl kullanılabileceğini gösterilmektedir:
- Kişisel hesaplar, iş hesapları veya okul hesaplarında oturum açın.
- Erişim belirteci alın.
- Microsoft Graph API 'sini veya *Microsoft Identity platform uç noktasından*erişim belirteçleri gerektiren diğer API 'leri çağırın.

>[!NOTE]
>Bu öğreticide, Microsoft kimlik doğrulama kitaplığı (MSAL) kullanarak yeni bir angular SPA oluşturma işlemi adım adım açıklanmaktadır. Örnek bir uygulama indirmek istiyorsanız [hızlı başlangıç](quickstart-v2-angular.md)bölümüne bakın.

## <a name="how-the-sample-app-works"></a>Örnek uygulamanın nasıl çalıştığı

![Bu öğreticide oluşturulan örnek uygulamanın nasıl çalıştığını gösteren diyagram](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.svg)

### <a name="more-information"></a>Daha fazla bilgi

Bu öğreticide oluşturulan örnek uygulama, Microsoft Graph API 'sini veya Microsoft Identity platform uç noktasından belirteçleri kabul eden bir Web API 'sini sorgulamak için angular SPA 'yı sağlar. Angular kitaplığı için MSAL, çekirdek MSAL. js kitaplığının bir sarmalayıcısıdır. Microsoft Azure Active Directory, Microsoft hesabı kullanıcıları ve sosyal kimlik kullanıcıları (Facebook, Google ve LinkedIn gibi) kullanarak kurumsal kullanıcıların kimliğini doğrulamak için angular (6 +) uygulamalarının kimlik doğrulamasını sağlar. Kitaplık Ayrıca uygulamaların Microsoft bulut hizmetlerine veya Microsoft Graph erişmesini sağlar.

Bu senaryoda, bir Kullanıcı oturum açtıktan sonra, yetkilendirme üst bilgisi aracılığıyla bir erişim belirteci istenir ve HTTP isteklerine eklenir. Belirteç alma ve yenileme, MSAL tarafından işlenir.

### <a name="libraries"></a>Kitaplıklar

Bu öğretici aşağıdaki kitaplığı kullanır:

|Kitaplık|Açıklama|
|---|---|
|[msal. js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|JavaScript angular sarmalayıcı için Microsoft kimlik doğrulama kitaplığı|

MSAL. js kitaplığı için kaynak kodunu GitHub ' da [Azuread/Microsoft-Authentication-Library-for-js](https://github.com/AzureAD/microsoft-authentication-library-for-js) deposunda bulabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi çalıştırmak için şunlar gerekir:

* [Node. js](https://nodejs.org/en/download/)gibi yerel bir Web sunucusu. Bu öğreticideki yönergeler Node. js ' ye dayalıdır.
* Proje dosyalarını düzenlemek için [Visual Studio Code](https://code.visualstudio.com/download)gibi tümleşik bir geliştirme ORTAMı (IDE).

## <a name="create-your-project"></a>Projenizi oluşturma

Aşağıdaki NPM komutlarını kullanarak yeni bir angular uygulaması oluşturun:

```Bash
npm install -g @angular/cli@8                    # Install the Angular CLI
npm install @angular/material@8 @angular/cdk@8   # Install the Angular Material component library (optional, for UI)
ng new my-application --routing=true --style=css # Generate a new Angular app
npm install msal @azure/msal-angular             # Install MSAL and MSAL Angular in your application
ng generate component page-name                  # To add a new page (such as a home or profile page)
```

## <a name="register-your-application"></a>Uygulamanızı kaydetme

Azure portal [tek sayfalı bir uygulamayı kaydetmek için yönergeleri](https://docs.microsoft.com/azure/active-directory/develop/scenario-spa-app-registration) izleyin.

Kaydlarınızın uygulamaya **genel bakış** sayfasında, daha sonra kullanılmak üzere **uygulama (istemci) kimlik** değeri ' ne göz atın.

**Yeniden YÖNLENDIRME URI** değerini kaydedin **http://localhost:4200/** ve örtülü izin ayarlarını etkinleştirin.

## <a name="configure-the-application"></a>Uygulamayı yapılandırma

1. *Src/App* klasöründe *app. Module. TS* ' yi düzenleyin ve `MSALModule` `imports` `isIE` sabitine ek olarak ekleyin:

    ```javascript
    const isIE = window.navigator.userAgent.indexOf('MSIE ') > -1 || window.navigator.userAgent.indexOf('Trident/') > -1;
    @NgModule({
      declarations: [
        AppComponent
      ],
      imports: [
        BrowserModule,
        AppRoutingModule,
        MsalModule.forRoot({
          auth: {
            clientId: 'Enter_the_Application_Id_here', // This is your client ID
            authority: 'Enter_the_Cloud_Instance_Id_Here'/'Enter_the_Tenant_Info_Here', // This is your tenant ID
            redirectUri: 'Enter_the_Redirect_Uri_Here'// This is your redirect URI
          },
          cache: {
            cacheLocation: 'localStorage',
            storeAuthStateInCookie: isIE, // Set to true for Internet Explorer 11
          },
        }, {
          popUp: !isIE,
          consentScopes: [
            'user.read',
            'openid',
            'profile',
          ],
          unprotectedResources: [],
          protectedResourceMap: [
            ['https://graph.microsoft.com/v1.0/me', ['user.read']]
          ],
          extraQueryParameters: {}
        })
      ],
      providers: [],
      bootstrap: [AppComponent]
    })
    ```

    Şu değerleri değiştirin:

    |Değer adı|Hakkında|
    |---------|---------|
    |Enter_the_Application_Id_Here|Uygulama kaydlarınızın **genel bakış** sayfasında bu, **uygulamanızın (istemci) kimlik** değeridir. |
    |Enter_the_Cloud_Instance_Id_Here|Bu, Azure bulutunun örneğidir. Ana veya küresel Azure bulutu için girin **https://login.microsoftonline.com**. Ulusal bulutlar (örneğin, Çin) için bkz. [Ulusal bulutlar](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud).|
    |Enter_the_Tenant_Info_Here| Aşağıdaki seçeneklerden birine ayarlayın: uygulamanız *bu kuruluş dizinindeki hesapları*destekliyorsa, bu değeri dizin (KIRACı) kimliği veya kiracı adı (örneğin, **contoso.Microsoft.com**) ile değiştirin. Uygulamanız *herhangi bir kuruluş dizinindeki hesapları*destekliyorsa, bu değeri **kuruluşlar**ile değiştirin. Uygulamanız *herhangi bir kurumsal dizin ve kişisel Microsoft hesabında hesapları*destekliyorsa, bu değeri **ortak**ile değiştirin. *Yalnızca kişisel Microsoft hesaplarına*yönelik desteği kısıtlamak için bu değeri **tüketicilerle**değiştirin. |
    |Enter_the_Redirect_Uri_Here|İle **http://localhost:4200**değiştirin.|

    Kullanılabilir yapılandırılabilir seçenekler hakkında daha fazla bilgi için bkz. [istemci uygulamalarını başlatma](msal-js-initializing-client-applications.md).

2. Aynı dosyanın en üstünde aşağıdaki içeri aktarma ifadesini ekleyin:

    ```javascript
    import { MsalModule, MsalInterceptor } from '@azure/msal-angular';
    ```

3. Aşağıdaki import deyimlerini en üst öğesine ekleyin `src/app/app.component.ts`:

    ```javascript
    import { MsalService } from '@azure/msal-angular';
    import { Component, OnInit } from '@angular/core';
    ```
## <a name="sign-in-a-user"></a>Kullanıcı oturumu açma

Kullanıcı oturumu açmak `AppComponent` için aşağıdaki kodu ekleyin:

```javascript
export class AppComponent implements OnInit {
    constructor(private broadcastService: BroadcastService, private authService: MsalService) { }

    login() {
        const isIE = window.navigator.userAgent.indexOf('MSIE ') > -1 || window.navigator.userAgent.indexOf('Trident/') > -1;

        if (isIE) {
          this.authService.loginRedirect({
            extraScopesToConsent: ["user.read", "openid", "profile"]
          });
        } else {
          this.authService.loginPopup({
            extraScopesToConsent: ["user.read", "openid", "profile"]
          });
        }
    }
}
```

> [!TIP]
> Internet Explorer kullanıcıları `loginRedirect` için kullanmanızı öneririz.

## <a name="acquire-a-token"></a>Belirteç alma

### <a name="angular-interceptor"></a>Angular yakalayıcısı

MSAL angular, bilinen `Interceptor` korumalı kaynaklara angular `http` istemcisini kullanan giden istekler için otomatik olarak belirteçler elde eden bir sınıf sağlar.

İlk olarak, `Interceptor` sınıfı uygulamanıza sağlayıcı olarak ekleyin:

```javascript
import { MsalInterceptor, MsalModule } from "@azure/msal-angular";
import { HTTP_INTERCEPTORS, HttpClientModule } from "@angular/common/http";

@NgModule({
    // ...
    providers: [
        {
            provide: HTTP_INTERCEPTORS,
            useClass: MsalInterceptor,
            multi: true
        }
    ]
}
```

Daha sonra, ' `MsalModule.forRoot()` `protectedResourceMap` `consentScopes`ye bu kapsamları dahil et ve dahil olan korumalı kaynakların haritasını belirtin:

```javascript
@NgModule({
  // ...
  imports: [
    // ...
    MsalModule.forRoot({
      auth: {
        clientId: 'Enter_the_Application_Id_here', // This is your client ID
        authority: 'https://login.microsoftonline.com/Enter_the_Tenant_Info_Here', // This is your tenant info
        redirectUri: 'Enter_the_Redirect_Uri_Here' // This is your redirect URI
      },
      cache: {
        cacheLocation: 'localStorage',
        storeAuthStateInCookie: isIE, // Set to true for Internet Explorer 11
      },
    },
    {
      popUp: !isIE,
      consentScopes: [
        'user.read',
        'openid',
        'profile',
      ],
      unprotectedResources: [],
      protectedResourceMap: [
        ['https://graph.microsoft.com/v1.0/me', ['user.read']]
      ],
      extraQueryParameters: {}
    })
  ],
});
```

Son olarak, bir kullanıcının profilini HTTP isteğiyle alın:

```JavaScript
const graphMeEndpoint = "https://graph.microsoft.com/v1.0/me";

getProfile() {
  this.http.get(graphMeEndpoint).toPromise()
    .then(profile => {
      this.profile = profile;
    });
}
```

### <a name="acquiretokensilent-acquiretokenpopup-acquiretokenredirect"></a>acquireTokenSilent, acquireTokenPopup, acquireTokenRedirect
MSAL belirteçleri elde etmek için üç yöntem kullanır `acquireTokenRedirect`: `acquireTokenPopup`,, `acquireTokenSilent`ve. Ancak, önceki bölümde gösterildiği gibi `MsalInterceptor` , angular uygulamaları için sınıfını kullanmanızı öneririz.

#### <a name="get-a-user-token-silently"></a>Kullanıcı belirtecini sessizce alma

Yöntemi `acquireTokenSilent` , Kullanıcı etkileşimi olmadan belirteç alma ve yenileme işlemleri gerçekleştirir. Ya `loginRedirect` `loginPopup` da yöntemi ilk kez yürütüldükten sonra, `acquireTokenSilent` daha sonraki çağrılarındaki korunan kaynaklara erişmek için kullanılan belirteçleri almak için genellikle kullanılır. Belirteçleri istek veya yenileme çağrıları sessizce yapılır.

```javascript
const requestObj = {
    scopes: ["user.read"]
};

this.authService.acquireTokenSilent(requestObj).then(function (tokenResponse) {
    // Callback code here
    console.log(tokenResponse.accessToken);
}).catch(function (error) {
    console.log(error);
});
```

Bu kodda, `scopes` API için erişim belirtecinde döndürülmek istenen kapsamları içerir.

Örneğin:

* `["user.read"]`Microsoft Graph için
* `["<Application ID URL>/scope"]`Özel Web API 'Leri için (yani, `api://<Application ID>/access_as_user`)

#### <a name="get-a-user-token-interactively"></a>Etkileşimli olarak kullanıcı belirteci alma

Bazen kullanıcının Microsoft Identity platform uç noktasıyla etkileşim kurması gerekir. Örneğin:

* Parolasının süresi sona erdiği için kullanıcıların kimlik bilgilerini yeniden girmesi gerekebilir.
* Uygulamanız, kullanıcının onaylaması gereken ek kaynak kapsamlarına erişim istiyor.
* İki öğeli kimlik doğrulaması gereklidir.

Çoğu uygulama için önerilen model ilk olarak çağrı `acquireTokenSilent` yapmak, sonra özel durumu yakalamak ve sonra etkileşimli bir istek `acquireTokenPopup` başlatmak için `acquireTokenRedirect`(veya) çağırır.

Sonuçları `acquireTokenPopup` açılan bir oturum açma penceresinde çağırma. Alternatif olarak `acquireTokenRedirect` , kullanıcıları Microsoft Identity platform uç noktasına yönlendirir. Bu pencerede, kullanıcıların kimlik bilgilerini onaylaması, gerekli kaynağa onay vermesi veya iki öğeli kimlik doğrulamayı tamamlaması gerekir.

```javascript
  const requestObj = {
      scopes: ["user.read"]
  };

  this.authService.acquireTokenPopup(requestObj).then(function (tokenResponse) {
      // Callback code here
      console.log(tokenResponse.accessToken);
  }).catch(function (error) {
      console.log(error);
  });
```

> [!NOTE]
> Bu hızlı başlangıç, `loginRedirect` Internet `acquireTokenRedirect` Explorer tarafından açılır pencerelerin işlenmesiyle ilgili [bilinen bir sorun](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) nedeniyle Microsoft Internet Explorer ile ve yöntemlerini kullanır.

## <a name="log-out"></a>Oturumu Kapat

Bir kullanıcının oturumunu kapatmak için aşağıdaki kodu ekleyin:

```javascript
logout() {
  this.authService.logout();
}
```

## <a name="add-ui"></a>UI Ekle
Angular malzeme bileşen kitaplığı 'nı kullanarak Kullanıcı arabirimi ekleme hakkında bir örnek için bkz. [örnek uygulama](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular).

## <a name="test-your-code"></a>Kodunuza test etme

1.  Uygulama klasöründen bir komut satırı isteminde aşağıdaki komutları çalıştırarak bağlantı noktasını dinlemek için Web sunucusunu başlatın:

    ```bash
    npm install
    npm start
    ```
1. Tarayıcınızda, veya **http://localhost:4200** **http://localhost:{port}** yazın; burada *bağlantı noktası* , Web sunucunuzun dinlediği bağlantı noktasıdır.


### <a name="provide-consent-for-application-access"></a>Uygulama erişimi için onay sağlayın

Uygulamanızda oturum açmaya ilk kez başladığınızda, profilinize bu profile erişim vermeniz ve oturum açmasını sağlamanız istenir:

!["Izin isteniyor" penceresi](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

## <a name="add-scopes-and-delegated-permissions"></a>Kapsam ve temsilci izinleri ekleme

Microsoft Graph API 'SI, kullanıcının profilini okumak için *Kullanıcı. Read* kapsamını gerektirir. Varsayılan olarak, bu kapsam kayıt portalı 'nda kayıtlı her uygulamaya otomatik olarak eklenir. Microsoft Graph için diğer API 'Ler ve arka uç sunucunuza yönelik özel API 'Ler için ek kapsamlar gerekebilir. Örneğin, Microsoft Graph API 'SI, kullanıcının takvimlerini listelemek için *takvimlerin. Read* kapsamını gerektirir.

Kullanıcının takvimlerine bir uygulama bağlamında erişmek için, *takvimler.* uygulama kayıt bilgilerine, temsilci olarak oku iznini ekleyin. Ardından, *takvimlere. Read* kapsamını `acquireTokenSilent` çağrıya ekleyin.

>[!NOTE]
>Kapsam sayısını artırdıkça kullanıcıdan ek Yarışması istenebilir.

Bir arka uç API 'SI bir kapsam gerektirmiyorsa (önerilmez), belirteçleri almak için çağrılarındaki kapsam olarak *ClientID* 'yi kullanabilirsiniz.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Ardından, bir kullanıcı oturumunu nasıl kullanabileceğinizi ve angular öğreticisinde belirteç edinmeyi öğrenin:

> [!div class="nextstepaction"]
> [Angular öğreticisi](https://docs.microsoft.com/azure/active-directory/develop/tutorial-v2-angular)
