---
title: Açısal tek sayfalık uygulama eğitimi - Azure
titleSuffix: Microsoft identity platform
description: Açısal SPA uygulamalarının Microsoft kimlik platformu bitiş noktasından erişim belirteçleri gerektiren bir API'yi nasıl çağırabileceğini öğrenin.
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
ms.openlocfilehash: ba7863d15ac0dfbebe6f14ef0d6f0daa93160b58
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81380034"
---
# <a name="tutorial-sign-in-users-and-call-the-microsoft-graph-api-from-an-angular-single-page-application"></a>Öğretici: Oturum açın ve Açısal tek sayfalı bir uygulamadan Microsoft Graph API'yi arayın

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Önizlemeler, [ek kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanılabilirlik (GA) önce değişebilir.

Bu öğretici, açısal tek sayfalı bir uygulamanın (SPA) nasıl yapabildiğini gösterir:
- Kişisel hesaplarda, iş hesaplarında veya okul hesaplarında oturum açın.
- Bir erişim jetonu edinin.
- *Microsoft kimlik platformu bitiş noktasından*erişim belirteçleri gerektiren Microsoft Graph API'sini veya diğer API'leri arayın.

>[!NOTE]
>Bu öğretici, Microsoft Kimlik Doğrulama Kitaplığı 'nı (MSAL) kullanarak yeni bir Açısal SPA oluşturmanız için size yol görür. Örnek bir uygulama indirmek istiyorsanız, [hızlı başlat'a](quickstart-v2-angular.md)bakın.

## <a name="how-the-sample-app-works"></a>Örnek uygulama nasıl çalışır?

![Bu öğreticide oluşturulan örnek uygulamanın nasıl çalıştığını gösteren diyagram](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.svg)

<!--start-collapse-->
### <a name="more-information"></a>Daha fazla bilgi

Bu öğreticide oluşturulan örnek uygulama, Açısal bir SPA'nın Microsoft Graph API'sini veya Microsoft kimlik platformu bitiş noktasından belirteçleri kabul eden bir Web API'sini sorgulamasını sağlar. Açısal kütüphane için MSAL çekirdek MSAL.js kitaplığın bir sarmalayıcıolduğunu. Microsoft Azure Active Directory, Microsoft hesap kullanıcıları ve sosyal kimlik kullanıcıları (Facebook, Google ve LinkedIn gibi) kullanarak kurumsal kullanıcıların kimliğini doğrulamak için Açısal (6+) uygulamalar sağlar. Kitaplık ayrıca uygulamaların Microsoft bulut hizmetlerine veya Microsoft Graph'a erişmesini sağlar.

Bu senaryoda, bir kullanıcı giriş yaptıktan sonra, bir erişim belirteci istenir ve yetkilendirme üstbilgisi aracılığıyla HTTP isteklerine eklenir. Belirteç edinimi ve yenileme MSAL tarafından yürütilir.

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Kitaplıklar

Bu öğretici aşağıdaki kitaplığı kullanır:

|Kitaplık|Açıklama|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|JavaScript Açısal Sarıcı için Microsoft Kimlik Doğrulama Kitaplığı|

