---
title: Kimlik doğrulaması ve yetkilendirme için Azure AD ile tümleştirilen bir .NET web API'si oluşturma | Microsoft Docs
description: Kimlik doğrulaması ve yetkilendirme için Azure AD ile tümleştirilen bir .NET MVC web API'si nasıl oluşturulur?
services: active-directory
documentationcenter: .net
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 67e74774-1748-43ea-8130-55275a18320f
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 07/15/2019
ms.author: ryanwi
ms.reviewer: jmprieur, andret
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5375d47c1b012a1c808a1115b7c902d99b05bf9d
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68304714"
---
# <a name="quickstart-build-a-net-web-api-that-integrates-with-azure-ad-for-authentication-and-authorization"></a>Hızlı Başlangıç: Kimlik doğrulama ve yetkilendirme için Azure AD ile tümleşen bir .NET Web API 'SI oluşturun

[!INCLUDE [active-directory-develop-applies-v1](../../../includes/active-directory-develop-applies-v1.md)]

Korumalı kaynaklara erişim sağlayan bir uygulama derliyorsanız, söz konusu kaynaklara istenmeyen erişimi nasıl önleyeceğinizi bilmeniz gerekir. Azure Active Directory (Azure AD), yalnızca birkaç kod satırıyla OAuth 2.0 taşıyıcı erişim belirteçleri kullanarak web API'sini korumayı basitleştirir ve rahatlatır.

ASP.NET web uygulamalarında, bu korumayı .NET Framework 4.5'e eklenmiş topluluk tarafından yönlendirilen OWIN ara yazılımının Microsoft uyarlamasını kullanarak gerçekleştirebilirsiniz. Burada, OWIN kullanarak aşağıdakileri gerçekleştiren bir "To Do List" web API'si oluşturacağız:

* Hangi API'lerin korunacağını belirleme.
* Web API'si çağrılarının geçerli bir erişim belirteci içerdiğini doğrulama.

Bu hızlı başlangıçta, To Do List API'sini oluşturacak ve şunları yapmayı öğreneceksiniz:

1. Bir uygulamayı Azure AD'ye kaydedin.
2. Uygulamayı OWIN kimlik doğrulaması işlem hattını kullanacak şekilde ayarlama.
3. Web API'sini çağırmak için bir istemci uygulaması yapılandırma.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için şu önkoşulları tamamlayın:

* [Uygulama çatısını indirin](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) veya [tamamlanmış örneği indirin](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). Bunların hepsi Visual Studio 2013 çözümüdür.
* Uygulamanızı kaydedeceğiniz bir Azure AD kiracınız olmalıdır. Henüz kiracınız yoksa, [nasıl alacağınızı öğrenin](quickstart-create-new-tenant.md).

## <a name="step-1-register-an-application-with-azure-ad"></a>1\. adım: Bir uygulamayı Azure AD 'ye kaydetme

Uygulamanızın güvenliğini sağlamaya yardımcı olmak için, ilk olarak kiracınızda bir uygulama oluşturmalı ve Azure AD'de birkaç önemli bilgi parçası sağlamalısınız.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sayfanın sağ üst köşesinden hesabınızı seçtikten sonra **Dizin değiştir** gezintisini seçerek ve sonra da uygun kiracıyı belirterek Azure AD kiracınızı seçin.
    * Hesabınızın altında tek bir Azure AD kiracısı varsa veya uygun Azure AD kiracısını zaten seçtiyseniz, bu adımı atlayın.

