---
title: Angular tek sayfalı uygulamalardaki kullanıcı oturum açma-Azure
titleSuffix: Microsoft identity platform
description: Angular uygulamasının Microsoft Identity platformunu kullanarak erişim belirteçleri gerektiren bir API 'YI nasıl çağırabileceğinizi öğrenin.
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
ms.openlocfilehash: 4b6a2481c18314a44470a020033ffdc4ba1d7259
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81380025"
---
# <a name="quickstart-sign-in-users-and-get-an-access-token-in-an-angular-single-page-application"></a>Hızlı başlangıç: angular tek sayfalı uygulamada Kullanıcı oturumu açma ve erişim belirteci edinme

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Önizlemeler, [ek kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma (GA) önce değişebilir.

Bu hızlı başlangıçta, angular bir tek sayfalı uygulamanın (SPA) kişisel Microsoft hesapları, iş hesapları veya okul hesapları olan kullanıcıların oturumunu nasıl imzalayabileceğinizi öğrenmek için bir kod örneği kullanırsınız. Angular SPA, Microsoft Graph API 'sini veya herhangi bir Web API 'sini çağırmak için bir erişim belirteci de alabilir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. [Ücretsiz bir tane oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Node. js](https://nodejs.org/en/download/).
* Projeyi çalıştırmak için proje dosyalarını veya [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) 'yi düzenlemek [Visual Studio Code](https://code.visualstudio.com/download) .

> [!div renderon="docs"]
> ## <a name="register-and-download-the-quickstart-app"></a>Hızlı başlangıç uygulamasını kaydedin ve indirin
> Hızlı başlangıç uygulamasını başlatmak için aşağıdaki seçeneklerden birini kullanın.
>
> ### <a name="option-1-express-register-and-automatically-configure-the-app-and-then-download-the-code-sample"></a>Seçenek 1 (Express): uygulamayı kaydedin ve otomatik olarak yapılandırın ve ardından kod örneğini indirin
>
> 1. [Azure Portal](https://portal.azure.com) oturum açın.
> 1. Hesabınızın birden fazla kiracıya erişimi varsa, sağ üst köşedeki hesabı seçin ve ardından Portal oturumunuzu kullanmak istediğiniz Azure Active Directory (Azure AD) kiracısına ayarlayın.
> 1. Yeni [uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs) bölmesini Azure Portal açın.
> 1. Uygulamanız için bir ad girin ve ardından **Kaydet**' i seçin.
> 1. Hızlı başlangıç bölmesine gidin ve angular hızlı başlangıcını görüntüleyin. Yeni uygulamanızı indirip otomatik olarak yapılandırmak için yönergeleri izleyin.
>
> ### <a name="option-2-manual-register-and-manually-configure-the-application-and-code-sample"></a>Seçenek 2 (el ile): uygulamayı ve kod örneğini kaydetme ve el ile yapılandırma
>
> #### <a name="step-1-register-the-application"></a>1. Adım: uygulamayı kaydetme
>
> 1. [Azure Portal](https://portal.azure.com) oturum açın.
> 1. Hesabınızın birden fazla kiracıya erişimi varsa, sağ üst köşedeki hesabınızı seçin ve Portal oturumunuzu kullanmak istediğiniz Azure AD kiracısına ayarlayın.
> 1. Azure portal [tek sayfalı bir uygulamayı kaydetmek](https://docs.microsoft.com/azure/active-directory/develop/scenario-spa-app-registration) için yönergeleri izleyin.
> 1. Uygulama kaydlarınızın **kimlik doğrulama** bölmesine yeni bir platform ekleyin ve yenıden yönlendirme URI 'sini kaydedin: `http://localhost:4200/`.
> 1. Bu hızlı başlangıç, [örtük verme akışını](v2-oauth2-implicit-grant-flow.md)kullanır. **Kimlik belirteçleri** ve **erişim belirteçleri**için **örtülü izin** ayarlarını seçin. KIMLIK belirteçleri ve erişim belirteçleri gereklidir çünkü bu uygulama kullanıcılar üzerinde oturum açar ve bir API çağırır.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-the-application-in-the-azure-portal"></a>1. Adım: uygulamayı Azure portal yapılandırma
> Bu hızlı başlangıçta çalışması için kod örneği için, bir yeniden yönlendirme URI 'SI eklemeniz **http://localhost:4200/** ve **örtülü izni**etkinleştirmeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişiklikleri benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-javascript/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-the-code-sample"></a>2. Adım: kod örneğini Indirme
>[!div renderon="docs"]
>Projeyi Node. js kullanarak bir Web sunucusuyla çalıştırmak için, [örnek depoyu kopyalayın](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular) veya [temel proje dosyalarını indirin](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular/archive/master.zip). Visual Studio Code gibi bir düzenleyiciyi kullanarak dosyaları açın.

> [!div renderon="portal" id="autoupdate" class="sxs-lookup nextstepaction"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular/archive/master.zip)

> [!div renderon="docs"]
>#### <a name="step-3-configure-the-javascript-app"></a>3. Adım: JavaScript uygulamasını yapılandırma
>
> *Src/App* klasöründe *app. Module. TS* öğesini düzenleyin ve altındaki `clientId` `authority` `MsalModule.forRoot`ve değerlerini ayarlayın.
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
> Şu değerleri değiştirin:
>
>|Değer adı|Açıklama|
>|---------|---------|
>|Enter_the_Application_Id_Here|Uygulama kaydlarınızın **genel bakış** sayfasında bu, **uygulamanızın (istemci) kimlik** değeridir. |
>|Enter_the_Cloud_Instance_Id_Here|Bu, Azure bulutunun örneğidir. Ana veya küresel Azure bulutu için girin **https://login.microsoftonline.com**. Ulusal bulutlar (örneğin, Çin) için bkz. [Ulusal bulutlar](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud).|
>|Enter_the_Tenant_Info_Here| Aşağıdaki seçeneklerden birine ayarlayın: uygulamanız *bu kuruluş dizinindeki hesapları*destekliyorsa, bu değeri dizin (KIRACı) kimliği veya kiracı adı (örneğin, **contoso.Microsoft.com**) ile değiştirin. Uygulamanız *herhangi bir kuruluş dizinindeki hesapları*destekliyorsa, bu değeri **kuruluşlar**ile değiştirin. Uygulamanız *herhangi bir kurumsal dizin ve kişisel Microsoft hesabında hesapları*destekliyorsa, bu değeri **ortak**ile değiştirin. *Yalnızca kişisel Microsoft hesaplarına*yönelik desteği kısıtlamak için bu değeri **tüketicilerle**değiştirin. |
>|Enter_the_Redirect_Uri_Here|İle **http://localhost:4200**değiştirin.|
>|Önbellekelocation  | Seçim Kimlik doğrulama durumu için tarayıcı depolamayı ayarlayın. Varsayılan değer **sessionStorage**' dır.   |
>|Storeauthstateıncookie  | Seçim Kimlik doğrulama isteği durumunu depolayan kitaplığı belirler. Tarayıcı tanımlama bilgilerinde kimlik doğrulama akışlarını doğrulamak için bu durum gereklidir. Bu tanımlama bilgisi, Internet Explorer ve Edge 'in bu iki tarayıcıyı kapsayacak şekilde ayarlanır. Daha fazla ayrıntı için bkz. [bilinen sorunlar](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues->on-IE-and-Edge-Browser#issues). |
> > [!TIP]
> > **Uygulama (istemci) Kimliği**, **Dizin (kiracı) Kimliği** ve **Desteklenen hesap türleri** değerlerini bulmak için Azure portalında uygulamanın **Genel bakış** sayfasına gidin.

Kullanılabilir yapılandırılabilir seçenekler hakkında daha fazla bilgi için bkz. [istemci uygulamalarını başlatma](msal-js-initializing-client-applications.md). 

MSAL. js kitaplığı için kaynak kodunu GitHub ' da [Azuread/Microsoft-Authentication-Library-for-js](https://github.com/AzureAD/microsoft-authentication-library-for-js) deposunda bulabilirsiniz.

>[!div class="sxs-lookup" renderon="portal"]
>#### <a name="step-3-run-the-project"></a>3. Adım: projeyi çalıştırma

>[!div renderon="docs"]
>#### <a name="step-4-run-the-project"></a>4. Adım: projeyi çalıştırma

Node. js kullanıyorsanız:

1. Proje dizininden aşağıdaki komutları çalıştırarak sunucuyu başlatın:

   ```console
   npm install
   npm start
   ```

1. Öğesine **http://localhost:4200/** gidin.
1. **Oturum aç**' ı seçin.
1. Microsoft Graph çağırmak için **profil** ' i seçin.

Tarayıcı uygulamayı yükledikten sonra **oturum aç**' ı seçin. İlk kez oturum açmaya başladığınızda, uygulamanın profilinize erişmesine izin vermek için onayınızı sağlamanız ve oturumunuzu açmanız istenir. Başarıyla oturum açtıktan sonra **profil**' i seçin ve Kullanıcı profili bilgileriniz sayfada görüntülenir.

## <a name="how-the-sample-works"></a>Örneğin nasıl çalıştığı

![Bu hızlı başlangıçta örnek uygulamanın nasıl çalıştığını gösteren diyagram](media/quickstart-v2-javascript/javascriptspa-intro.svg)


## <a name="next-steps"></a>Sonraki adımlar

Ardından, bir kullanıcı oturumunu nasıl kullanabileceğinizi ve angular öğreticisinde belirteç edinmeyi öğrenin:

> [!div class="nextstepaction"]
> [Angular öğreticisi](https://docs.microsoft.com/azure/active-directory/develop/tutorial-v2-angular)