> [!NOTE]
> *Msal.js,* kişisel hesapların, iş hesaplarının ve okul hesaplarının oturum açmasını ve jeton edinmesini sağlayan Microsoft kimlik platformu bitiş noktasını hedefler. Microsoft kimlik platformu bitiş noktası [bazı sınırlamalar](../azuread-dev/azure-ad-endpoint-comparison.md#limitations)vardır.
> v1.0 ve v2.0 uç noktaları arasındaki farkları anlamak için [bitiş noktası karşılaştırma kılavuzuna](../azuread-dev/azure-ad-endpoint-comparison.md)bakın.

MSAL.js kitaplığı için kaynak kodunu GitHub'daki [AzureAD/microsoft-authentication-library-for-js](https://github.com/AzureAD/microsoft-authentication-library-for-js) deposunda bulabilirsiniz.

<!--end-collapse-->


## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi çalıştırmak için şunları yapmanız gerekir:

* [Node.js](https://nodejs.org/en/download/)gibi yerel bir web sunucusu. Bu öğreticideki talimatlar Düğüm.js dayanmaktadır.
* Proje dosyalarını düzenlemesi için [Visual Studio Code](https://code.visualstudio.com/download)gibi tümleşik bir geliştirme ortamı (IDE).

## <a name="create-your-project"></a>Projenizi oluşturun

Aşağıdaki npm komutlarını kullanarak yeni bir Açısal uygulama oluşturun:

```Bash
npm install -g @angular/cli@8                    # Install the Angular CLI
npm install @angular/material@8 @angular/cdk@8   # Install the Angular Material component library (optional, for UI)
ng new my-application --routing=true --style=css # Generate a new Angular app
npm install msal @azure/msal-angular             # Install MSAL and MSAL Angular in your application
ng generate component page-name                  # To add a new page (such as a home or profile page)
```

## <a name="register-your-application"></a>Uygulamanızı kaydetme

Azure portalında [tek sayfalık bir uygulama kaydetmek için yönergeleri](https://docs.microsoft.com/azure/active-directory/develop/scenario-spa-app-registration) izleyin.

Kaydınızın uygulamaya **Genel Bakış** sayfasında, daha sonra kullanmak üzere **Uygulama (istemci) kimlik** değerine dikkat edin.

Yeniden **Yönlendirme URI** değerinizi olarak **http://localhost:4200/** kaydedin ve örtülü hibe ayarlarını etkinleştirin.

## <a name="configure-the-application"></a>Uygulamayı yapılandırma

1. *src/app* klasöründe *app.module.ts'yi* ve `MSALModule` `imports` `isIE` sabiti de ekleyin:

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

    Bu değerleri değiştirin:

    |Değer adı|Hakkında|
    |---------|---------|
    |Enter_the_Application_Id_Here|Uygulama kaydınızın **Genel Bakış** sayfasında, bu sizin **Uygulama (istemci) kimlik** değerinizdir. |
    |Enter_the_Cloud_Instance_Id_Here|Bu, Azure bulutunun örneğidir. Ana veya küresel Azure bulutu için ' yi girin. **https://login.microsoftonline.com** Ulusal bulutlar (örneğin, Çin) için [Ulusal bulutlara](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud)bakın.|
    |Enter_the_Tenant_Info_Here| Aşağıdaki seçeneklerden birine ayarlayın: Uygulamanız *bu kuruluş dizinindeki hesapları*destekliyorsa, bu değeri dizin (kiracı) kimliği veya kiracı adı (örneğin, **contoso.microsoft.com)** ile değiştirin. Uygulamanız *herhangi bir kuruluş dizinindeki hesapları destekliyorsa,* bu değeri **kuruluşlarla**değiştirin. Uygulamanız *herhangi bir kuruluş dizinindeki ve kişisel Microsoft hesaplarındaki hesapları*destekliyorsa, bu değeri **ortak**olanla değiştirin. Desteği yalnızca *kişisel Microsoft hesaplarına*kısıtlamak için, bu değeri **tüketicilerle**değiştirin. |
    |Enter_the_Redirect_Uri_Here|Değiştir' **http://localhost:4200**le değiştirin.|

    Kullanılabilir yapılandırılabilir seçenekler hakkında daha fazla bilgi için [bkz.](msal-js-initializing-client-applications.md)

2. Aynı dosyanın üst kısmında aşağıdaki alma deyimini ekleyin:

    ```javascript
    import { MsalModule, MsalInterceptor } from '@azure/msal-angular';
    ```

3. Aşağıdaki alma deyimlerini en `src/app/app.component.ts`üste ekleyin:

    ```javascript
    import { MsalService } from '@azure/msal-angular';
    import { Component, OnInit } from '@angular/core';
    ```
## <a name="sign-in-a-user"></a>Kullanıcıda oturum açma

Bir kullanıcıda `AppComponent` oturum açabilmek için aşağıdaki kodu ekleyin:

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
> Internet Explorer `loginRedirect` kullanıcıları için kullanmanızı öneririz.

## <a name="acquire-a-token"></a>Belirteç alma

### <a name="angular-interceptor"></a>Açısal Durdurucu

MSAL Açısal, `Interceptor` Açısal `http` istemciyi bilinen korumalı kaynaklara kullanan giden istekler için otomatik olarak belirteçleri alan bir sınıf sağlar.

İlk olarak, `Interceptor` uygulamanız için bir sağlayıcı olarak sınıf dahil:

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

Ardından, korunan kaynakların `MsalModule.forRoot()` bir haritasını sağlamak `protectedResourceMap` ve `consentScopes`bu kapsamları şu şekilde dahil edin:

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

Son olarak, bir HTTP isteği ile bir kullanıcının profilini almak:

```JavaScript
const graphMeEndpoint = "https://graph.microsoft.com/v1.0/me";

getProfile() {
  this.http.get(graphMeEndpoint).toPromise()
    .then(profile => {
      this.profile = profile;
    });
}
```

### <a name="acquiretokensilent-acquiretokenpopup-acquiretokenredirect"></a>TokenSilent'ı satın almak, TokenPopup'u satın almak, TokenRedirect'i satın almak
MSAL belirteçleri elde etmek `acquireTokenRedirect` `acquireTokenPopup`için `acquireTokenSilent`üç yöntem kullanır: , , ve . Ancak, önceki bölümde `MsalInterceptor` gösterildiği gibi, Açısal uygulamalar yerine sınıfı kullanmanızı öneririz.

#### <a name="get-a-user-token-silently"></a>Kullanıcı belirtecini sessizce alma

Yöntem, `acquireTokenSilent` kullanıcı etkileşimi olmadan belirteç satın almalarını ve yenilemeyi işler. Veya yöntem ilk kez yürütüldükten sonra, `acquireTokenSilent` genellikle sonraki aramalarda korumalı kaynaklara erişmek için kullanılan belirteçleri elde etmek için kullanılır. `loginPopup` `loginRedirect` Jeton istemek veya yenilemek için çağrılar sessizce yapılır.

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

Bu kodda, `scopes` API'nin erişim belirtecinde döndürülmek istenen kapsamları içerir.

Örneğin:

* `["user.read"]`Microsoft Graph için
* `["<Application ID URL>/scope"]`özel web API'leri `api://<Application ID>/access_as_user`için (yani, )

#### <a name="get-a-user-token-interactively"></a>Etkileşimli olarak kullanıcı belirteci alma

Bazen kullanıcının Microsoft kimlik platformu bitiş noktasıyla etkileşimde olması gerekir. Örneğin:

* Parolalarının süresi dolduğundan kullanıcıların kimlik bilgilerini yeniden girmeleri gerekebilir.
* Uygulamanız, kullanıcının onay ını alması gereken ek kaynak kapsamlarına erişim talep ediyor.
* İki faktörlü kimlik doğrulama gereklidir.

Çoğu uygulama için önerilen desen `acquireTokenSilent` önce aramak, sonra özel durumu `acquireTokenPopup` yakalamak `acquireTokenRedirect`ve ardından etkileşimli bir istek başlatmak için (veya) aramaktır.

Arama, `acquireTokenPopup` açılır pencereyle sonuçlanır. Alternatif olarak, `acquireTokenRedirect` kullanıcıları Microsoft kimlik platformu bitiş noktasına yönlendirir. Bu pencerede, kullanıcıların kimlik bilgilerini onaylamaları, gerekli kaynağa izin vermeleri veya iki faktörlü kimlik doğrulamayı tamamlamaları gerekir.

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
> Bu hızlı başlatma, `acquireTokenRedirect` internet explorer tarafından açılır pencerelerin işlenmesiyle ilgili bilinen bir [sorun](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) nedeniyle Microsoft Internet Explorer ile `loginRedirect` ve yöntemleri kullanır.

## <a name="log-out"></a>Oturumu çıkış

Bir kullanıcıyı oturum dışı açmak için aşağıdaki kodu ekleyin:

```javascript
logout() {
  this.authService.logout();
}
```

## <a name="add-ui"></a>UI Ekle
Açısal Malzeme bileşen kitaplığını kullanarak Kullanıcı Arabirimi ekleme nin [bir](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular)örneği için örnek uygulamaya bakın.

## <a name="test-your-code"></a>Kodunuza test etme

1.  Uygulama klasöründen komut satırı isteminde aşağıdaki komutları çalıştırarak bağlantı noktasını dinlemek için web sunucusunu başlatın:

    ```bash
    npm install
    npm start
    ```
1. Tarayıcınızda, web **http://localhost:4200** **http://localhost:{port}** sunucunuzun dinlediği *bağlantı noktasının* bulunduğu bağlantı noktasını girin veya girin.


### <a name="provide-consent-for-application-access"></a>Uygulama erişimi için onay sağlama

Başvurunuzda ilk oturum açtığınızda, bu uygulamanın profilinize erişmesine izin vermeniz ve oturum açmanız için izin vermeniz istenir:

!["İstenen İzinler" penceresi](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)



<!--start-collapse-->
### <a name="add-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri ekleme

Microsoft Graph API' si, kullanıcının profilini okuması için *kullanıcının okuma* kapsamını gerektirir. Varsayılan olarak, bu kapsam kayıt portalına kayıtlı olan her uygulamaya otomatik olarak eklenir. Microsoft Graph için diğer API'lerin yanı sıra arka uç sunucunuz için özel API'ler ek kapsamlar gerektirebilir. Örneğin, Microsoft Graph API kullanıcının takvimlerini listelemek için *Takvimler.Oku* kapsamını gerektirir.

Bir uygulama bağlamında kullanıcının takvimlerine erişmek için, uygulama kayıt bilgilerine *Takvimler.Read* temsilcisi iznini ekleyin. Ardından, `acquireTokenSilent` aramaiçin *Takvimler.Oku* kapsamını ekleyin.

>[!NOTE]
>Kapsam sayısını artırdıkça kullanıcıdan ek izinler istenebilir.

Arka uç API'si kapsam gerektirmiyorsa (önerilmez), belirteçleri elde etmek için aramalarda *istemciyi* kapsam olarak kullanabilirsiniz.

<!--end-collapse-->

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Ardından, bir kullanıcıda nasıl oturum açışlayacağınıve Açısal öğreticide belirteçleri nasıl edineceklerini öğrenin:

> [!div class="nextstepaction"]
> [Açısal öğretici](https://docs.microsoft.com/azure/active-directory/develop/tutorial-v2-angular)
