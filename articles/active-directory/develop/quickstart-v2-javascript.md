---
title: Microsoft Identity platform JavaScript hızlı başlangıç-Azure
description: JavaScript uygulamalarının Microsoft Identity platform kullanarak erişim belirteçleri gerektiren bir API 'YI nasıl çağırabileceğinizi öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: nacanuma
ms.collection: M365-identity-device-management
ms.openlocfilehash: 20b8780ae0c24ca2b037211f7cb977f0cacee0f9
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68325367"
---
# <a name="quickstart-sign-in-users-and-acquire-an-access-token-from-a-javascript-single-page-application"></a>Hızlı Başlangıç: Kullanıcıların oturum açması ve JavaScript tek sayfalı uygulamasından erişim belirteci alma

Bu hızlı başlangıçta, JavaScript tek sayfalı uygulamanın (SPA) kişisel hesap, iş hesabı ve okul hesaplarının kullanıcılarına nasıl oturum açabileceğinizi gösteren bir kod örneği kullanmayı öğreneceksiniz. JavaScript SPA, Microsoft Graph API 'sini veya herhangi bir Web API 'sini çağırmak için bir erişim belirteci de alabilir.

![Bu hızlı başlangıçta örnek uygulamanın nasıl çalıştığı](media/quickstart-v2-javascript/javascriptspa-intro.svg)

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta aşağıdaki kurulum gereklidir:
* Projeyi bir Node. js sunucusuyla çalıştırmak için [Node. js](https://nodejs.org/en/download/)' yi indirip yükleyin.
* Proje dosyalarını düzenlemek için [Visual Studio Code](https://code.visualstudio.com/download)indirip yükleyin.
* Projeyi bir Visual Studio çözümü olarak çalıştırmak için [Visual studio 2019](https://visualstudio.microsoft.com/downloads/)' i indirip yükleyin.

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-application"></a>Hızlı başlangıç uygulamanızı kaydedin ve indirin
> Hızlı başlangıç uygulamanızı başlatmak için aşağıdaki seçeneklerden birini kullanın.
>
> ### <a name="option-1-express-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Seçenek 1 (Express): Uygulamanızı kaydedin ve otomatik olarak yapılandırın ve ardından kod örneğinizi indirin
>
> 1. Bir iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak [Azure Portal](https://portal.azure.com) oturum açın.
> 1. Hesabınız birden fazla kiracıya erişim veriyorsa, sağ üst kısımdaki hesabı seçin ve ardından Portal oturumunuzu kullanmak istediğiniz Azure Active Directory (Azure AD) kiracısına ayarlayın.
> 1. Yeni [Azure Portal-uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs) bölmesine gidin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**' i seçin.
> 1. Yeni uygulamanızı indirip otomatik olarak yapılandırmak için yönergeleri izleyin.
>
> ### <a name="option-2-manual-register-and-manually-configure-your-application-and-code-sample"></a>Seçenek 2 (El Ile): Uygulamanızı ve kod örneğinizi kaydetme ve el ile yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1\. adım: Uygulamanızı kaydetme
>
> 1. Bir iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak [Azure Portal](https://portal.azure.com) oturum açın.
>
> 1. Hesabınız birden fazla kiracıya erişim veriyorsa, sağ üst kısımdaki hesabınızı seçin ve ardından Portal oturumunuzu kullanmak istediğiniz Azure AD kiracısı olarak ayarlayın.
> 1. Geliştiriciler için Microsoft Identity platformu [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasına gidin.
> 1. **Yeni kayıt**seçeneğini belirleyin.
> 1. **Bir uygulamayı kaydet** sayfası göründüğünde, uygulamanız için bir ad girin.
> 1. **Desteklenen hesap türleri**altında, **herhangi bir kurumsal dizin ve kişisel Microsoft hesabında hesaplar**' ı seçin.
> 1. **Yeniden yönlendirme URI 'si** bölümünde, açılan listede, **Web** platformunu seçin ve değerini olarak `http://localhost:30662/`ayarlayın.
> 1. **Kaydol**’u seçin. Uygulamaya **genel bakış** sayfasında, daha sonra kullanılmak üzere **uygulama (istemci) kimliği** değerini aklınızda edin.
> 1. Bu hızlı başlangıç, [örtük izin akışının](v2-oauth2-implicit-grant-flow.md) etkinleştirilmesini gerektirir. Kayıtlı uygulamanın sol bölmesinde **kimlik doğrulaması**' nı seçin.
> 1. **Gelişmiş ayarlar** bölümünde, **örtük izin**' ın altında, **Kimlik belirteçleri** ve **erişim belirteçleri** onay kutularını seçin. KIMLIK belirteçleri ve erişim belirteçleri gereklidir, çünkü bu uygulamanın kullanıcıları oturum açması ve bir API çağırması gerekir.
> 1. Bölmenin en üstünde **Kaydet**' i seçin.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>1\. adım: Uygulamanızı Azure portal yapılandırma
> Bu hızlı başlangıçta çalışması için kod örneği için, bir yeniden yönlendirme URI 'si `http://localhost:30662/` eklemeniz ve **örtülü izni**etkinleştirmeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişiklikleri benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-javascript/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-the-project"></a>2\. adım: Projeyi indirin

Geliştirme ortamınız için uygun olan seçeneği seçin:

* Projeyi Node. js kullanarak bir Web sunucusuyla çalıştırmak için, [temel proje dosyalarını indirin](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip). Dosyaları açmak için [Visual Studio Code](https://code.visualstudio.com/)gibi bir düzenleyici kullanın.

* Seçim Projeyi IIS sunucusuyla çalıştırmak için [Visual Studio projesini indirin](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip). ZIP dosyasını yerel bir klasöre (örneğin, *C:\Azure-Samples*) ayıklayın.



#### <a name="step-3-configure-your-javascript-app"></a>3\. adım: JavaScript uygulamanızı yapılandırma

> [!div renderon="docs"]
> *Javascriptspa* klasöründe *index. html*dosyasını düzenleyin `clientID` ve altındaki `msalConfig`ve `authority` değerlerini ayarlayın.

> [!div class="sxs-lookup" renderon="portal"]
> *Javascriptspa* klasöründe, *index. html*dosyasını düzenleyin ve aşağıdaki kodla değiştirin `msalConfig` :

```javascript
var msalConfig = {
    auth: {
        clientId: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.com/Enter_the_Tenant_info_here"
    },
    cache: {
        cacheLocation: "localStorage",
        storeAuthStateInCookie: true
    }
};

```
> [!div renderon="portal"]
> > [!NOTE]
> > Bu hızlı başlangıç, Enter_the_Supported_Account_Info_Here destekler.


> [!div renderon="docs"]
>
> Konumlar:
> - Enter_the_Application_Id_here >, kaydettiğiniz uygulamanın **uygulama (istemci) kimliğidir** .  *\<*
> - Enter_the_Tenant_info_here > aşağıdaki seçeneklerden birine ayarlanır:  *\<*
>    - Uygulamanız *bu kuruluş dizinindeki hesapları*destekliyorsa, bu DEĞERI **Kiracı kimliği** veya **kiracı adı** (örneğin, *contoso.Microsoft.com*) ile değiştirin.
>    - Uygulamanız *herhangi bir kuruluş dizinindeki hesapları*destekliyorsa, bu değeri **kuruluşlar**ile değiştirin.
>    - Uygulamanız *herhangi bir kurumsal dizin ve kişisel Microsoft hesabında hesapları*destekliyorsa, bu değeri **ortak**ile değiştirin. *Yalnızca kişisel Microsoft hesaplarına*yönelik desteği kısıtlamak için bu değeri tüketicilerle değiştirin .
>
> > [!TIP]
> > **Uygulama (istemci) Kimliği**, **Dizin (kiracı) Kimliği** ve **Desteklenen hesap türleri** değerlerini bulmak için Azure portalında uygulamanın **Genel bakış** sayfasına gidin.
>

#### <a name="step-4-run-the-project"></a>4\. Adım: Projeyi çalıştırma

* [Node. js](https://nodejs.org/en/download/)kullanıyorsanız:

    1. Sunucuyu başlatmak için, proje dizininden aşağıdaki komutu çalıştırın:

        ```batch
        npm install
        node server.js
        ```

    1. Bir Web tarayıcısı açın ve adresine gidin `http://localhost:30662/`.
    1. Oturum açmak için **oturum aç** ' ı seçin ve ardından Microsoft Graph API 'yi çağırın.


* [Visual Studio](https://visualstudio.microsoft.com/downloads/)kullanıyorsanız, proje çözümünü seçin ve ardından projeyi çalıştırmak için F5 ' i seçin.

Tarayıcı uygulamayı yükledikten sonra **oturum aç**' ı seçin. İlk kez oturum açtığınızda, uygulamanın profilinize erişmesine ve oturumunuzu açmasını sağlamak için onayınızı vermeniz istenir. Başarıyla oturum açtıktan sonra, Kullanıcı profili bilgilerinizin sayfada görüntülenmesi gerekir.

## <a name="more-information"></a>Daha fazla bilgi

### <a name="msaljs"></a>msal. js

MSAL kitaplığı, kullanıcıları imzalar ve Microsoft Identity platform tarafından korunan bir API 'ye erişmek için kullanılan belirteçleri ister. Hızlı Başlangıç *dizini. html* dosyası kitaplığa bir başvuru içerir:

```html
<script src="https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0/js/msal.min.js"></script>
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
var msalConfig = {
    auth: {
        clientId: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.com/Enter_the_Tenant_Info_Here"
    },
    cache: {
        cacheLocation: "localStorage",
        storeAuthStateInCookie: true
    }
};

var myMSALObj = new Msal.UserAgentApplication(msalConfig);
```

> |Konum  |  |
> |---------|---------|
> |`ClientId`     | Azure portal kayıtlı uygulamanın uygulama KIMLIĞI.|
> |`authority`    | Seçim Daha önce yapılandırma bölümünde açıklandığı gibi, hesap türlerini destekleyen yetkili URL 'SI. Varsayılan yetkili `https://login.microsoftonline.com/common`. |
> |`cacheLocation`  | Seçim Kimlik doğrulama durumu için tarayıcı depolamayı ayarlar. Varsayılan değer sessionStorage ' dır.   |
> |`storeAuthStateInCookie`  | Seçim Tarayıcı tanımlama bilgilerinde kimlik doğrulama akışlarının doğrulanması için gerekli olan kimlik doğrulama isteği durumunu depolayan kitaplık. Bu tanımlama bilgisi, bazı [bilinen sorunları](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues)azaltmak üzere IE ve Edge tarayıcıları için ayarlanır. |

Kullanılabilir yapılandırılabilir seçenekler hakkında daha fazla bilgi için bkz. [istemci uygulamalarını başlatma](msal-js-initializing-client-applications.md).

### <a name="sign-in-users"></a>Oturum açma kullanıcıları

Aşağıdaki kod parçacığı, kullanıcıların oturum açma şeklini gösterir:

```javascript
var requestObj = {
    scopes: ["user.read"]
};

myMSALObj.loginPopup(requestObj).then(function (loginResponse) {
    //Login Success callback code here
}).catch(function (error) {
    console.log(error);
});
```

> |Konum  |  |
> |---------|---------|
> | `scopes`   | Seçim Oturum açma sırasında kullanıcı onayı için istenen kapsamları içerir. Örneğin, `[ "user.read" ]` Microsoft Graph veya `[ "<Application ID URL>/scope" ]` özel Web API 'leri için (yani, `api://<Application ID>/access_as_user`). |

> [!TIP]
> Alternatif olarak, geçerli sayfayı bir açılan pencere `loginRedirect` yerine oturum açma sayfasına yeniden yönlendirmek için yöntemini kullanmak isteyebilirsiniz.

### <a name="request-tokens"></a>İstek belirteçleri

MSAL belirteçleri elde etmek için üç yöntem kullanır `acquireTokenRedirect`: `acquireTokenPopup`, ve`acquireTokenSilent`

#### <a name="get-a-user-token-silently"></a>Kullanıcı belirtecini sessizce alma

Yöntemi `acquireTokenSilent` , Kullanıcı etkileşimi olmadan belirteç alma ve yenileme işlemleri gerçekleştirir. Ya `loginRedirect` `acquireTokenSilent` da yöntemi ilk kez yürütüldükten sonra, sonraki çağrılar için korunan kaynaklara erişmek üzere kullanılan belirteçleri elde etmek için yaygın olarak kullanılan yöntemdir. `loginPopup` Belirteçleri istek veya yenileme çağrıları sessizce yapılır.

```javascript
var requestObj = {
    scopes: ["user.read"]
};

myMSALObj.acquireTokenSilent(requestObj).then(function (tokenResponse) {
    // Callback code here
    console.log(tokenResponse.accessToken);
}).catch(function (error) {
    console.log(error);
});
```

> |Konum  |  |
> |---------|---------|
> | `scopes`   | API için erişim belirtecine döndürülmek istenen kapsamları içerir. Örneğin, `[ "user.read" ]` Microsoft Graph veya `[ "<Application ID URL>/scope" ]` özel Web API 'leri için (yani, `api://<Application ID>/access_as_user`).|

#### <a name="get-a-user-token-interactively"></a>Etkileşimli olarak kullanıcı belirteci alma

Kullanıcıları Microsoft Identity platform uç noktasıyla etkileşimde bulunmak için zorlamanız gereken durumlar vardır. Örneğin:
* Parolasının süresi sona erdiği için kullanıcıların kimlik bilgilerini yeniden girmesi gerekebilir.
* Uygulamanız, kullanıcının onaylaması gereken ek kaynak kapsamlarına erişim istiyor.
* İki öğeli kimlik doğrulaması gereklidir.

Çoğu uygulama için olağan olarak önerilen desenler ilk olarak çağrı `acquireTokenSilent` yapılır, sonra özel durumu yakalar ve sonra etkileşimli bir `acquireTokenPopup` istek başlatmak `acquireTokenRedirect`için (veya) çağırır.

Oturum açmak için sonuçları açılan pencerede çağırma. `acquireTokenPopup` (Veya `acquireTokenRedirect` kullanıcıları Microsoft Identity platform uç noktasına yönlendirmeye neden olur.) Bu pencerede, kullanıcıların kimlik bilgilerini onaylayarak, gerekli kaynağa onay vererek veya iki öğeli kimlik doğrulamasını tamamlayarak etkileşimde olmaları gerekir.

```javascript
var requestObj = {
    scopes: ["user.read"]
};

myMSALObj.acquireTokenPopup(requestObj).then(function (tokenResponse) {
    // Callback code here
    console.log(tokenResponse.accessToken);
}).catch(function (error) {
    console.log(error);
});
```

> [!NOTE]
> Bu hızlı başlangıç, `loginRedirect` Internet `acquireTokenRedirect` Explorer tarafından açılan pencerelerin işlenmesiyle ilgili bilinen bir [sorun](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) nedeniyle Microsoft Internet Explorer ile ve yöntemlerini kullanır.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç için uygulama oluşturmaya yönelik daha ayrıntılı bir adım adım kılavuz için bkz.:

> [!div class="nextstepaction"]
> [Oturum açma ve MS Graf çağırma öğreticisi](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-javascriptspa)

Belgeler, SSS, sorunlar ve daha fazlası için MSAL depoya göz atın, bkz:

> [!div class="nextstepaction"]
> [MSAL. js GitHub deposu](https://github.com/AzureAD/microsoft-authentication-library-for-js)
