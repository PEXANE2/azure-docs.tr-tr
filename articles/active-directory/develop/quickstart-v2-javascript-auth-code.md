---
title: Kimlik doğrulama koduna sahip JavaScript tek sayfalı uygulamalarda oturum açma kullanıcıları | Mavisi
titleSuffix: Microsoft identity platform
description: JavaScript uygulamasının Microsoft Identity platformunu kullanarak erişim belirteçleri gerektiren bir API 'YI nasıl çağırabileceğinizi öğrenin.
services: active-directory
author: hahamil
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 04/22/2020
ms.author: hahamil
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:JavaScript
ROBOTS: NOINDEX
ms.openlocfilehash: d362db3a51848603c78d663c5b628192ff028d02
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82209535"
---
# <a name="quickstart-sign-in-users-and-get-an-access-token-in-a-javascript-spa-using-the-auth-code-flow"></a>Hızlı başlangıç: Kullanıcı oturum açma ve kimlik doğrulama kod akışını kullanarak JavaScript SPA 'da erişim belirteci edinme 

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Önizlemeler, [ek kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma (GA) önce değişebilir.


Bu hızlı başlangıçta yetkilendirme kodu akışıyla MSAL. js 2,0 kullanılır. Örtük Flow ile MSAL. js 1,0 'yi kullanmak için [Bu hızlı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v2-javascript)başlangıcı görüntüleyin.

Bu hızlı başlangıçta, bir JavaScript tek sayfalı uygulamanın (SPA) kişisel hesap, iş hesabı ve okul hesapları kullanıcılarına nasıl oturum açabileceğinizi öğrenmek için bir kod örneği kullanırsınız. JavaScript SPA, Microsoft Graph API 'sini veya herhangi bir Web API 'sini çağırmak için bir erişim belirteci de alabilir. Örneğin bir çizim için [nasıl çalıştığını](#how-the-sample-works) görün.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz bir Azure aboneliği oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* [Node.js](https://nodejs.org/en/download/)
* [Visual Studio Code](https://code.visualstudio.com/download) (proje dosyalarını düzenlemek için)


> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-application"></a>Hızlı başlangıç uygulamanızı kaydedin ve indirin
> Hızlı başlangıç uygulamanızı başlatmak için aşağıdaki seçeneklerden birini kullanın.
>
> ### <a name="option-1-express-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Seçenek 1 (Express): uygulamanızı kaydedin ve otomatik olarak yapılandırın ve ardından kod örneğinizi indirin
>
> 1. [Azure Portal](https://portal.azure.com) oturum açın.
> 1. Hesabınız birden fazla kiracıya erişim veriyorsa, sağ üst kısımdaki hesabı seçin ve ardından Portal oturumunuzu kullanmak istediğiniz Azure Active Directory (Azure AD) kiracısına ayarlayın.
> 1. [Uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs)'nı seçin.
> 1. Uygulamanız için bir ad girin.
> 1. **Desteklenen hesap türleri**altında, **herhangi bir kurumsal dizin ve kişisel Microsoft hesabında hesaplar**' ı seçin.
> 1. **Kaydol**’u seçin.
> 1. Hızlı başlangıç bölmesine gidin ve yeni uygulamanızı indirip otomatik olarak yapılandırmak için yönergeleri izleyin.
>
> ### <a name="option-2-manual-register-and-manually-configure-your-application-and-code-sample"></a>Seçenek 2 (El Ile): uygulamanızı ve kod örneğinizi kaydetme ve el ile yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
>
> 1. [Azure Portal](https://portal.azure.com) oturum açın.
>
> 1. Hesabınız birden fazla kiracıya erişim veriyorsa, sağ üst kısımdaki hesabınızı seçin ve ardından Portal oturumunuzu kullanmak istediğiniz Azure AD kiracısı olarak ayarlayın.
> 1. [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908)'nı seçin.
> 1. **Yeni kayıt**seçeneğini belirleyin.
> 1. **Uygulamayı kaydet** sayfası görüntülendiğinde, uygulamanız için ad girin.
> 1. **Desteklenen hesap türleri**altında, **herhangi bir kurumsal dizin ve kişisel Microsoft hesabında hesaplar**' ı seçin.
> 1. **Kaydol**’u seçin. Uygulamaya **genel bakış** sayfasında, daha sonra kullanılmak üzere **uygulama (istemci) kimliği** değerini aklınızda edin.
> 1. Kayıtlı uygulamanın sol bölmesinde **kimlik doğrulaması**' nı seçin.
> 1. **Platform yapılandırması**altında **Platform Ekle**' yi seçin. Sol tarafta bir panel açılır. Burada, **tek sayfalı uygulamalar** bölgesini seçin.
> 1. Hala solda, **yeniden YÖNLENDIRME URI** değerini olarak `http://localhost:3000/`ayarlayın. 
> 1. **Yapılandır**'ı seçin.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>1. Adım: uygulamanızı Azure portal yapılandırma
> Bu hızlı başlangıçta kod örneğinin çalışmasını sağlamak için, bir `redirectUri` olarak `http://localhost:3000/`eklemeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişiklikleri benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-javascript/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-the-project"></a>2. Adım: Projeyi indirme

> [!div renderon="docs"]
> Projeyi Node. js kullanarak bir Web sunucusuyla çalıştırmak için, [temel proje dosyalarını indirin](https://github.com/Azure-Samples/ms-identity-javascript-v2/archive/quickstart.zip).

> [!div renderon="portal" class="sxs-lookup"]
> Node. js kullanarak projeyi bir Web sunucusu ile çalıştırma

> [!div renderon="portal" id="autoupdate" class="nextstepaction" class="sxs-lookup"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/ms-identity-javascript-v2/archive/quickstart.zip)

> [!div renderon="docs"]
> #### <a name="step-3-configure-your-javascript-app"></a>3. Adım: JavaScript uygulamanızı yapılandırma
>
> *Uygulama* klasöründe *AuthConfig. js*' yi düzenleyin `clientID`ve altında `authority` `redirectUri` `msalConfig`ve değerlerini ayarlayın.
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

> [!div renderon="portal" class="sxs-lookup"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`

> [!div renderon="docs"]
>
> Konumlar:
> - Enter_the_Application_Id_Here>, kaydettiğiniz uygulamanın **uygulama (istemci) kimliğidir** . * \<*
> - Enter_the_Cloud_Instance_Id_Here>Azure bulutunun örneğidir. * \<* Ana veya küresel Azure bulutu için yalnızca girmeniz *https://login.microsoftonline.com/* yeterlidir. **Ulusal** bulutlar (örneğin, Çin) için bkz. [Ulusal bulutlar](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud).
> - Enter_the_Tenant_info_here>aşağıdaki seçeneklerden birine ayarlanır: * \<*
>    - Uygulamanız *bu kuruluş dizinindeki hesapları*destekliyorsa, bu DEĞERI **Kiracı kimliği** veya **kiracı adı** (örneğin, *contoso.Microsoft.com*) ile değiştirin.
>    - Uygulamanız *herhangi bir kuruluş dizinindeki hesapları*destekliyorsa, bu değeri **kuruluşlar**ile değiştirin.
>    - Uygulamanız *herhangi bir kurumsal dizin ve kişisel Microsoft hesabında hesapları*destekliyorsa, bu değeri **ortak**ile değiştirin. *Yalnızca kişisel Microsoft hesaplarına*yönelik desteği kısıtlamak için bu değeri **tüketicilerle**değiştirin.
> - * \<Enter_the_Redirect_Uri_Here>*`http://localhost:3000`
> > [!TIP]
> > **Uygulama (istemci) kimliği**, **Dizin (kiracı) kimliği**ve **Desteklenen hesap türlerinin**değerlerini bulmak Için, Azure Portal uygulama kaydının **genel bakış** sayfasına gidin.
>
> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>3. Adım: uygulamanız yapılandırıldı ve çalıştırılmaya hazırlanıyor
> Projenizi uygulamanızın özelliklerinin değerleriyle yapılandırdık.

> [!div renderon="docs"]
>
> Daha sonra, aynı klasörde, `graphMeEndpoint` ve `graphMailEndpoint` `apiConfig` nesnesini ayarlamak için *graphconfig. js* dosyasını düzenleyin.
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
> Enter_the_Graph_Endpoint_Here>, API çağrılarının üzerinde verilecek uç noktadır. * \<* Ana veya genel Microsoft Graph API hizmeti için girin `https://graph.microsoft.com`. Daha fazla bilgi için bkz. [Ulusal bulut dağıtımı](https://docs.microsoft.com/graph/deployments).
>
> #### <a name="step-4-run-the-project"></a>4. Adım: projeyi çalıştırma

[Node. js](https://nodejs.org/en/download/)kullanarak projeyi bir Web sunucusuyla çalıştırın:

1. Sunucuyu başlatmak için, proje dizini içinden aşağıdaki komutları çalıştırın:
    ```bash
    npm install
    npm start
    ```
1. `http://localhost:3000/` adresine gidin.

1. Oturum açma işlemini başlatmak için **oturum aç** ' ı seçin ve ardından Microsoft Graph API 'yi çağırın.

    İlk kez oturum açtığınızda, uygulamanın profilinize erişmesine izin vermek için onayınızı sağlamanız ve oturumunuzu açmanız istenir. Başarıyla oturum açtıktan sonra, Kullanıcı profili bilgilerinizin sayfada görüntülenmesi gerekir.

## <a name="more-information"></a>Daha fazla bilgi

### <a name="how-the-sample-works"></a>Örneğin nasıl çalıştığı

![Örnek JavaScript SPA 'nın çalışması: 1. SPA, oturum açma işlemini başlatır. 2. SPA, Microsoft Identity platformundan bir KIMLIK belirteci alır. 3. SPA, belirteç alma 'yı çağırır. 4. Microsoft Identity platformu, SPA 'ya bir erişim belirteci döndürür. 5. SPA, Microsoft Graph API 'sine erişim belirteci ile HTTP GET isteği yapar. 6. Graph API SPA 'ya HTTP yanıtı döndürür.](media/quickstart-v2-javascript/javascriptspa-intro.svg)

### <a name="msaljs"></a>msal. js

MSAL. js kitaplığı, kullanıcıları imzalar ve Microsoft Identity platform tarafından korunan bir API 'ye erişmek için kullanılan belirteçleri ister. Örneğin *index. html* dosyası, kitaplığa bir başvuru içerir:

```html
<script type="text/javascript" src="https://alcdn.msftauth.net/lib/1.2.1/js/msal.js" integrity="sha384-9TV1245fz+BaI+VvCjMYL0YDMElLBwNS84v3mY57pXNOt6xcUYch2QLImaTahcOP" crossorigin="anonymous"></script>
```
> [!TIP]
> Önceki sürümü [msal. js yayınları](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases)altındaki en son yayınlanan sürümle değiştirebilirsiniz.

Alternatif olarak, Node. js yüklüyse, Node. js paket yöneticisini (NPM) kullanarak en son sürümü indirebilirsiniz:

```batch
npm install msal
```

## <a name="next-steps"></a>Sonraki adımlar

[Msal. js GitHub deposu](https://github.com/AzureAD/microsoft-authentication-library-for-js) ek kitaplık belgeleri içerir ve bir SSS ve sorun desteği sağlar.

Bu hızlı başlangıç için uygulama oluşturmaya yönelik daha ayrıntılı bir adım adım kılavuz için bkz.:

> [!div class="nextstepaction"]
> [Oturum açma ve MS Graf çağırma öğreticisi](https://docs.microsoft.com/azure/active-directory/develop/tutorial-v2-javascript-auth-code)
