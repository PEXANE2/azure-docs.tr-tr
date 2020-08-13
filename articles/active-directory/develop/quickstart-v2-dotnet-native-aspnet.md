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
ms.openlocfilehash: 62cebb4e774e2f86ed6a4a17edd6da71f7c7cd9f
ms.sourcegitcommit: a2a7746c858eec0f7e93b50a1758a6278504977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2020
ms.locfileid: "88141338"
---
# <a name="quickstart-call-an-aspnet-web-api-protected-by-microsoft-identity-platform"></a>Hızlı başlangıç: Microsoft Identity platform tarafından korunan bir ASP.NET Web API 'SI çağırma

Bu hızlı başlangıçta, bir Web API 'sini kullanıma sunar ve bunu yalnızca kimliği doğrulanmış kullanıcının erişebileceği şekilde koruyabilirsiniz. Bu örnek, kişisel hesaplar tarafından verilen belirteçleri (outlook.com, live.com ve diğerleri dahil olmak üzere) ve Microsoft Identity platformu ile tümleştirilmiş herhangi bir şirketten veya kuruluştan iş ve okul hesaplarını kabul edecek şekilde bir ASP.NET Web API 'sinin nasıl kullanıma sunuleceği gösterilmektedir.

Örnek ayrıca, bir Web API 'sine erişmek için bir erişim belirteci isteme şeklini gösteren bir Windows masaüstü uygulaması (WPF) istemcisi içerir.

## <a name="prerequisites"></a>Ön koşullar

Bu örneği çalıştırmak için aşağıdakilere ihtiyacınız olacaktır:

