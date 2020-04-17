---
title: Microsoft kimlik platformu tarafından korunan ASP.NET web API'sını arayın
description: Bu hızlı başlangıçta, Microsoft kimlik platformu tarafından korunan ASP.NET web API'sini windows masaüstü (WPF) uygulamasından nasıl çağıracaksınız öğrenin. WPF istemcisi bir kullanıcının kimliğini doğrular, erişim jetonunu ister ve web API'sını çağırır.
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
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81536157"
---
# <a name="quickstart-call-an-aspnet-web-api-protected-by-microsoft-identity-platform"></a>Quickstart: Microsoft kimlik platformu tarafından korunan ASP.NET web API'sını arayın

Bu hızlı başlangıçta, bir web API'si ortaya çıkarır ve yalnızca kimlik doğrulaması yapılan kullanıcının erişebileceği şekilde onu korursunuz. Bu örnek, kişisel hesaplar (outlook.com, live.com ve diğerleri dahil) tarafından verilen belirteçlerin yanı sıra Microsoft kimlik platformuyla bütünleşmiş herhangi bir şirket veya kuruluşun iş ve okul hesaplarını kabul edebilmek için ASP.NET bir web API'sini nasıl ifşa edebileceğimizi gösterir.

Örnekte ayrıca, bir web API'sine erişmek için bir erişim jetonunu nasıl isteyebileceğinizi gösteren bir Windows Desktop uygulaması (WPF) istemcisi de bulunmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Bu örneği çalıştırmak için aşağıdakilere ihtiyacınız olacaktır:

