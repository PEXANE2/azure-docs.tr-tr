---
title: JavaScript tek sayfalı uygulamalarda oturum açma kullanıcıları | Mavisi
titleSuffix: Microsoft identity platform
description: JavaScript uygulamasının Microsoft Identity platformunu kullanarak erişim belirteçleri gerektiren bir API 'YI nasıl çağırabileceğinizi öğrenin.
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:JavaScript
ms.topic: quickstart
ms.workload: identity
ms.date: 04/11/2019
ms.author: nacanuma
ms.openlocfilehash: 9077d5c471911c9967c327c457d683b06856b920
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78249060"
---
# <a name="quickstart-sign-in-users-and-get-an-access-token-in-a-javascript-spa"></a>Hızlı başlangıç: bir JavaScript SPA 'da Kullanıcı oturumu açma ve erişim belirteci edinme

Bu hızlı başlangıçta, bir JavaScript tek sayfalı uygulamanın (SPA) kişisel hesap, iş hesabı ve okul hesapları kullanıcılarına nasıl oturum açabileceğinizi öğrenmek için bir kod örneği kullanırsınız. JavaScript SPA, Microsoft Graph API 'sini veya herhangi bir Web API 'sini çağırmak için bir erişim belirteci de alabilir. (Örneğin bir çizim için [nasıl çalıştığını](#how-the-sample-works) görün.)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* [Node.js](https://nodejs.org/en/download/)
* [Visual Studio Code](https://code.visualstudio.com/download) (proje dosyalarını düzenlemek için)


> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-application"></a>Hızlı başlangıç uygulamanızı kaydedin ve indirin
> Hızlı başlangıç uygulamanızı başlatmak için aşağıdaki seçeneklerden birini kullanın.
>
> ### <a name="option-1-express-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Seçenek 1 (Express): uygulamanızı kaydedin ve otomatik olarak yapılandırın ve ardından kod örneğinizi indirin
>
> 1. Bir iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak [Azure Portal](https://portal.azure.com) oturum açın.
> 1. Hesabınız birden fazla kiracıya erişim veriyorsa, sağ üst kısımdaki hesabı seçin ve ardından Portal oturumunuzu kullanmak istediğiniz Azure Active Directory (Azure AD) kiracısına ayarlayın.
> 1. Yeni [Azure Portal-uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs) bölmesine gidin.
> 1. Uygulamanız için bir ad girin. 
> 1. **Desteklenen hesap türleri**altında, **herhangi bir kurumsal dizin ve kişisel Microsoft hesabında hesaplar**' ı seçin.
> 1. **Kaydol**’u seçin.
> 1. Yeni uygulamanızı indirip otomatik olarak yapılandırmak için yönergeleri izleyin.
>
> ### <a name="option-2-manual-register-and-manually-configure-your-application-and-code-sample"></a>Seçenek 2 (El Ile): uygulamanızı ve kod örneğinizi kaydetme ve el ile yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1\. Adım: Uygulamanızı kaydetme
>
> 1. Bir iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak [Azure Portal](https://portal.azure.com) oturum açın.
>
> 1. Hesabınız birden fazla kiracıya erişim veriyorsa, sağ üst kısımdaki hesabınızı seçin ve ardından Portal oturumunuzu kullanmak istediğiniz Azure AD kiracısı olarak ayarlayın.
> 1. Geliştiriciler için Microsoft Identity platformu [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasına gidin.
> 1. **Yeni kayıt**seçeneğini belirleyin.
> 1. **Bir uygulamayı kaydet** sayfası göründüğünde, uygulamanız için bir ad girin.
> 1. **Desteklenen hesap türleri**altında, **herhangi bir kurumsal dizin ve kişisel Microsoft hesabında hesaplar**' ı seçin.
> 1. **Kaydol**’u seçin. Uygulamaya **genel bakış** sayfasında, daha sonra kullanılmak üzere **uygulama (istemci) kimliği** değerini aklınızda edin.
> 1. Bu hızlı başlangıç, [örtük izin akışının](v2-oauth2-implicit-grant-flow.md) etkinleştirilmesini gerektirir. Kayıtlı uygulamanın sol bölmesinde **kimlik doğrulaması**' nı seçin.
> 1. **Platform yapılandırması**altında **Platform Ekle**' yi seçin. Sol tarafta bir panel açılır. Burada **Web uygulamaları** bölgesini seçin.
> 1. Hala solda, **yeniden YÖNLENDIRME URI** değerini `http://localhost:3000/`olarak ayarlayın. Ardından, **erişim belirteci** ve **kimlik belirteci**' ni seçin.
> 1. **Yapılandır**' ı seçin.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>1\. Adım: uygulamanızı Azure portal yapılandırma
> Bu hızlı başlangıçta kod örneğini yapmak için, `http://localhost:3000/` olarak `redirectUri` eklemeli ve **örtük izni**etkinleştirmeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişiklikleri benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-javascript/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-the-project"></a>2\. Adım: Projeyi indirme

> [!div renderon="docs"]
> Projeyi Node. js kullanarak bir Web sunucusuyla çalıştırmak için, [temel proje dosyalarını indirin](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip).

> [!div renderon="portal"]
> Node. js kullanarak projeyi bir Web sunucusu ile çalıştırma

> [!div renderon="portal" id="autoupdate" class="nextstepaction"]
> [Kod örneğini indirin]()

> [!div renderon="docs"]
> #### <a name="step-3-configure-your-javascript-app"></a>3\. Adım: JavaScript uygulamanızı yapılandırma
>
> *Javascriptspa* klasöründe, *AuthConfig. js*' yi düzenleyin ve `msalConfig`altındaki `clientID` ve `authority` değerlerini ayarlayın.
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
>      forceRefresh: false // Set this to "true" to skip a cached token and go to the server to get a new
>    }
>  };  
> ```

> [!div renderon="portal"]
> > [!NOTE]
> > Enter_the_Supported_Account_Info_Here

> [!div renderon="docs"]
>
> Konumlar:
> - *\<Enter_the_Application_Id_Here >* , kaydettiğiniz uygulamanın **uygulama (istemci) kimliğidir** .
> - *\<Enter_the_Cloud_Instance_Id_Here >* Azure bulutunun örneğidir. Ana veya küresel Azure bulutu için *https://login.microsoftonline.com* girmeniz yeterlidir. **Ulusal** bulutlar (örneğin, Çin) için bkz. [Ulusal bulutlar](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud).
> - *\<Enter_the_Tenant_info_here >* aşağıdaki seçeneklerden birine ayarlanır:
>    - Uygulamanız *bu kuruluş dizinindeki hesapları*destekliyorsa, bu DEĞERI **Kiracı kimliği** veya **kiracı adı** (örneğin, *contoso.Microsoft.com*) ile değiştirin.
>    - Uygulamanız *herhangi bir kuruluş dizinindeki hesapları*destekliyorsa, bu değeri **kuruluşlar**ile değiştirin.
>    - Uygulamanız *herhangi bir kurumsal dizin ve kişisel Microsoft hesabında hesapları*destekliyorsa, bu değeri **ortak**ile değiştirin. *Yalnızca kişisel Microsoft hesaplarına*yönelik desteği kısıtlamak için bu değeri **tüketicilerle**değiştirin.
>
> > [!TIP]
> > **Uygulama (istemci) Kimliği**, **Dizin (kiracı) Kimliği** ve **Desteklenen hesap türleri** değerlerini bulmak için Azure portalında uygulamanın **Genel bakış** sayfasına gidin.
>
> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>3\. Adım: uygulamanız yapılandırıldı ve çalıştırılmaya hazırlanıyor
> Projenizi uygulamanızın özelliklerinin değerleriyle yapılandırdık. 

> [!div renderon="docs"]
> #### <a name="step-4-run-the-project"></a>4\. Adım: projeyi çalıştırma

[Node. js](https://nodejs.org/en/download/)kullanarak projeyi bir Web sunucusuyla çalıştırın:

1. Sunucuyu başlatmak için, proje dizininden aşağıdaki komutu çalıştırın:
    ```batch
    npm install
    npm start
    ```
1. Bir Web tarayıcısı açın ve `http://localhost:3000/`gidin.

1. Oturum açmak için **oturum aç** ' ı seçin ve ardından Microsoft Graph API 'yi çağırın.

Tarayıcı uygulamayı yükledikten sonra **oturum aç**' ı seçin. İlk kez oturum açtığınızda, uygulamanın profilinize erişmesine ve oturumunuzu açmasını sağlamak için onayınızı vermeniz istenir. Başarıyla oturum açtıktan sonra, Kullanıcı profili bilgilerinizin sayfada görüntülenmesi gerekir.

## <a name="more-information"></a>Daha fazla bilgi

### <a name="how-the-sample-works"></a>Örneğin nasıl çalıştığı

![Bu hızlı başlangıçta örnek uygulamanın nasıl çalıştığı](media/quickstart-v2-javascript/javascriptspa-intro.svg)

### <a name="msaljs"></a>msal. js

MSAL kitaplığı, kullanıcıları imzalar ve Microsoft Identity platform tarafından korunan bir API 'ye erişmek için kullanılan belirteçleri ister. Hızlı Başlangıç *dizini. html* dosyası kitaplığa bir başvuru içerir:

```html
<script type="text/javascript" src="https://alcdn.msftauth.net/lib/1.2.1/js/msal.js" integrity="sha384-9TV1245fz+BaI+VvCjMYL0YDMElLBwNS84v3mY57pXNOt6xcUYch2QLImaTahcOP" crossorigin="anonymous"></script>
```
> [!TIP]
> Önceki sürümü [msal. js yayınları](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases)altındaki en son yayınlanan sürümle değiştirebilirsiniz.


Alternatif olarak, Node. js yüklüyse, Node. js Paket Yöneticisi (NPM) üzerinden en son sürümü indirebilirsiniz:

```batch
npm install msal
```

### <a name="msal-initialization"></a>MSAL başlatma

Hızlı başlangıç kodu ayrıca MSAL kitaplığının nasıl başlatılacağını gösterir:

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
      forceRefresh: false // Set this to "true" to skip a cached token and go to the server to get a new
    }
  };  

const myMSALObj = new Msal.UserAgentApplication(msalConfig);
```

> |Konum  |  |
> |---------|---------|
> |`clientId`     | Azure portal kayıtlı uygulamanın uygulama KIMLIĞI.|
> |`authority`    | Seçim Daha önce yapılandırma bölümünde açıklandığı gibi, hesap türlerini destekleyen yetkili URL 'SI. Varsayılan yetkili `https://login.microsoftonline.com/common`. |
> |`redirectUri`     | Uygulama kaydının yapılandırılmış yanıtı/redirectUri. Bu durumda `http://localhost:3000/`. |
> |`cacheLocation`  | Seçim Kimlik doğrulama durumu için tarayıcı depolamayı ayarlar. Varsayılan değer sessionStorage ' dır.   |
> |`storeAuthStateInCookie`  | Seçim Tarayıcı tanımlama bilgilerinde kimlik doğrulama akışlarının doğrulanması için gerekli olan kimlik doğrulama isteği durumunu depolayan kitaplık. Bu tanımlama bilgisi, bazı [bilinen sorunları](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues)azaltmak üzere IE ve Edge tarayıcıları için ayarlanır. |

Kullanılabilir yapılandırılabilir seçenekler hakkında daha fazla bilgi için bkz. [istemci uygulamalarını başlatma](msal-js-initializing-client-applications.md).

### <a name="sign-in-users"></a>Oturum açma kullanıcıları

Aşağıdaki kod parçacığı, kullanıcıların oturum açma şeklini gösterir:

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
> | `scopes`   | Seçim Oturum açma sırasında kullanıcı onayı için istenen kapsamları içerir. Örneğin, özel Web API 'Leri için Microsoft Graph veya `[ "<Application ID URL>/scope" ]` için `[ "user.read" ]` (yani, `api://<Application ID>/access_as_user`). |

> [!TIP]
> Alternatif olarak, geçerli sayfayı bir açılan pencere yerine oturum açma sayfasına yönlendirmek için `loginRedirect` yöntemini kullanmak isteyebilirsiniz.

### <a name="request-tokens"></a>İstek belirteçleri

MSAL belirteçleri elde etmek için üç yöntem kullanır: `acquireTokenRedirect`, `acquireTokenPopup`ve `acquireTokenSilent`

#### <a name="get-a-user-token-silently"></a>Kullanıcı belirtecini sessizce alma

`acquireTokenSilent` yöntemi, Kullanıcı etkileşimi olmadan belirteç alma ve yenileme işlemleri gerçekleştirir. `loginRedirect` veya `loginPopup` yöntemi ilk kez yürütüldükten sonra, `acquireTokenSilent`, sonraki çağrılar için korunan kaynaklara erişmek için kullanılan belirteçleri elde etmek için yaygın olarak kullanılan yöntemdir. Belirteçleri istek veya yenileme çağrıları sessizce yapılır.

```javascript
// Add scopes for the access token to be used at Microsoft Graph API endpoints.
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
> | `scopes`   | API için erişim belirtecine döndürülmek istenen kapsamları içerir. Örneğin, özel Web API 'Leri için Microsoft Graph veya `[ "<Application ID URL>/scope" ]` için `[ "mail.read" ]` (yani, `api://<Application ID>/access_as_user`).|

#### <a name="get-a-user-token-interactively"></a>Etkileşimli olarak kullanıcı belirteci alma

Kullanıcıları Microsoft Identity platform uç noktasıyla etkileşimde bulunmak için zorlamanız gereken durumlar vardır. Örnek:
* Parolasının süresi sona erdiği için kullanıcıların kimlik bilgilerini yeniden girmesi gerekebilir.
* Uygulamanız, kullanıcının onaylaması gereken ek kaynak kapsamlarına erişim istiyor.
* İki öğeli kimlik doğrulaması gereklidir.

Çoğu uygulama için önerilen desenler öncelikle `acquireTokenSilent` çağırıla, sonra özel durumu yakalamalı ve sonra etkileşimli bir istek başlatmak için `acquireTokenPopup` (veya `acquireTokenRedirect`) çağırmalıdır.

`acquireTokenPopup` çağırmak, oturum açmak için bir açılan pencerede sonuçlanır. (Veya `acquireTokenRedirect`, kullanıcıları Microsoft Identity platform uç noktasına yönlendirmeye neden olur.) Bu pencerede, kullanıcıların kimlik bilgilerini onaylayarak, gerekli kaynağa onay vererek veya iki öğeli kimlik doğrulamasını tamamlayarak etkileşimde olmaları gerekir.

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
> Bu hızlı başlangıç, Internet Explorer tarafından açılan pencerelerin işlenmesiyle ilgili [bilinen bir sorun](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) nedeniyle Microsoft Internet explorer ile `loginRedirect` ve `acquireTokenRedirect` yöntemlerini kullanır.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç için uygulama oluşturmaya yönelik daha ayrıntılı bir adım adım kılavuz için bkz.:

> [!div class="nextstepaction"]
> [Oturum açma ve MS Graf çağırma öğreticisi](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-javascriptspa)

Belgeler, SSS, sorunlar ve daha fazlası için MSAL depoya göz atın, bkz:

> [!div class="nextstepaction"]
> [MSAL. js GitHub deposu](https://github.com/AzureAD/microsoft-authentication-library-for-js)

Microsoft Identity platformunu geliştirmemize yardımcı olun. Kısa bir iki sorulık anketi tamamlayarak düşüncelerinizi bize söyleyin.

> [!div class="nextstepaction"]
> [Microsoft Identity platform Anketi](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)