* Visual Studio 2017 veya 2019.  [Visual Studio 'yu ücretsiz](https://www.visualstudio.com/downloads/)indirin.

* Bir [Microsoft hesabı](https://www.outlook.com) ya da [Microsoft 365 Geliştirici Programı](/office/developer-program/office-365-developer-program)

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
   - **Ad** bölümünde, örneğin, uygulamanın kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin `AppModelv2-NativeClient-DotNet-TodoListService` .
   - **Desteklenen hesap türlerini** **herhangi bir kuruluş dizinindeki hesaplara**değiştirin.
   - Uygulamayı kaydetmek için **Kaydet**'i seçin.

1. Uygulamaya **genel bakış** sayfasında, **uygulama (istemci) kimlik** değerini bulun ve daha sonra için kaydedin. Bu proje için Visual Studio yapılandırma dosyasını yapılandırmanız gerekir ( `ClientId` içinde `TodoListService\Web.config` ).
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

1. Visual Studio 'da çözümü açın ve ardından **TodoListService** projesi kökünün altındaki **Web.config** dosyasını açın.
1. Parametresinin değerini, `ida:ClientId` **istemci kimliği (uygulama kimliği)** Ile, uygulama kayıt portalı 'nda yeni kaydettiğiniz uygulamadan değiştirin.

### <a name="add-the-new-scope-to-the-todolistclients-appconfig"></a>Yeni kapsamı *TodoListClient*'in app.config ekleyin

* **TodoListClient** projesinin kök klasöründe yer alan **app.config** dosyasını açın ve sonra, dizeyi değiştirerek, parametre *ALTıNA yeni* kaydettiğiniz uygulamadan **uygulama kimliği** ' ni yapıştırın `TodoListServiceScope` `{Enter the Application ID of your TodoListService from the app registration portal}` .

  > [!NOTE]
  > Aşağıdaki biçimi kullandığından emin olun:
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
   - **Ad** bölümünde, örneğin, uygulamanın kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin `NativeClient-DotNet-TodoListClient` .
   - **Desteklenen hesap türlerini** **herhangi bir kuruluş dizinindeki hesaplara**değiştirin.
   - Uygulamayı kaydetmek için **Kaydet**'i seçin.
   
   > [!NOTE]
   > *TodoListClient* projesinin **app.config**, varsayılan değeri `ida:Tenant` olarak ayarlanır `common` .
   >
   > `common`bir Iş veya okul hesabı ya da Microsoft Kişisel hesabı kullanarak oturum açabilmeniz ( **herhangi bir kuruluş dizininde hesapları**seçtiğiniz için) anlamına gelir.
   >
   > `organizations`bir Iş veya okul hesabı kullanarak oturum açabilmeniz anlamına gelir.
   >
   > `consumers`yalnızca bir Microsoft Kişisel hesabı kullanarak oturum açabilmeniz anlamına gelir.
   >
   
1. Uygulamanın genel bakış sayfasında **kimlik doğrulama** bölümünü seçin.
   1. **Platform yapılandırması**altında **Platform Ekle** düğmesini seçin.
   1. **Mobil ve Masaüstü uygulamaları**için **mobil ve Masaüstü uygulamaları**' nı seçin.
   1. **Yeniden yönlendirme URI 'leri**için **https://login.microsoftonline.com/common/oauth2/nativeclient** onay kutusunu işaretleyin.
   1. **Yapılandır**'ı seçin.   
1. **API izinleri** bölümünü seçin
   1. **İzin ekleyin** düğmesini seçin.
   1. **API 'Lerim** sekmesini seçin.
   1. API 'Ler listesinde, `AppModelv2-NativeClient-DotNet-TodoListService API` veya Web API 'si için girdiğiniz adı seçin.
   1. Zaten işaretli değilse **access_as_user** iznini denetleyin. Gerekirse arama kutusunu kullanın.
   1. **Izin Ekle** düğmesini seçin.

### <a name="configure-your-todolistclient-project"></a>*TodoListClient* projenizi yapılandırma

1. *Uygulama kayıt portalı*' nda, **genel bakış** sayfasında, **uygulama (istemci) kimliğinin** değerini kopyalayın
1. **TodoListClient** projesinin kök klasöründe bulunan **app.config** dosyasını açın ve sonra değeri `ida:ClientId` parametre değerine yapıştırın

## <a name="run-your-project"></a>Projenizi çalıştırma

1. `<F5>`Projenizi çalıştırmak için tuşuna basın. *TodoListClient* ' i açmanız gerekir.
1. Sağ üst köşedeki **oturum aç** ' ı seçin ve uygulamanızı kaydetmek için kullandığınız kullanıcıyla veya aynı dizindeki bir kullanıcıyla oturum açın.
1. Bu noktada, ilk kez oturum açıyorsanız, *TodoListService* Web API 'sine onay vermeniz istenebilir.
1. Oturum açma Ayrıca, *TodoListService* Web API 'sine erişmek ve *Yapılacaklar* listesini değiştirmek için *access_as_user* kapsamına erişim belirteci ister.

## <a name="pre-authorize-your-client-application"></a>İstemci uygulamanızı önceden yetkilendirme

Diğer dizinlerden kullanıcıların Web API 'nize erişmesine izin vermenin bir yolu, Web API 'niz için *önceden yetkilendirilmiş* uygulamalar listesindeki uygulama kimliklerini istemci uygulamalarından ekleyerek Web API 'nize erişmek üzere istemci uygulamalarının *önceden yetkilendirilmesini* sağlar. Önceden yetkilendirilmiş bir istemci ekleyerek, kullanıcının Web API 'nizi kullanma onayını zorunlu kılmayacaktır. Web uygulamanızı önceden yetkilendirmek için aşağıdaki adımları izleyin::

1. *Uygulama kayıt portalına* geri dönün ve **TodoListService**özelliklerini açın.
1. **API 'Yi kullanıma** sunma bölümünde, *yetkili istemci uygulamaları* bölümünde **istemci uygulaması Ekle** ' ye tıklayın.
1. *ISTEMCI kimliği* alanında UYGULAMANıN uygulama kimliğini yapıştırın `TodoListClient` .
1. *Yetkili kapsamlar* bölümünde, bu Web API 'sinin kapsamını seçin `api://<Application ID>/access_as_user` .
1. Sayfanın alt kısmındaki **Uygulama Ekle** düğmesine basın.

## <a name="run-your-project"></a>Projenizi çalıştırma

1. `<F5>`Projenizi çalıştırmak için tuşuna basın. *TodoListClient* ' i açmanız gerekir.
1. Sağ üst köşedeki **oturum aç** ' ı (veya temiz önbellek/oturum açma) seçin ve ardından kişisel Microsoft hesabı (live.com veya hotmail.com) veya iş ya da okul hesabı kullanarak oturum açın.

## <a name="optional-restrict-sign-in-access-to-your-application"></a>İsteğe bağlı: uygulamanıza yönelik oturum açma erişimini kısıtlayın

Varsayılan olarak, bu kod örneğini indirerek ve uygulamayı önceki adımlardan sonra Azure Active Directory v2 uç noktasını kullanacak şekilde yapılandırdığınızda, Azure AD ile tümleştirilen her türlü kuruluşun Iş veya okul hesapları, belirteçler isteyebilir ve Web API 'niz üzerinde erişim sağlayabilir.

Uygulamanızda kimlerin oturum açabileceğini kısıtlamak için seçeneklerden birini kullanın:

### <a name="option-1-restrict-access-to-a-single-organization-single-tenant"></a>Seçenek 1: tek bir kuruluşa erişimi kısıtlama (tek kiracılı)

Uygulamanız için oturum açma erişimini, yalnızca tek bir Azure AD kiracısında bulunan Kullanıcı hesaplarına (Bu kiracının *Konuk hesapları* dahil) kısıtlayabilirsiniz. Bu senaryo *iş kolu uygulamaları*için ortaktır:

1. **App_Start \Startup.exe** dosyasını açın ve ' a geçirilecek meta veri uç noktasının değerini değiştirin `OpenIdConnectSecurityTokenProvider` `"https://login.microsoftonline.com/{Tenant ID}/v2.0/.well-known/openid-configuration"` (örneğin, kiracı adını da kullanabilirsiniz `contoso.onmicrosoft.com` ).
2. Aynı dosyada, `ValidIssuer` `TokenValidationParameters` `"https://sts.windows.net/{Tenant ID}/"` ve `ValidateIssuer` bağımsız değişkeninde özelliğini olarak ayarlayın `true` .

### <a name="option-2-use-a-custom-method-to-validate-issuers"></a>Seçenek 2: verenler doğrulamak için özel bir yöntem kullanma

**Issuervalidator** parametresini kullanarak verenler doğrulamak için özel bir yöntem uygulayabilirsiniz. Bu parametrenin nasıl kullanılacağı hakkında daha fazla bilgi için, [Tokenvalidationparameters sınıfı](/dotnet/api/microsoft.identitymodel.tokens.tokenvalidationparameters?view=azure-dotnet)hakkında makalesini okuyun.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Identity platformunun desteklediği korumalı Web API senaryosu hakkında daha fazla bilgi edinin:
> [!div class="nextstepaction"]
> [Korumalı Web API senaryosu](scenario-protected-web-api-overview.md)