* Visual Studio 2017 veya 2019.  [Visual Studio'u ücretsiz olarak](https://www.visualstudio.com/downloads/)indirin.

* Bir [Microsoft hesabı](https://www.outlook.com) veya [Office 365 Geliştirici Programı](/office/developer-program/office-365-developer-program)

## <a name="download-or-clone-this-sample"></a>Bu örneği indirin veya klonla

Bu örneği kabuğunuzdan veya komut satırınızdan klonlayabilirsiniz:

  ```console
  git clone https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git
  ```

Veya, [bir ZIP dosyası olarak örnek indirebilirsiniz.](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip)

## <a name="register-your-web-api-in-the-application-registration-portal"></a>Web API'nizi başvuru kayıt portalına kaydedin

### <a name="choose-the-azure-ad-tenant-where-you-want-to-create-your-applications"></a>Uygulamalarınızı oluşturmak istediğiniz Azure REKLAM kiracısını seçin

Uygulamalarınızı el ile kaydetmek istiyorsanız, ilk adım olarak şunları yapmanız gerekir:

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Hesabınız birden fazla Azure AD kiracısında mevcutsa, sayfanın üst kısmındaki menüdeki sağ üst köşedeki profilinizi seçin ve **ardından dizini değiştirin.**
   Portal oturumunuzu istediğiniz Azure AD kiracısına değiştirin.

### <a name="register-the-service-app-todolistservice"></a>Hizmet uygulamasını kaydedin (TodoListService)

1. Geliştiriciler için Microsoft kimlik platformuna gidin [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfası.
1. **Yeni kayıt**seçin.
1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
   - **Ad** bölümüne, örneğin `AppModelv2-NativeClient-DotNet-TodoListService`uygulama kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin.
   - **Desteklenen hesap türlerini** **herhangi bir kuruluş dizininde Hesaplarolarak değiştirin.**
   - Uygulamayı kaydetmek için **Kaydet**'i seçin.

1. Uygulamaya **Genel Bakış** sayfasında, **Uygulama (istemci) kimlik** değerini bulun ve daha sonraya kaydedin. Bu proje için Visual Studio yapılandırma dosyasını yapılandırmak`ClientId` `TodoListService\Web.config`için (içinde) gerekir.
1. **Api Açığa Çıkarma** bölümünü seçin ve:
   - **Kapsam Ekle'yi** seçin
   - **Kaydet ve Devam** et'i seçerek önerilen Uygulama Kimliği URI'yi (api://{clientId}) kabul edin
   - Aşağıdaki parametreleri girin:
     - **Kapsam adı** kullanımı için`access_as_user`
     - **Kimler izin verebilir** **için Yöneticiler ve kullanıcılar** seçeneğinin seçildiğinden emin olun
     - **admin onay görüntüleme adı** türünde`Access TodoListService as a user`
     - **Admin rıza açıklama** türünde`Accesses the TodoListService web API as a user`
     - **kullanıcı onayı görüntüleme adı** türünde`Access TodoListService as a user`
     - **Kullanıcı onayı açıklama** türünde`Accesses the TodoListService web API as a user`
     - **Durumu** **Etkin** Olarak Tutun
     - **Kapsam Ekle'yi** seçin

### <a name="configure-the-service-project-to-match-the-registered-web-api"></a>Hizmet projesini kayıtlı web API'sine uyacak şekilde yapılandırın

1. Çözümü Visual Studio'da açın ve **ardından TodoListService** projesinin kökü altında **Web.config** dosyasını açın.
1. Parametre değerini, `ida:ClientId` Başvuru Kayıt Portalı'na yeni kaydettiğiniz uygulamadan **müşteri kimliği (Başvuru Kimliği)** ile değiştirin.

### <a name="add-the-new-scope-to-the-todolistclients-appconfig"></a>*TodoListClient*'s app.config yeni kapsam ekleyin

1. **TodoListClient** projesinin kök klasöründe bulunan **app.config** dosyasını açın ve ardından *TodoListService* için yeni `TodoListServiceScope` kaydolduğunuz uygulamadan Uygulama `{Enter the Application ID of your TodoListService from the app registration portal}` **Kimliği'ni** parametre altında yapıştırın, dizeyi değiştirin.

   > Not: Aşağıdaki biçimi kullandığından emin olun:
   >
   > `api://{TodoListService-Application-ID}/access_as_user`
   >
   >({TodoListService-Application-ID} TodoListService için Uygulama Kimliği temsil eden GUID olduğu yer.

## <a name="register-the-client-app-todolistclient"></a>İstemci uygulamasını kaydedin (TodoListClient)

Bu adımda, Uygulama kayıt portalına yeni bir uygulama kaydederek *TodoListClient* projenizi yapılandırabilirsiniz. İstemci ve *sunucunun aynı uygulama* olarak kabul edildiği durumlarda, 'Adım 2'de kayıtlı olan aynı uygulamayı yeniden kullanabilirsiniz. Kullanıcıların Microsoft kişisel hesaplarında oturum açmalarını istiyorsanız, aynı uygulamayı kullanmak gereklidir

### <a name="register-the-todolistclient-application-in-the-application-registration-portal"></a>*TodoListClient* uygulamasını Başvuru *kayıt portalına* kaydedin

1. Geliştiriciler için Microsoft kimlik platformuna gidin [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfası.
1. **Yeni kayıt**seçin.
1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
   - **Ad** bölümüne, örneğin `NativeClient-DotNet-TodoListClient`uygulama kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin.
   - **Desteklenen hesap türlerini** **herhangi bir kuruluş dizininde Hesaplarolarak değiştirin.**
   - Uygulamayı kaydetmek için **Kaydet**'i seçin.
1. Uygulamanın Genel Bakış sayfasında, **Kimlik doğrulaması** bölümünü seçin.
   - Yönlendirme **URI'lerinde,** | **kamu istemcileri (mobil, masaüstü) bölümü için Önerilen Yönlendirme URI'leri,****https://login.microsoftonline.com/common/oauth2/nativeclient**
   - **Kaydet**’i seçin.
1. **API izinleri** bölümünü seçin
   - İzin **ekle** düğmesini tıklatın ve ardından,
   - **API'lerim** sekmesini seçin.
   - API'ler listesinde, web `AppModelv2-NativeClient-DotNet-TodoListService API`API'si için girdiğiniz adı veya adı seçin.
   - Önceden denetlenmediyse **access_as_user** iznini kontrol edin. Gerekirse arama kutusunu kullanın.
   - İzin **Ekle** düğmesini seçin

### <a name="configure-your-todolistclient-project"></a>*TodoListClient* projenizi yapılandırın

1. Uygulama *kayıt portalında,* **Genel Bakış** sayfasında Uygulama **(istemci) kimliğinin** değerini kopyalayın
1. **TodoListClient** projesinin kök klasöründe bulunan **app.config** dosyasını açın ve ardından `ida:ClientId` parametre değerine değeri yapıştırın

## <a name="run-your-project"></a>Projenizi çalıştırma

1. Projenizi çalıştırmak için basın. `<F5>` *TodoListClient'unuzun* açılması gerekir.
1. Sağ üstte **Oturum Aç'ı** seçin ve uygulamanızı kaydetmek için kullandığınız kullanıcıyla veya aynı dizinde bir kullanıcıyla oturum açın.
1. Bu noktada, ilk kez oturum açtıysanız, *TodoListService* Web Api'ye onay vermeniz istenebilir.
1. Oturum açma, *TodoListService* Web Api'sine erişmek ve *Yapılacaklar* listesini işlemek için *access_as_user* kapsamına giriş izni de ister.

## <a name="pre-authorize-your-client-application"></a>Müşteri başvurunuzu ön yetkilendirme

Diğer dizinlerden kullanıcıların web API'nize erişmesine izin vermenin yollarından biri, istemci uygulamalarının web API'nıza erişmesine *ön yetki* vermektir ve web API'nız için önceden *izin verilen* uygulamalar listesine istemci uygulamalarından gelen Uygulama kimliklerini eklemektir. Önceden yetkilendirilmiş bir istemci ekleyerek, kullanıcının web API'nizi kullanmasına izin vermesini gerektirmezsiniz. Web Uygulamanızı ön yetkilendirmek için aşağıdaki adımları izleyin::

1. Uygulama kayıt *portalına* geri dön ve **TodoListService'inizin**özelliklerini açın.
1. **API'yi Ortaya Çıkar** bölümünde, Yetkili istemci *uygulamaları* bölümü altındaki **istemci uygulaması ekle'yi** tıklatın.
1. *İstemci Kimliği* alanında, uygulamanın uygulama `TodoListClient` kimliğini yapıştırın.
1. Yetkili *kapsamlar* bölümünde, bu web API'sinin `api://<Application ID>/access_as_user`kapsamını seçin.
1. Sayfanın altındaki **Uygulama Ekle** düğmesine basın.

## <a name="run-your-project"></a>Projenizi çalıştırma

1. Projenizi çalıştırmak için basın. `<F5>` *TodoListClient'unuzun* açılması gerekir.
1. Sağ üstte **Oturum Aç'ı** (veya Önbellek/Oturum Aç'ı Temizle) seçin ve ardından kişisel bir Microsoft hesabı (live.com veya hotmail.com) veya iş veya okul hesabını kullanarak oturum açın.

## <a name="optional-restrict-sign-in-access-to-your-application"></a>İsteğe bağlı: Uygulamanızda oturum açma erişimini kısıtlama

Varsayılan olarak, bu kod örneğini indirdiğinizde ve uygulamayı önceki adımları izleyerek Azure Active Directory v2 bitiş noktasını kullanacak şekilde yapılandırdığınızda, hem outlook.com, live.com ve diğerleri gibi kişisel hesaplar hem de Azure AD ile tümleşik kuruluşlardaki Çalışma veya okul hesapları belirteçleri isteyebilir ve web API'nıza erişebilir.

Uygulamanızda kimlerin oturum açabileceğini kısıtlamak için seçeneklerden birini kullanın:

### <a name="option-1-restrict-access-to-a-single-organization-single-tenant"></a>Seçenek 1: Tek bir kuruluşa erişimi kısıtlama (tek kiracılı)

Uygulamanızın oturum açma erişimini yalnızca tek bir Azure AD kiracısında bulunan kullanıcı *hesaplarıyla* (o kiracının konuk hesapları da dahil olmak üzere) kısıtlayabilirsiniz. Bu *senaryo, iş satırı uygulamaları*için yaygın bir durumdur:

1. **App_Start\Startup.Auth** dosyasını açın ve metne geçen `OpenIdConnectSecurityTokenProvider` meta veri bitiş noktasının `"https://login.microsoftonline.com/{Tenant ID}/v2.0/.well-known/openid-configuration"` değerini değiştirin (Kiracı Adını `contoso.onmicrosoft.com`da kullanabilirsiniz.
2. Aynı dosyada, `ValidIssuer` `TokenValidationParameters` özelliği to `"https://sts.windows.net/{Tenant ID}/"` ve `ValidateIssuer` bağımsız değişkenüzerinde `true`ayarlayın.

### <a name="option-2-use-a-custom-method-to-validate-issuers"></a>Seçenek 2: İhraççıları doğrulamak için özel bir yöntem kullanın

**İhraççı Validator** parametresini kullanarak verenleri doğrulamak için özel bir yöntem uygulayabilirsiniz. Bu parametrenin nasıl kullanılacağı hakkında daha fazla bilgi için [TokenValidationParameters sınıfı](/dotnet/api/microsoft.identitymodel.tokens.tokenvalidationparameters?view=azure-dotnet)hakkında bilgi edinin.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
Microsoft kimlik platformunun desteklediği korumalı web API senaryosu hakkında daha fazla bilgi edinin:
> [!div class="nextstepaction"]
> [Korumalı web API senaryosu](scenario-protected-web-api-overview.md)