3. Sol taraftaki gezinti bölmesinde **Azure Active Directory**'yi seçin.
4. **Uygulama kayıtları**öğesini seçin ve ardından **Yeni kayıt**' ı seçin.
5. **Bir uygulamayı kaydet** sayfası göründüğünde, uygulamanız için bir ad girin. Örneğin, "yapılacaklar listesi hizmeti".
**Desteklenen hesap türleri**altında, **herhangi bir kurumsal dizin ve kişisel Microsoft hesabında hesaplar**' ı seçin.
6. **Yeniden yönlendirme URI 'si** bölümünün altında **Web** platformu ' nu seçin ve değeri olarak `https://localhost:44321/` ayarlayın (Azure AD 'nin belirteçleri döndürecek konum).
7. Bittiğinde **Kaydet**’i seçin. Uygulamaya **genel bakış** sayfasında, **uygulama (istemci) kimliği** değerini aklınızda edin.
8. **BIR API 'Yi kullanıma** sunma ' yı seçip **Kapsam Ekle**' ye tıklayın.
9. **Kaydet ve devam et ' i**seçerek ÖNERILEN uygulama kimliği URI 'sini (API://{ClientID}) kabul edin.
10. Aşağıdaki parametreleri girin:
    1. **Kapsam adı**için "access_as_user" yazın.
    1. **Yöneticiler ve kullanıcılar** seçeneğinin **Izin verebilen kim**için seçildiğinden emin olun.
    1. **Yönetici onayı görünen adı**' nda "Kullanıcı olarak erişim TodoListService" yazın.
    1. **Yönetici onayı açıklama** türü "TodoListService Web API 'sine Kullanıcı olarak erişir".
    1. **Kullanıcı izni görünen adı** "Kullanıcı olarak erişim TodoListService" yazın.
    1. **Kullanıcı onayı açıklama** türü "TodoListService Web API 'sine Kullanıcı olarak erişir" yazın.
    1. **Durum**' da **etkin**' i seçin.
11. Yapılandırmayı kaydetmek için **Kapsam Ekle** ' yi seçin. Portalı açık bırakın, çünkü kısa süre sonra istemcinizi de kaydetmeniz gerekecektir.

## <a name="step-2-set-up-the-app-to-use-the-owin-authentication-pipeline"></a>2\. adım: Uygulamayı OWıN kimlik doğrulama işlem hattını kullanacak şekilde ayarlama

Gelen istekleri ve belirteçleri doğrulamak için, uygulamanızı Azure AD ile iletişim kuracak şekilde ayarlamanız gerekir.

1. Başlangıç olarak, çözümü açın ve Paket Yöneticisi Konsolu'nu kullanarak TodoListService projesine OWIN ara yazılımı NuGet paketlerini ekleyin.

    ```
    Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
    Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
    ```

2. TodoListService projesine `Startup.cs` adlı bir OWIN Startup sınıfı ekleyin.  Projeye sağ tıklayın, **Ekle > Yeni Öğe**'yi seçin, ardından **OWIN**'i aratın. OWIN ara yazılımı, uygulamanız başlatıldığında `Configuration(…)` yöntemini çağırır.

3. Sınıf bildirimini `public partial class Startup` olarak değiştirin. Başka bir dosyada bu sınıfın bir kısmını sizin için zaten uygulamıştır. `Configuration(…)` yönteminde, web uygulamanızda kimlik doğrulamasını ayarlamak için `ConfigureAuth(…)` çağrısı yapın.

    ```csharp
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
    ```

4. `App_Start\Startup.Auth.cs` dosyasını açın ve `ConfigureAuth(…)` yöntemini uygulayın. `WindowsAzureActiveDirectoryBearerAuthenticationOptions` içinde sağladığınız parametreler, uygulamanızın Azure AD ile iletişim kurması için koordinat işlevi görecektir. Bunları kullanmak için `System.IdentityModel.Tokens` ad alanındaki sınıfları kullanmanız gerekir.

    ```csharp
    using Microsoft.IdentityModel.Tokens;
    ```

    ```csharp
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseWindowsAzureActiveDirectoryBearerAuthentication(
            new WindowsAzureActiveDirectoryBearerAuthenticationOptions
            {
                 Tenant = ConfigurationManager.AppSettings["ida:Tenant"],
                 TokenValidationParameters = new TokenValidationParameters
                 {
                    ValidAudience = ConfigurationManager.AppSettings["ida:Audience"]
                 }
            });
    }
    ```

5. Denetleyicilerinizi ve eylemlerinizi JSON Web Token (JWT) taşıyıcı kimlik doğrulamasıyla korumaya yardımcı olmak için `[Authorize]` özniteliklerini kullanın. `Controllers\TodoListController.cs` sınıfına bir yetkilendirme etiketi ekleyin; bu etiket kullanıcıyı bu sayfaya erişmeden önce oturum açmaya zorlar.

    ```csharp
    [Authorize]
    public class TodoListController : ApiController
    {
    ```

    Yetkili bir kullanıcı `TodoListController` API'lerinden birini başarıyla çağırdığında, eylemin çağrıyı yapanla ilgili bilgilere erişmesi gerekebilir. OWIN, `ClaimsPrincpal` nesnesi yoluyla taşıyıcı belirtecinin içindeki taleplere erişim sağlar.  

6. Web API'leriyle ilgili yaygın bir gereksinim de, kullanıcının To Do List Service uygulamasına erişmek için gereken izinlere onay verdiğinden emin olmak üzere belirteçte bulunan "kapsamların" doğrulanmasıdır.

    ```csharp
    public IEnumerable<TodoItem> Get()
    {
        // user_impersonation is the default permission exposed by applications in Azure AD
        if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
        {
            throw new HttpResponseException(new HttpResponseMessage {
              StatusCode = HttpStatusCode.Unauthorized,
              ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found"
            });
        }
        ...
    }
    ```

7. TodoListService projesinin kökündeki `web.config` dosyasını açın ve `<appSettings>` bölümüne yapılandırma değerlerinizi girin.
    * `ida:Tenant`, Azure AD kiracınızın adıdır (örneğin, contoso.onmicrosoft.com).
    * `ida:Audience`, Azure portalına girdiğiniz uygulamanın Uygulama Kimliği URI'sidir.

## <a name="step-3-configure-a-client-application-and-run-the-service"></a>3\. adım: İstemci uygulaması yapılandırma ve hizmeti çalıştırma

To Do List Service'i iş başında görebilmek için önce To Do List istemcisini Azure AD'den belirteçleri alacak ve hizmete çağrı yapabilecek şekilde yapılandırmanız gerekir.

1. [Azure portalına](https://portal.azure.com) geri dönün.
1. Azure AD kiracınızda yeni bir uygulama kaydı oluşturun.  Uygulamanızı kullanıcılara açıklayan bir **ad** girin, **yeniden yönlendirme URI** değeri `https://TodoListClient/` girin ve açılan menüde **ortak istemci (mobil ve Masaüstü)** seçeneğini belirleyin.
1. Kaydı bitirdikten sonra, Azure AD uygulamanıza benzersiz bir uygulama kimliği atar. Bu değeri sonraki adımlarda kullanacağınız için, uygulama sayfasından kopyalayın.
1. **API izinleri**' ni seçin ve **izin ekleyin**.  **Yapılacaklar listesi hizmetini**bulun ve seçin, **temsilci Izinleri**altına **user_impersonation Access TodoListService** iznini ekleyin ve ardından **izin Ekle**' yi seçin.
1. Visual Studio'da, TodoListClient projesinde `App.config` dosyasını açın ve `<appSettings>` bölümüne kendi yapılandırma değerlerinizi girin.

    * `ida:Tenant`, Azure AD kiracınızın adıdır (örneğin, contoso.onmicrosoft.com).
    * `ida:ClientId`, Azure portalından kopyaladığınız uygulama kimliğidir.
    * `todo:TodoListResourceId`, Azure portalına girdiğiniz To Do List Service uygulamasının Uygulama Kimliği URI'sidir.

1. Her projeyi temizleyin, derleyin ve çalıştırın.
1. Henüz yapmadıysanız, kiracınızda *.onmicrosoft.com etki alanıyla yeni bir kullanıcı oluşturun.
1. Bu kullanıcıyla To Do List istemcisinde oturum açın ve kullanıcının yapılacaklar listesine birkaç görev ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Tamamlanan örneği, başvuru için (yapılandırma değerleriniz olmadan) [GitHub](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip)'dan indirin. Artık diğer kimlik senaryolarına ilerleyebilirsiniz.
