---
title: Microsoft Identity platform tarafından korunan bir ASP.NET Web API 'SI çağırma
description: Bu hızlı başlangıçta, bir Windows Masaüstü (WPF) uygulamasından Microsoft Identity platform tarafından korunan bir ASP.NET Web API 'SI çağırma hakkında bilgi edinin. WPF istemcisi bir kullanıcının kimliğini doğrular, bir erişim belirteci ister ve Web API 'sini çağırır.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 12/12/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:ASP.NET
ms.openlocfilehash: 084083a704a007e6675234883c62350d1d9a0849
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81536157"
---
# <a name="quickstart-call-an-aspnet-web-api-protected-by-microsoft-identity-platform"></a>Hızlı başlangıç: Microsoft Identity platform tarafından korunan bir ASP.NET Web API 'SI çağırma

Bu hızlı başlangıçta, bir Web API 'sini kullanıma sunar ve bunu yalnızca kimliği doğrulanmış kullanıcının erişebileceği şekilde koruyabilirsiniz. Bu örnek, kişisel hesaplar tarafından verilen belirteçleri (outlook.com, live.com ve diğerleri dahil olmak üzere) ve Microsoft Identity platformu ile tümleştirilmiş herhangi bir şirketten veya kuruluştan iş ve okul hesaplarını kabul edecek şekilde bir ASP.NET Web API 'sinin nasıl kullanıma sunuleceği gösterilmektedir.

Örnek ayrıca, bir Web API 'sine erişmek için bir erişim belirteci isteme şeklini gösteren bir Windows masaüstü uygulaması (WPF) istemcisi içerir.

## <a name="prerequisites"></a>Ön koşullar

Bu örneği çalıştırmak için aşağıdakilere ihtiyacınız olacaktır:

