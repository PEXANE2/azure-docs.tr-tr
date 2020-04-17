---
title: JavaScript tek sayfalı uygulamalarda oturum açın | Azure
titleSuffix: Microsoft identity platform
description: Bir JavaScript uygulamasının Microsoft kimlik platformlarını kullanarak erişim belirteçleri gerektiren bir API'yi nasıl çağırabileceğini öğrenin.
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 04/11/2019
ms.author: nacanuma
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:JavaScript
ms.openlocfilehash: 1d22f66ad5f7adf5bb8196c3e72a2a343f4558b0
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81536106"
---
# <a name="quickstart-sign-in-users-and-get-an-access-token-in-a-javascript-spa"></a>Quickstart: Kullanıcıları oturum açın ve JavaScript SPA'da bir erişim jetonu alın

Bu hızlı başlangıçta, bir JavaScript tek sayfalık uygulamanın (SPA) kişisel hesap, iş hesabı ve okul hesaplarının kullanıcılarını nasıl oturum açabileceğini öğrenmek için bir kod örneği kullanırsınız. JavaScript SPA, Microsoft Graph API'yi veya herhangi bir web API'sını aramak için erişim belirteci de alabilir. (Bkz. örnek bir resim için [nasıl çalışır?)](#how-the-sample-works)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* [Node.js](https://nodejs.org/en/download/)
* [Visual Studio Code](https://code.visualstudio.com/download) (proje dosyalarını düzenlemesi için)


> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-application"></a>Hızlı başlatma uygulamanızı kaydedin ve indirin
> Hızlı başlangıç uygulamanızı başlatmak için aşağıdaki seçeneklerden birini kullanın.
>
> ### <a name="option-1-express-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Seçenek 1 (Express): Uygulamanızı kaydedin ve otomatik olarak yapılandırın ve ardından kod örneğini indirin
>
> 1. Bir iş veya okul hesabı veya kişisel bir Microsoft hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
> 1. Hesabınız size birden fazla kiracıya erişim sağlıyorsa, sağ üstteki hesabı seçin ve ardından portal oturumunuzu kullanmak istediğiniz Azure Etkin Dizin (Azure AD) kiracısına ayarlayın.
> 1. Yeni Azure [portalı - Uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs) bölmesine gidin.
> 1. Uygulamanız için bir ad girin.
> 1. **Desteklenen hesap türleri**altında, tüm kuruluş **dizininde ve kişisel Microsoft hesaplarında Hesapları**seçin.
> 1. **Kaydol**’u seçin.
> 1. Yeni uygulamanızı indirmek ve otomatik olarak yapılandırmak için yönergeleri izleyin.
>
> ### <a name="option-2-manual-register-and-manually-configure-your-application-and-code-sample"></a>Seçenek 2 (Manuel): Başvurunuzu ve kod örneğini kaydedin ve el ile yapılandırın
>
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
>
> 1. Bir iş veya okul hesabı veya kişisel bir Microsoft hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
>
> 1. Hesabınız size birden fazla kiracıya erişim sağlıyorsa, sağ üstteki hesabınızı seçin ve ardından portal oturumunuzu kullanmak istediğiniz Azure AD kiracısına ayarlayın.
> 1. Geliştiriciler için Microsoft kimlik [platformuna](https://go.microsoft.com/fwlink/?linkid=2083908) gidin Uygulama kayıtları sayfası.
> 1. **Yeni kayıt**seçin.
> 1. **Uygulamayı kaydet** sayfası görüntülendiğinde, uygulamanız için ad girin.
> 1. **Desteklenen hesap türleri**altında, tüm kuruluş **dizininde ve kişisel Microsoft hesaplarında Hesapları**seçin.
> 1. **Kaydol**’u seçin. Uygulamaya **Genel Bakış** sayfasında, daha sonra kullanmak üzere **Uygulama (istemci) kimlik** değerine dikkat edin.
> 1. Bu hızlı başlatma, [Örtülü hibe akışının](v2-oauth2-implicit-grant-flow.md) etkinleştirilmesini gerektirir. Kayıtlı uygulamanın sol bölmesinde **Kimlik Doğrulama'yı**seçin.
> 1. **Platform Yapılandırmaları**altında platform **ekle'yi**seçin. Solda bir panel açılır. Burada, **Web Uygulamaları** bölgesini seçin.
> 1. Hala solda, **Uri'yi yeniden** yönlendirme `http://localhost:3000/`değerini . Ardından, **Access Token** ve **Id Token'ı**seçin.
> 1. **Yapılandır**'ı seçin.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Adım 1: Uygulamanızı Azure portalında yapılandırın
> Bu hızlı başlangıç çalışmasında kod örneğini yapmak `redirectUri` için, bir as `http://localhost:3000/` eklemeniz ve **Örtülü hibeyi**etkinleştirmeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişiklikleri benim için yapın]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-javascript/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-the-project"></a>2. Adım: Projeyi indirme

> [!div renderon="docs"]
> Node.js kullanarak bir web sunucusu ile proje çalıştırmak için, [çekirdek proje dosyalarını indirin.](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip)

> [!div renderon="portal"]
> Node.js kullanarak projeyi bir web sunucusuyla çalıştırın

> [!div renderon="portal" id="autoupdate" class="nextstepaction"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip)

> [!div renderon="docs"]
> #### <a name="step-3-configure-your-javascript-app"></a>Adım 3: JavaScript uygulamanızı yapılandırın
>
> *JavaScriptSPA* klasöründe *authConfig.js'yi*edin `clientID`ve `authority` `redirectUri` altında `msalConfig`, ve değerleri ayarlayın.
>
> ```javascript
>
>  // Config object to be passed to Msal on creation
>  const msalConfig = {
>    auth: {
>      clientId: "Enter_the_Application_Id_Here",
>      authority: "Enter_the_Cloud_Instance_Id_HereEnter_the_Tenant_Info_Here",
>      redirectUri: "Enter_the_Redirect_Uri_Here",
>    },
>    cache: {
>      cacheLocation: "sessionStorage", // This configures where your cache will be stored
>      storeAuthStateInCookie: false, // Set this to "true" if you are having issues on IE11 or Edge
>    }
>  };
>
>```

> [!div renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`

> [!div renderon="docs"]
>
> Konumlar:
> - Enter_the_Application_Id_Here>, kaydettiğiniz uygulamanın **Uygulama (istemci) kimliğidir.** * \<*
> - Enter_the_Cloud_Instance_Id_Here>, Azure bulutunun bir örneğidir. * \<* Ana veya küresel Azure bulutu *https://login.microsoftonline.com*için ' girin. **Ulusal** bulutlar (örneğin, Çin) için [Ulusal bulutlara](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud)bakın.
> - Enter_the_Tenant_info_here>aşağıdaki seçeneklerden birine ayarlanmıştır: * \<*
>    - Uygulamanız *bu kuruluş dizinindeki hesapları*destekliyorsa, bu değeri Kiracı Kimliği veya **Kiracı** **adı** ile değiştirin (örneğin, *contoso.microsoft.com).*
>    - Uygulamanız *herhangi bir kuruluş dizinindeki hesapları destekliyorsa,* bu değeri **kuruluşlarla**değiştirin.
>    - Uygulamanız *herhangi bir kuruluş dizinindeki ve kişisel Microsoft hesaplarındaki hesapları*destekliyorsa, bu değeri **ortak**olanla değiştirin. Desteği yalnızca *kişisel Microsoft hesaplarına*kısıtlamak için, bu değeri **tüketicilerle**değiştirin.
>
> > [!TIP]
> > **Uygulama (istemci) Kimliği**, **Dizin (kiracı) Kimliği** ve **Desteklenen hesap türleri** değerlerini bulmak için Azure portalında uygulamanın **Genel bakış** sayfasına gidin.
>
> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Adım 3: Uygulamanız yapılandırıldı ve çalışmaya hazır
> Projenizi uygulamanızın özellikleriyle yapılandırdık.

> [!div renderon="docs"]
>
> Daha sonra, hala aynı klasörde, *graphConfig.js* `graphMeEndpoint` dosyasını `apiConfig` ve nesne için ayarlamak `graphMeEndpoint` için edin.
> ```javascript
>   // Add here the endpoints for MS Graph API services you would like to use.
>   const graphConfig = {
>     graphMeEndpoint: "Enter_the_Graph_Endpoint_Herev1.0/me",
>     graphMailEndpoint: "Enter_the_Graph_Endpoint_Herev1.0/me/messages"
>   };
>
>   // Add here scopes for access token to be used at MS Graph API endpoints.
>   const tokenRequest = {
>       scopes: ["Mail.Read"]
>   };
> ```
>

> [!div renderon="docs"]
>
> Konumlar:
> - Enter_the_Graph_Endpoint_Here>, API çağrılarının karşısına konulacak son noktadır. * \<* Ana veya genel Microsoft Graph API `https://graph.microsoft.com`hizmeti için, sadece girin. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/graph/deployments)
>
> #### <a name="step-4-run-the-project"></a>Adım 4: Projeyi çalıştırın

[Node.js](https://nodejs.org/en/download/)kullanarak bir web sunucusu ile proje çalıştırın:

1. Sunucuyu başlatmak için proje dizininden aşağıdaki komutu çalıştırın:
    ```batch
    npm install
    npm start
    ```
1. Bir web tarayıcısı `http://localhost:3000/`açın ve .

1. Oturum açma'yı başlatmak için **Oturum Aç'ı** seçin ve ardından Microsoft Graph API'yi arayın.

Tarayıcı uygulamayı yüklendikten sonra **Oturum Aç'ı**seçin. İlk oturum açtığınızda, uygulamanın profilinize erişmesine ve oturum açmasına izin vermek için izninizi sağlamanız istenir. Başarılı bir şekilde oturum açıldıktan sonra, kullanıcı profili bilgileriniz sayfada görüntülenmelidir.

## <a name="more-information"></a>Daha fazla bilgi

### <a name="how-the-sample-works"></a>Örnek nasıl çalışır?

![Bu hızlı başlatmadaki örnek uygulama nasıl çalışır?](media/quickstart-v2-javascript/javascriptspa-intro.svg)

### <a name="msaljs"></a>msal.js

MSAL kitaplığı kullanıcıları işaretler ve Microsoft kimlik platformu tarafından korunan bir API'ye erişmek için kullanılan belirteçleri ister. Quickstart *index.html* dosyası kitaplık için bir başvuru içerir:

```html
<script type="text/javascript" src="https://alcdn.msftauth.net/lib/1.2.1/js/msal.js" integrity="sha384-9TV1245fz+BaI+VvCjMYL0YDMElLBwNS84v3mY57pXNOt6xcUYch2QLImaTahcOP" crossorigin="anonymous"></script>
```
> [!TIP]
> Önceki sürümü [MSAL.js sürümleri](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases)altında en son yayımlanan sürümü ile değiştirebilirsiniz.

Alternatif olarak, Node.js yüklü varsa, Node.js Package Manager (npm) üzerinden en son sürümü indirebilirsiniz:

```batch
npm install msal
```

### <a name="msal-initialization"></a>MSAL başlatma

Hızlı başlatma kodu, MSAL kitaplığını niçin başlatılabildiğini de gösterir:

```javascript
  // Config object to be passed to Msal on creation
  const msalConfig = {
    auth: {
      clientId: "75d84e7a-40bx-f0a2-91b9-0c82d4c556aa", // this is a fake id
      authority: "https://login.microsoftonline.com/common",
      redirectUri: "http://localhost:3000/",
    },
    cache: {
      cacheLocation: "sessionStorage", // This configures where your cache will be stored
      storeAuthStateInCookie: false, // Set this to "true" if you are having issues on IE11 or Edge
    }
  };

const myMSALObj = new Msal.UserAgentApplication(msalConfig);
```

> |Konum  |  |
> |---------|---------|
> |`clientId`     | Azure portalına kayıtlı uygulamanın uygulama kimliği.|
> |`authority`    | (İsteğe bağlı) Yapılandırma bölümünde daha önce açıklandığı gibi hesap türlerini destekleyen yetkili URL. Varsayılan yetki. `https://login.microsoftonline.com/common` |
> |`redirectUri`     | Uygulama kaydının yapılandırılmış yanıt/yönlendirmeUri. Bu durumda, `http://localhost:3000/`. |
> |`cacheLocation`  | (İsteğe bağlı) Auth durumu için tarayıcı depolamasını ayarlar. Varsayılan olan sessionStorage'dır.   |
> |`storeAuthStateInCookie`  | (İsteğe bağlı) Kimlik doğrulama isteği durumunu depolayan kitaplık, tarayıcı tanımlama bilgilerinde kimlik doğrulama akışlarının doğrulanması için gereklidir. Bu çerez, bilinen bazı [sorunları](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues)azaltmak için IE ve Edge tarayıcıları için ayarlanmıştır. |

Kullanılabilir yapılandırılabilir seçenekler hakkında daha fazla bilgi için [bkz.](msal-js-initializing-client-applications.md)

### <a name="sign-in-users"></a>Oturum aç kullanıcıları

Aşağıdaki kod snippet kullanıcıları nasıl oturum açılabildiğini gösterir:

```javascript
// Add scopes for the id token to be used at Microsoft identity platform endpoints.
const loginRequest = {
    scopes: ["openid", "profile", "User.Read"],
};

myMSALObj.loginPopup(loginRequest)
    .then((loginResponse) => {
    //Login Success callback code here
}).catch(function (error) {
    console.log(error);
});
```

> |Konum  |  |
> |---------|---------|
> | `scopes`   | (İsteğe bağlı) Oturum açma zamanında kullanıcı onayı için istenen kapsamları içerir. Örneğin, `[ "user.read" ]` Microsoft Graph `[ "<Application ID URL>/scope" ]` veya özel web API'leri için (diğer bir şey). `api://<Application ID>/access_as_user` |

> [!TIP]
> Alternatif olarak, geçerli sayfayı `loginRedirect` açılır pencere yerine oturum açma sayfasına yönlendirmek için yöntemi kullanmak isteyebilirsiniz.

### <a name="request-tokens"></a>İstek belirteçleri

MSAL belirteçleri elde etmek `acquireTokenRedirect` `acquireTokenPopup`için üç yöntem kullanır: , , ve`acquireTokenSilent`

#### <a name="get-a-user-token-silently"></a>Kullanıcı belirtecini sessizce alma

Yöntem, `acquireTokenSilent` herhangi bir kullanıcı etkileşimi olmadan belirteç satın almave yenileme işler. Veya yöntem ilk kez yürütüldükten sonra, `acquireTokenSilent` sonraki aramalar için korumalı kaynaklara erişmek için kullanılan belirteçleri elde etmek için yaygın olarak kullanılan yöntemdir. `loginPopup` `loginRedirect` Jeton istemek veya yenilemek için çağrılar sessizce yapılır.

```javascript

const tokenRequest = {
    scopes: ["Mail.Read"]
};

myMSALObj.acquireTokenSilent(tokenRequest)
    .then((tokenResponse) => {
        // Callback code here
        console.log(tokenResponse.accessToken);
    }).catch((error) => {
        console.log(error);
    });
```

> |Konum  |  |
> |---------|---------|
> | `scopes`   | API için erişim belirteci döndürülmek istenen kapsamları içerir. Örneğin, `[ "mail.read" ]` Microsoft Graph `[ "<Application ID URL>/scope" ]` veya özel web API'leri için (diğer bir şey). `api://<Application ID>/access_as_user`|

#### <a name="get-a-user-token-interactively"></a>Etkileşimli olarak kullanıcı belirteci alma

Kullanıcıları Microsoft kimlik platformu bitiş noktasıyla etkileşime girmeye zorlamanız gereken durumlar vardır. Örneğin:
* Parolalarının süresi dolduğundan kullanıcıların kimlik bilgilerini yeniden girmeleri gerekebilir.
* Uygulamanız, kullanıcının onay ını alması gereken ek kaynak kapsamlarına erişim talep ediyor.
* İki faktörlü kimlik doğrulama gereklidir.

Çoğu uygulama için önerilen olağan desen `acquireTokenSilent` önce aramak, sonra özel `acquireTokenPopup` durumu `acquireTokenRedirect`yakalamak ve ardından etkileşimli bir istek başlatmak için (veya) aramaktır.

Oturum `acquireTokenPopup` açmak için açılan pencerede sonuçları arama. (Veya `acquireTokenRedirect` kullanıcıları Microsoft kimlik platformu bitiş noktasına yönlendirmeyle sonuçlanır.) Bu pencerede, kullanıcıların kimlik bilgilerini onaylayarak, gerekli kaynağa onay vererek veya iki faktörlü kimlik doğrulamasını tamamlayarak etkileşimde olmaları gerekir.

```javascript
// Add here scopes for access token to be used at MS Graph API endpoints.
const tokenRequest = {
    scopes: ["Mail.Read"]
};

myMSALObj.acquireTokenPopup(requestObj)
    .then((tokenResponse) => {
        // Callback code here
        console.log(tokenResponse.accessToken);
    }).catch((error) => {
        console.log(error);
    });
```

> [!NOTE]
> Bu hızlı başlatma, `acquireTokenRedirect` internet explorer tarafından açılır pencerelerin işlenmesiyle ilgili bilinen bir [sorun](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) nedeniyle Microsoft Internet Explorer ile `loginRedirect` ve yöntemleri kullanır.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç için uygulama oluşturma konusunda daha ayrıntılı bir adım adım kılavuz için bkz:

> [!div class="nextstepaction"]
> [Oturum açma ve MS Grafiği'ni arama eğitimi](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-javascriptspa)

Belgeler, SSS, sorunlar ve daha fazlası için MSAL repo'ya göz atmak için bkz:

> [!div class="nextstepaction"]
> [MSAL.js GitHub repo](https://github.com/AzureAD/microsoft-authentication-library-for-js)
