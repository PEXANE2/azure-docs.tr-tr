---
title: Açısal tek sayfalı uygulamalarda oturum açın - Azure
titleSuffix: Microsoft identity platform
description: Bir Açısal uygulamanın Microsoft kimlik platformlarını kullanarak erişim belirteçleri gerektiren bir API'yi nasıl çağırabileceğini öğrenin.
services: active-directory
author: jasonnutter
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:JavaScript
ms.topic: quickstart
ms.workload: identity
ms.date: 03/18/2020
ms.author: janutter
ms.openlocfilehash: 61a098b92db13b8422d9cfebb19610c5de7685cd
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631759"
---
# <a name="quickstart-sign-in-users-and-get-an-access-token-in-an-angular-spa"></a>Quickstart: Kullanıcıları oturum açın ve Açısal SPA'da bir erişim belirteci alın

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Önizlemeler, [ek kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

Bu hızlı başlangıçta, açısal tek sayfalı bir uygulamanın (SPA) kişisel Microsoft hesapları ve iş hesapları ve okul hesapları olan kullanıcılarda nasıl oturum açabileceğini öğrenmek için bir kod örneği kullanırsınız. Açısal BIR SPA, Microsoft Graph API'yi veya herhangi bir web API'sını aramak için bir erişim belirteci de alabilir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* [Node.js](https://nodejs.org/en/download/)
* [Proje](https://code.visualstudio.com/download) dosyalarını düzenlemesi için Visual Studio Kodu veya projeyi çalıştırmak için [Visual Studio 2019.](https://visualstudio.microsoft.com/downloads/)

> [!div renderon="docs"]
> ## <a name="register-and-download-the-quickstart-app"></a>Hızlı başlatma uygulamasını kaydedin ve indirin
> Hızlı başlangıç uygulamasını başlatmak için aşağıdaki seçeneklerden birini kullanın:
>
> ### <a name="option-1-express-register-and-auto-configure-the-app-then-download-the-code-sample"></a>Seçenek 1 (Express): Uygulamayı kaydedin ve otomatik olarak yapılandırın. Sonra kod örneğini indirin
>
> 1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
> 1. Hesabınızda birden fazla kiracıya erişim varsa, sağ üstteki hesabı seçin ve ardından portal oturumunuzu kullanmak istediğiniz Azure Etkin Dizin (Azure AD) kiracısına ayarlayın.
> 1. Yeni [Azure portalını](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs) açın - Uygulama kayıtları bölmesi.
> 1. Uygulamanız için bir ad girin ve ardından **Kaydol'u**seçin.
> 1. Hızlı başlat bölmesine gidin ve Açısal hızlı başlatış'ı görüntüleyin. Yeni uygulamanızı indirmek ve otomatik olarak yapılandırmak için yönergeleri izleyin.
>
> ### <a name="option-2-manual-register-and-manually-configure-the-application-and-code-sample"></a>Seçenek 2 (Manuel): Uygulama ve kod örneğini kaydedin ve el ile yapılandırın
>
> #### <a name="step-1-register-the-application"></a>Adım 1: Başvuruyu kaydedin
>
> 1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
>
> 1. Hesabınızda birden fazla kiracıya erişim varsa, sağ üstteki hesabınızı seçin ve portal oturumunuzu kullanmak istediğiniz Azure AD kiracısına ayarlayın.
> 1. Azure portalında [tek sayfalık bir uygulama kaydetmek](https://docs.microsoft.com/azure/active-directory/develop/scenario-spa-app-registration) için yönergeleri izleyin.
> 1. Uygulama kaydınızın **Kimlik Doğrulama bıçağına** yeni bir platform ekleyin ve yeniden yönlendirme URI'sini kaydedin:`http://localhost:4200/`
> 1. Bu quickstart [örtülü hibe akışını](v2-oauth2-implicit-grant-flow.md)kullanır. **Kimlik belirteçleri** ve Erişim **Belirteçleri**için **Örtülü Hibe** ayarlarını seçin. Bu uygulama kullanıcıları işaretlediği ve api aradığı için kimlik belirteçleri ve erişim belirteçleri gereklidir.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-the-application-in-the-azure-portal"></a>Adım 1: Azure portalında uygulamayı yapılandırma
> Bu hızlı başlangıç için kod örneği için, bir `redirectUri` `http://localhost:4200/` as eklemeniz ve **Örtülü hibeyi**etkinleştirmeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişiklikleri benim için yapın]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-javascript/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-the-code-sample"></a>Adım 2: Kod örneğini indirin
>[!div renderon="docs"]
>Node.js kullanarak projeyi bir web sunucusuyla https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular çalıştırmak, çekirdek proje dosyalarını klonlamak veya [indirmek.](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular/archive/master.zip) Visual Studio Code gibi bir düzenleyici kullanarak dosyaları açın.

> [!div renderon="portal" id="autoupdate" class="sxs-lookup nextstepaction"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular/archive/master.zip)

> [!div renderon="docs"]
>#### <a name="step-3-configure-the-javascript-app"></a>Adım 3: JavaScript uygulamasını yapılandırın
>
> *src/app* klasöründe *app.module.ts'yi* edin `clientId` `authority` ve `MsalModule.forRoot`değerleri .
>
>```javascript
>MsalModule.forRoot({
>    auth: {
>        clientId: 'Enter_the_Application_Id_here', // This is your client ID
>        authority: 'https://login.microsoftonline.com/Enter_the_Tenant_Info_Here', // This is your tenant info
>        redirectUri: 'Enter_the_Redirect_Uri_Here' // This is your redirect URI
>    },
>    cache: {
>        cacheLocation: 'localStorage',
>        storeAuthStateInCookie: isIE, // set to true for IE 11
>    },
>},
> //... )
>```

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > Enter_the_Supported_Account_Info_Here


> [!div renderon="docs"]
>
> Bu değerleri şu şekilde değiştirin:
>
>|Değer adı|Açıklama|
>|---------|---------|
>|Enter_the_Application_Id_Here|Başvuru kaydınızın **genel bakış** sayfasında, bu sizin **başvuru (istemci) kimliği** |
>|Enter_the_Cloud_Instance_Id_Here|Bu, Azure bulutunun örneğidir. Ana veya küresel Azure bulutu için ' yi girin. https://login.microsoftonline.com Ulusal bulutlar (örneğin, Çin) için [Ulusal bulutlara](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud)bakın.|
>|Enter_the_Tenant_Info_Here| Aşağıdaki seçeneklerden birine ayarlayın: 1) Uygulamanız *bu kuruluş dizinindeki hesapları*destekliyorsa, bu değeri **Dizin (Kiracı) Kimliği** veya Kiracı **adı** (örneğin, *contoso.microsoft.com)* ile değiştirin. 2) Uygulamanız *herhangi bir kuruluş dizinindeki hesapları destekliyorsa,* bu değeri **kuruluşlarla**değiştirin. 3) Uygulamanız *herhangi bir kuruluş dizinindeki ve kişisel Microsoft hesaplarındaki hesapları*destekliyorsa, bu değeri **ortak**değerlerle değiştirin. 4) Desteği *yalnızca kişisel Microsoft hesaplarına*kısıtlamak için, bu değeri **tüketicilerle**değiştirin. |
>|Enter_the_Redirect_Uri_Here|Değiştir`http://localhost:4200`|
>|önbellekKonum  | (İsteğe bağlı) Auth durumu için tarayıcı depolamasını ayarlar. Varsayılan olan sessionStorage'dır.   |
>|storeAuthStateInCookie  | (İsteğe bağlı) Kimlik doğrulama isteği durumunu depolayan kitaplık. Bu durum, tarayıcı tanımlama bilgilerindeki kimlik doğrulama akışlarını doğrulamak için gereklidir. Bu çerez, Internet Explorer ve Edge tarayıcılarının bu iki tarayıcıyı barındıracak şekilde ayarlanmıştır. Daha fazla ayrıntı için [bilinen sorunlara](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues->on-IE-and-Edge-Browser#issues)bakın. |
> > [!TIP]
> > **Uygulama (istemci) Kimliği**, **Dizin (kiracı) Kimliği** ve **Desteklenen hesap türleri** değerlerini bulmak için Azure portalında uygulamanın **Genel bakış** sayfasına gidin.

Kullanılabilir yapılandırılabilir seçenekler hakkında daha fazla bilgi için [bkz.](msal-js-initializing-client-applications.md)

>[!div class="sxs-lookup" renderon="portal"]
>#### <a name="step-3-run-the-project"></a>Adım 3: Projeyi çalıştırın

>[!div renderon="docs"]
>#### <a name="step-4-run-the-project"></a>Adım 4: Projeyi çalıştırın

* Node.js kullanıyorsanız:

    1. Proje dizininden aşağıdaki komutları çalıştırarak sunucuyu başlatın:

        ```console
        npm install
        npm start
        ```

    1. Göz `http://localhost:4200/`atın.
    1. **Giriş'i**seçin.
    1. Microsoft Graph'ı aramak için **Profil'i** seçin.

Tarayıcı uygulamayı yüklendikten sonra **Giriş'i**seçin. İlk oturum açtığınızda, uygulamanın profilinize erişmesine ve oturum açmasına izin vermek için izninizi sağlamanız istenir. Başarılı bir şekilde oturum açınca **Profil'i**seçin ve kullanıcı profili bilgileriniz sayfada görüntülenir.

## <a name="how-the-sample-works"></a>Örnek nasıl çalışır?

![Bu hızlı başlatmadaki örnek uygulama nasıl çalışır?](media/quickstart-v2-javascript/javascriptspa-intro.svg)


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> Bir kullanıcıda nasıl oturum açılabildiğini ve jeton elde etmeyi öğrenmek için [Açısal Öğretici'ye](https://docs.microsoft.com/azure/active-directory/develop/tutorial-v2-angular) gidin.

Belgeler, SSS, sorunlar ve daha fazlası için [MSAL repo'ya](https://github.com/AzureAD/microsoft-authentication-library-for-js) göz atın.