* Visual Studio 2017 veya 2019.  [Visual Studio 'yu ücretsiz](https://www.visualstudio.com/downloads/)indirin.

* [Microsoft hesabı](https://www.outlook.com) veya [Office 365 Geliştirici Programı](/office/developer-program/office-365-developer-program)

## <a name="download-or-clone-this-sample"></a>Bu örneği indir veya Kopyala

Bu örneği kabuktan veya komut satırınızdan kopyalayabilirsiniz:

  ```console
  git clone https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git
  ```

Ya da [ÖRNEĞI ZIP dosyası olarak indirebilirsiniz](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip).

## <a name="register-your-web-api-in-the-application-registration-portal"></a>Web API 'nizi uygulama kayıt portalına kaydetme

### <a name="choose-the-azure-ad-tenant-where-you-want-to-create-your-applications"></a>Uygulamalarınızı oluşturmak istediğiniz Azure AD kiracısını seçin

Uygulamalarınızı el ile kaydetmek istiyorsanız, ilk adım olarak şunları yapmanız gerekir:

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Hesabınız birden fazla Azure AD kiracısında mevcutsa, sayfanın üstündeki menüdeki sağ üst köşedeki profilinizi seçin ve ardından dizin ' i **değiştirin**.
   Portal oturumunuzu istenen Azure AD kiracısına değiştirin.

### <a name="register-the-service-app-todolistservice"></a>Hizmet uygulamasını kaydetme (TodoListService)

1. Geliştiriciler için Microsoft Identity platformu [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasına gidin.
1. **Yeni kayıt**seçeneğini belirleyin.
1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
   - **Ad** bölümünde, örneğin `AppModelv2-NativeClient-DotNet-TodoListService`, uygulamanın kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin.
   - **Desteklenen hesap türlerini** **herhangi bir kuruluş dizinindeki hesaplara**değiştirin.
   - Uygulamayı kaydetmek için **Kaydet**'i seçin.

1. Uygulamaya **genel bakış** sayfasında, **uygulama (istemci) kimlik** değerini bulun ve daha sonra için kaydedin. Bu proje için Visual Studio yapılandırma dosyasını yapılandırmanız gerekir (`ClientId` içinde `TodoListService\Web.config`).
1. API 'yi **kullanıma** sunma bölümünü seçin ve:
   - **Kapsam Ekle** ' yi seçin
   - **Kaydet ve devam et ' i** seçerek ÖNERILEN uygulama kimliği URI 'sini (API://{ClientID}) kabul edin
   - Aşağıdaki parametreleri girin:
     - **kapsam adı** kullanımı için`access_as_user`
     - **Yöneticiler ve kullanıcılar** seçeneğinin **izin verebilir** olarak seçildiğinden emin olun
     - **yönetici izni görünen adı** türü`Access TodoListService as a user`
     - **yönetici onayı açıklama** türü`Accesses the TodoListService web API as a user`
     - **Kullanıcı izni görünen adı** türü`Access TodoListService as a user`
     - **Kullanıcı onayı açıklama** türü`Accesses the TodoListService web API as a user`
     - **Durumu** **etkin** olarak tut
     - **Kapsam Ekle** ' yi seçin

### <a name="configure-the-service-project-to-match-the-registered-web-api"></a>Hizmet projesini kayıtlı Web API 'siyle eşleşecek şekilde yapılandırma

1. Visual Studio 'da çözümü açın ve ardından **TodoListService** projesi kökünün altındaki **Web. config** dosyasını açın.
1. `ida:ClientId` Parametresinin değerini, **Istemci KIMLIĞI (uygulama kimliği)** Ile, uygulama kayıt portalı 'nda yeni kaydettiğiniz uygulamadan değiştirin.

### <a name="add-the-new-scope-to-the-todolistclients-appconfig"></a>Yeni kapsamı *TodoListClient*'ın App. config dosyasına ekleyin

1. **TodoListClient** projesinin kök klasöründe bulunan **app. config** dosyasını açın ve sonra, dizeyi `TodoListServiceScope` `{Enter the Application ID of your TodoListService from the app registration portal}`değiştirerek, parametre altındaki *TodoListService* için yeni kaydettiğiniz uygulamadan **uygulama kimliği** ' ni yapıştırın.

   > Unutmayın: aşağıdaki biçimi kullandığından emin olun:
   >
   > `api://{TodoListService-Application-ID}/access_as_user`
   >
   >(burada {TodoListService-Application-ID}, TodoListService için uygulama KIMLIĞINI temsil eden GUID 'dir).

## <a name="register-the-client-app-todolistclient"></a>İstemci uygulamasını kaydetme (TodoListClient)

Bu adımda, uygulama kayıt portalı 'nda yeni bir uygulama kaydederek *TodoListClient* projenizi yapılandırırsınız. İstemci ve sunucunun *aynı uygulamayı* kabul ettiği durumlarda, ' 2. adım ' içinde kayıtlı aynı uygulamayı da kullanabilirsiniz. Kullanıcıların Microsoft kişisel hesaplarıyla oturum açmasını isterseniz aynı uygulamanın kullanılması gerekir

### <a name="register-the-todolistclient-application-in-the-application-registration-portal"></a>*TodoListClient* uygulamasını *uygulama kayıt portalına* kaydetme

1. Geliştiriciler için Microsoft Identity platformu [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasına gidin.
1. **Yeni kayıt**seçeneğini belirleyin.
1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
   - **Ad** bölümünde, örneğin `NativeClient-DotNet-TodoListClient`, uygulamanın kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin.
   - **Desteklenen hesap türlerini** **herhangi bir kuruluş dizinindeki hesaplara**değiştirin.
   - Uygulamayı kaydetmek için **Kaydet**'i seçin.
1. Uygulamanın Genel Bakış sayfasında, **Kimlik doğrulaması** bölümünü seçin.
   - **Yeniden yönlendirme URI** | 'lerinde**ortak istemciler için önerilen yeniden yönlendirme URI 'leri (mobil, masaüstü)** bölümüne bakın**https://login.microsoftonline.com/common/oauth2/nativeclient**
   - **Kaydet**’i seçin.
1. **API izinleri** bölümünü seçin
   - **Izin Ekle** düğmesine tıklayın ve ardından
   - **API 'Lerim** sekmesini seçin.
   - API 'Ler listesinde, veya Web API 'SI `AppModelv2-NativeClient-DotNet-TodoListService API`için girdiğiniz adı seçin.
   - Zaten işaretli değilse **access_as_user** iznini denetleyin. Gerekirse arama kutusunu kullanın.
   - **Izin Ekle** düğmesini seçin

### <a name="configure-your-todolistclient-project"></a>*TodoListClient* projenizi yapılandırma

1. *Uygulama kayıt portalı*' nda, **genel bakış** sayfasında, **uygulama (istemci) kimliğinin** değerini kopyalayın
1. **TodoListClient** projesinin kök klasöründe bulunan `ida:ClientId` **app. config** dosyasını açın ve değeri parametre değerine yapıştırın

## <a name="run-your-project"></a>Projenizi çalıştırma

1. Projenizi `<F5>` çalıştırmak için tuşuna basın. *TodoListClient* ' i açmanız gerekir.
1. Sağ üst köşedeki **oturum aç** ' ı seçin ve uygulamanızı kaydetmek için kullandığınız kullanıcıyla veya aynı dizindeki bir kullanıcıyla oturum açın.
1. Bu noktada, ilk kez oturum açıyorsanız, *TodoListService* Web API 'sine onay vermeniz istenebilir.
1. Oturum açma Ayrıca, *TodoListService* Web API 'sine erişmek ve *Yapılacaklar* listesini değiştirmek için *access_as_user* kapsamına erişim belirteci ister.

## <a name="pre-authorize-your-client-application"></a>İstemci uygulamanızı önceden yetkilendirme

Diğer dizinlerden kullanıcıların Web API 'nize erişmesine izin vermenin bir yolu, Web API 'niz için *önceden yetkilendirilmiş* uygulamalar listesindeki uygulama kimliklerini istemci uygulamalarından ekleyerek Web API 'nize erişmek üzere istemci uygulamalarının *önceden yetkilendirilmesini* sağlar. Önceden yetkilendirilmiş bir istemci ekleyerek, kullanıcının Web API 'nizi kullanma onayını zorunlu kılmayacaktır. Web uygulamanızı önceden yetkilendirmek için aşağıdaki adımları izleyin::

1. *Uygulama kayıt portalına* geri dönün ve **TodoListService**özelliklerini açın.
1. **API 'Yi kullanıma** sunma bölümünde, *yetkili istemci uygulamaları* bölümünde **istemci uygulaması Ekle** ' ye tıklayın.
1. *ISTEMCI kimliği* alanında `TodoListClient` uygulamanın uygulama kimliğini yapıştırın.
1. *Yetkili kapsamlar* bölümünde, bu Web API 'sinin `api://<Application ID>/access_as_user`kapsamını seçin.
1. Sayfanın alt kısmındaki **Uygulama Ekle** düğmesine basın.

## <a name="run-your-project"></a>Projenizi çalıştırma

1. Projenizi `<F5>` çalıştırmak için tuşuna basın. *TodoListClient* ' i açmanız gerekir.
1. Sağ üst köşedeki **oturum aç** ' ı (veya temiz önbellek/oturum açma) seçin ve ardından kişisel Microsoft hesabı (live.com veya hotmail.com) veya iş ya da okul hesabı kullanarak oturum açın.

## <a name="optional-restrict-sign-in-access-to-your-application"></a>İsteğe bağlı: uygulamanıza yönelik oturum açma erişimini kısıtlayın

Varsayılan olarak, bu kod örneğini indirerek ve uygulamayı önceki adımlardan sonra Azure Active Directory v2 uç noktasını kullanacak şekilde yapılandırdığınızda, Azure AD ile tümleştirilen her türlü kuruluşun Iş veya okul hesapları, belirteçler isteyebilir ve Web API 'niz üzerinde erişim sağlayabilir.

Uygulamanızda kimlerin oturum açabileceğini kısıtlamak için seçeneklerden birini kullanın:

### <a name="option-1-restrict-access-to-a-single-organization-single-tenant"></a>Seçenek 1: tek bir kuruluşa erişimi kısıtlama (tek kiracılı)

Uygulamanız için oturum açma erişimini, yalnızca tek bir Azure AD kiracısında bulunan Kullanıcı hesaplarına (Bu kiracının *Konuk hesapları* dahil) kısıtlayabilirsiniz. Bu senaryo *iş kolu uygulamaları*için ortaktır:

1. **App_Start \Startup.exe** dosyasını açın ve ' a `OpenIdConnectSecurityTokenProvider` geçirilecek meta veri uç noktasının değerini değiştirin `"https://login.microsoftonline.com/{Tenant ID}/v2.0/.well-known/openid-configuration"` (örneğin, kiracı adını da kullanabilirsiniz `contoso.onmicrosoft.com`).
2. Aynı dosyada, `ValidIssuer` `TokenValidationParameters` `"https://sts.windows.net/{Tenant ID}/"` ve `ValidateIssuer` bağımsız değişkeninde özelliğini olarak `true`ayarlayın.

### <a name="option-2-use-a-custom-method-to-validate-issuers"></a>Seçenek 2: verenler doğrulamak için özel bir yöntem kullanma

**Issuervalidator** parametresini kullanarak verenler doğrulamak için özel bir yöntem uygulayabilirsiniz. Bu parametrenin nasıl kullanılacağı hakkında daha fazla bilgi için, [Tokenvalidationparameters sınıfı](/dotnet/api/microsoft.identitymodel.tokens.tokenvalidationparameters?view=azure-dotnet)hakkında makalesini okuyun.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Identity platformunun desteklediği korumalı Web API senaryosu hakkında daha fazla bilgi edinin:
> [!div class="nextstepaction"]
> [Korumalı Web API senaryosu](scenario-protected-web-api-overview.md)
