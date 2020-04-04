---
title: Açısal tek sayfalık uygulama eğitimi - Azure
titleSuffix: Microsoft identity platform
description: Köşeli SPA uygulamalarının Microsoft kimlik platformu bitiş noktasından erişim belirteçleri gerektiren bir API'yi nasıl çağırabileceğini öğrenin
services: active-directory
documentationcenter: dev-center-name
author: hahamil
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/05/2020
ms.author: hahamil
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 81ade6f02b38a0a4d5ed04c8190b99216638c45d
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80637843"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-api-from-an-angular-single-page-application-spa"></a>Oturum açın ve Açısal tek sayfalı bir uygulamadan (SPA) Microsoft Graph API'yi arayın

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Önizlemeler, [ek kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

Bu kılavuz, açısal tek sayfalı bir uygulamanın (SPA) nasıl
- Kişisel hesapların yanı sıra iş ve okul hesaplarında oturum açın
- Erişim belirteci alma
- *Microsoft kimlik platformu bitiş noktasından* erişim belirteçleri gerektiren Microsoft Graph API veya diğer API'leri arayın

>[!NOTE]
>Bu öğretici nasıl MSAL kullanarak yeni bir Açısal SPA oluşturmak için size yol verecektir. Örnek bir uygulama indirmek istiyorsanız, lütfen [hızlı başlangıç](quickstart-v2-angular.md)

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Bu kılavuz tarafından oluşturulan örnek uygulama nasıl çalışır?

![Bu öğretici tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.svg)

<!--start-collapse-->
### <a name="more-information"></a>Daha fazla bilgi

Bu kılavuz tarafından oluşturulan örnek uygulama, Açısal BIR SPA'nın Microsoft Graph API'sini veya Microsoft kimlik platformu bitiş noktasından belirteçleri kabul eden bir web API'sini sorgulamasını sağlar. Açısal kütüphane için MSAL çekirdek MSAL.js kitaplığın bir sarmalayıcıolduğunu. Microsoft Azure Active Directory (AAD), Microsoft hesap kullanıcıları (MSA), sosyal kimlik kullanıcıları (Facebook, Google, LinkedIn, vb.) kullanarak kurumsal kullanıcıların kimliğini doğrulamak ve Microsoft Cloud veya Microsoft Graph'a erişim sağlamak için Açısal (6+) uygulamalar sağlar. Bu senaryoda, bir kullanıcı giriş yaptıktan sonra, bir erişim belirteci istenir ve yetkilendirme üstbilgisi aracılığıyla HTTP isteklerine eklenir. Belirteç edinme ve yenileme Microsoft Kimlik Doğrulama Kitaplığı (MSAL) tarafından işlenir.

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Kitaplıklar

Bu kılavuzda aşağıdaki kitaplık kullanır:

|Kitaplık|Açıklama|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|JavaScript Açısal Sarıcı için Microsoft Kimlik Doğrulama Kitaplığı|

> [!NOTE]
> *Msal.js,* kişisel hesapların, okul ve iş hesaplarının oturum açmasını ve jeton edinmesini sağlayan Microsoft kimlik platformu bitiş noktasını hedefler. Microsoft kimlik platformu bitiş noktası [bazı sınırlamalar](../azuread-dev/azure-ad-endpoint-comparison.md#limitations)vardır.
> v1.0 ve v2.0 uç noktaları arasındaki farkları anlamak için [bitiş noktası karşılaştırma kılavuzuna](../azuread-dev/azure-ad-endpoint-comparison.md)bakın.

<!--end-collapse-->


## <a name="prerequisites"></a>Ön koşullar

* Bu öğreticiyi çalıştırmak [için, Node.js](https://nodejs.org/en/download/) gibi yerel bir web sunucusuna ihtiyacınız vardır

* Proje dosyalarını düzenlemesi için Visual [Studio Code](https://code.visualstudio.com/download)gibi tümleşik bir geliştirme ortamı (IDE) yükleyin.

* Bu kılavuzdaki talimatlar Düğüm.js dayanmaktadır

## <a name="create-your-project"></a>Projenizi oluşturun

Aşağıdaki npm komutlarını kullanarak yeni bir Açısal uygulama oluşturun:

```Bash
npm install -g @angular/cli@8                    # Install the Angular CLI
npm install @angular/material@8 @angular/cdk@8   # Install the Angular Material component library (optional, for UI)
ng new my-application --routing=true --style=css # Generate a new Angular app
npm install msal @azure/msal-angular             # Install MSAL and MSAL Angular in your application
ng generate component page-name                  # To add a new page (such as a the home, profile page)
```

## <a name="register-your-application"></a>Uygulamanızı kaydetme

Azure portalında [tek sayfalık bir uygulama kaydetmek](https://docs.microsoft.com/azure/active-directory/develop/scenario-spa-app-registration) için yönergeleri izleyin.

 Kaydınızın uygulamaya **Genel Bakış** sayfasında, daha sonra kullanmak üzere **Uygulama (istemci) kimlik** değerine dikkat edin.

 **Redirect** URI'nizi `http://localhost:4200/` olarak kaydedin ve örtülü hibe ayarlarını etkinleştirin.

#### <a name="configure-your-angular-application"></a>Açısal uygulamanızı yapılandırın

1. *src/app* klasöründe *app.module.ts'yi* edin `MSALModule` `imports` ve aşağıda `isIE` gösterildiği gibi const'a ekleyin:

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
            authority: 'Enter_the_Cloud_Instance_Id_Here'/'Enter_the_Tenant_Info_Here', // This is your tenant id
            redirectUri: 'Enter_the_Redirect_Uri_Here'// This is your redirect URI
          },
          cache: {
            cacheLocation: 'localStorage',
            storeAuthStateInCookie: isIE, // set to true for IE 11
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

    Bu değerleri şu şekilde değiştirin:

    |Değer adı|Hakkında|
    |---------|---------|
    |Enter_the_Application_Id_Here|Başvuru kaydınızın **Genel Bakış** sayfasında, bu sizin **Uygulama (istemci) kimliğiniz** |
    |Enter_the_Cloud_Instance_Id_Here|Bu, Azure bulutunun örneğidir. Ana veya küresel Azure bulutu için ' yi girin. https://login.microsoftonline.com Ulusal bulutlar (örneğin, Çin) için [Ulusal bulutlara](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud)bakın.|
    |Enter_the_Tenant_Info_Here| Aşağıdaki seçeneklerden birine ayarlayın: 1) Uygulamanız *bu kuruluş dizinindeki hesapları*destekliyorsa, bu değeri **Dizin (Kiracı) Kimliği** veya Kiracı **adı** (örneğin, *contoso.microsoft.com)* ile değiştirin. 2) Uygulamanız *herhangi bir kuruluş dizinindeki hesapları destekliyorsa,* bu değeri **kuruluşlarla**değiştirin. 3) Uygulamanız *herhangi bir kuruluş dizinindeki ve kişisel Microsoft hesaplarındaki hesapları*destekliyorsa, bu değeri **ortak**değerlerle değiştirin. 4) Desteği *yalnızca kişisel Microsoft hesaplarına*kısıtlamak için, bu değeri **tüketicilerle**değiştirin. |
    |Enter_the_Redirect_Uri_Here|Değiştir`http://localhost:4200`|

    Kullanılabilir yapılandırılabilir seçenekler hakkında daha fazla bilgi için [bkz.](msal-js-initializing-client-applications.md)

2. Aynı dosyada, dosyanın üst kısmında aşağıdaki alma ekleyin:

    ```javascript
    import { MsalModule, MsalInterceptor } from '@azure/msal-angular';
    ```

    ### <a name="import-modules"></a>Modülleri içeri aktarma
    Aşağıdaki alma deyimlerini en üstüne ekleyin`src/app/app.component.ts`
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
        storeAuthStateInCookie: isIE, // set to true for IE 11
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

Son olarak, bir HTTP isteği yle bir kullanıcının profilini alın.

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
MSAL belirteçleri elde etmek `acquireTokenRedirect` `acquireTokenPopup`için `acquireTokenSilent`üç yöntem kullanır: , , ve . Ancak, önceki bölümde gösterildiği gibi, Açısal uygulamalar için bunun yerine Interceptor'ı kullanmanızı öneririz.

#### <a name="get-a-user-token-silently"></a>Kullanıcı belirtecini sessizce alma

Yöntem, `acquireTokenSilent` kullanıcı etkileşimi olmadan belirteç satın almalarını ve yenilemeyi işler. Veya yöntem ilk kez yürütüldükten sonra, `acquireTokenSilent` sonraki aramalarda korumalı kaynaklara erişmek için kullanılan belirteçleri elde etmek için yaygın olarak kullanılır. `loginPopup` `loginRedirect` Jeton istemek veya yenilemek için çağrılar sessizce yapılır.

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

API'nin erişim belirtecinde döndürülmek istenen kapsamları içerdiği yer. `scopes`

Örnek:

* `["user.read"]`Microsoft Graph için
* `["<Application ID URL>/scope"]`özel Web API'leri `api://<Application ID>/access_as_user`için (yani, )

#### <a name="get-a-user-token-interactively"></a>Etkileşimli olarak kullanıcı belirteci alma

Bazen kullanıcının Microsoft kimlik platformu bitiş noktasıyla etkileşimde olması gerekir. Örnek:

* Parolalarının süresi dolduğundan kullanıcıların kimlik bilgilerini yeniden girmeleri gerekebilir.
* Uygulamanız, kullanıcının onay ını alması gereken ek kaynak kapsamlarına erişim talep ediyor.
* İki faktörlü kimlik doğrulama gereklidir.

Çoğu uygulama için önerilen desen `acquireTokenSilent` önce aramak, sonra özel durumu `acquireTokenPopup` yakalamak `acquireTokenRedirect`ve ardından etkileşimli bir istek başlatmak için (veya) aramaktır.

Arama `acquireTokenPopup` sonuçları açılır pencerede olur. Alternatif olarak, `acquireTokenRedirect` kullanıcıları Microsoft kimlik platformu bitiş noktasına yönlendirir. Bu pencerede, kullanıcıların kimlik bilgilerini onaylamaları, gerekli kaynağa izin vermeleri veya iki faktörlü kimlik doğrulamayı tamamlamaları gerekir.

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

Bir kullanıcıyı oturum açmak için aşağıdaki kodu ekleyin.

```javascript
logout() {
  this.authService.logout();
}
```

#### <a name="add-ui"></a>UI Ekle
Açısal Malzeme bileşen kitaplığını kullanarak Kullanıcı Arası eklemenin basit bir örneği için [örnek uygulamayı](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular) kullanıma alın.

## <a name="test-your-code"></a>Kodunuza test etme

1.  Uygulama klasöründen komut satırı isteminde aşağıdaki komutları çalıştırarak bağlantı noktasını dinlemek için web sunucusunu başlatın:

    ```bash
    npm install
    npm start
    ```
1. Tarayıcınızda, web **http://localhost:4200** **http://localhost:{port}** sunucunuzun dinlediği *bağlantı noktasının* bulunduğu bağlantı noktasını girin veya girin.


### <a name="provide-consent-for-application-access"></a>Uygulama erişimi için onay sağlama

Başvurunuzda ilk oturum açtığınızda, bu uygulamanın profilinize erişme izni vermeniz ve oturum açmanız istenir:

!["İstenen İzinler" penceresi](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)



<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API' si, kullanıcının profilini okuması için *kullanıcının okuma* kapsamını gerektirir. Varsayılan olarak, bu kapsam kayıt portalına kayıtlı olan her uygulamaya otomatik olarak eklenir. Microsoft Graph için diğer API'lerin yanı sıra arka uç sunucunuz için özel API'ler ek kapsamlar gerektirebilir. Örneğin, Microsoft Graph API kullanıcının takvimlerini listelemek için *Takvimler.Oku* kapsamını gerektirir.

Bir uygulama bağlamında kullanıcının takvimlerine erişmek için, uygulama kayıt bilgilerine *Takvimler.Read* temsilcisi iznini ekleyin. Ardından, `acquireTokenSilent` aramaiçin *Takvimler.Oku* kapsamını ekleyin.

>[!NOTE]
>Kapsam sayısını artırdıkça kullanıcıdan ek izinler istenebilir.

Arka uç API'si kapsam gerektirmiyorsa (önerilmez), belirteçleri elde etmek için aramalarda *istemciyi* kapsam olarak kullanabilirsiniz.

<!--end-collapse-->

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Belgeler, SSS, sorunlar ve daha fazlası için MSAL repo'ya göz atın:

> [!div class="nextstepaction"]
> [MSAL.js GitHub repo](https://github.com/AzureAD/microsoft-authentication-library-for-js)
