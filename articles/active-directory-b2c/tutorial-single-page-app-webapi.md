---
title: "Öğretici: Tek sayfalı bir uygulamadan ASP.NET Core web API'sine erişim izni verme"
titleSuffix: Azure AD B2C
description: Bu eğitimde, bir .NET Core web API'sini korumak ve API'yi tek sayfalı Bir Düğüm.js uygulamasından aramak için Active Directory B2C'yi nasıl kullanacağınızı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 07/24/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: f6f9ff7bb0d504ecc163f6ce1f87477b1ea9c2d1
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78186160"
---
# <a name="tutorial-grant-access-to-an-aspnet-core-web-api-from-a-single-page-application-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C'yi kullanarak tek sayfalık bir uygulamadan ASP.NET Core web API'sine erişim izni verme

Bu öğretici, tek sayfalık bir uygulamadan Azure Active Directory B2C (Azure AD B2C) korumalı ASP.NET Core web API kaynağını nasıl çağırabileceğinizi gösterir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Web API'si uygulaması ekleme
> * Web API'si için kapsamları yapılandırma
> * Web API'sine izin verme
> * Uygulamayı kullanacak şekilde örneği yapılandırma

## <a name="prerequisites"></a>Ön koşullar

* [Tutorial: Azure Active Directory B2C'yi kullanarak tek sayfalık bir uygulamada kimlik doğrulamayı etkinleştirme](tutorial-single-page-app.md)adımlarını ve ön koşulları tamamlayın.
* Visual Studio 2019 veya sonrası veya Visual Studio Kodu
* .NET Core 2.2 veya sonrası
* Node.js

## <a name="add-a-web-api-application"></a>Web API'si uygulaması ekleme

[!INCLUDE [active-directory-b2c-appreg-webapi](../../includes/active-directory-b2c-appreg-webapi.md)]

## <a name="configure-scopes"></a>Kapsamları yapılandırma

Kapsamlar korumalı kaynaklara erişimi yönetmenin bir yolunu sunar. Kapsamlar web API’si tarafından kapsam tabanlı erişim denetimi uygulamak için kullanılır. Örneğin, bazı kullanıcılar hem okuma hem de yazma erişimine sahipken diğer kullanıcıların salt okunur izinleri olabilir. Bu öğreticide, web API'si için hem okuma hem de yazma izinlerini tanımlarsınız.

[!INCLUDE [active-directory-b2c-scopes](../../includes/active-directory-b2c-scopes.md)]

Tek sayfalı uygulamayı yapılandırırken, kapsamın daha sonraki bir adımda kullanılacağı `demo.read` **SCOPES** altındaki değeri kaydedin. Tam kapsam değeri `https://contosob2c.onmicrosoft.com/api/demo.read`.

## <a name="grant-permissions"></a>İzinleri verme

Başka bir uygulamadan korumalı web API'sini çağırmak için, bu uygulama izinlerini web API'sine vermeniz gerekir.

Ön koşul öğreticisinde, *webapp1*adında bir web uygulaması oluşturdunuz. Bu öğreticide, bu uygulamayı önceki bölümde oluşturduğunuz web API'sını aramak üzere yapılandırırsınız, *webapi1.*

[!INCLUDE [active-directory-b2c-permissions-api](../../includes/active-directory-b2c-permissions-api.md)]

Tek sayfalı web uygulamanız, korumalı web API'sini aramak için kayıtlıdır. Bir kullanıcı, tek sayfalı uygulamayı kullanmak için Azure AD B2C ile kimlik doğrulaması yaptı. Tek sayfalı uygulama, korumalı web API'sine erişmek için Azure AD B2C'den yetkilendirme izni alır.

## <a name="configure-the-sample"></a>Örneği yapılandırma

Artık web API'si kayıtlı ve kapsamları tanımlanmış olduğundan, web API kodunu Azure AD B2C kiracınızı kullanacak şekilde yapılandırırsınız. Bu eğitimde, GitHub'dan indirdiğiniz bir örneği .NET Core web uygulamasını yapılandırırsınız.

Bir [ \*.zip arşivi indirin](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi/archive/master.zip) veya GitHub'dan örnek web API projesini klonla.

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi.git
```

### <a name="configure-the-web-api"></a>Web API’sini yapılandırma

1. Visual Studio veya Visual Studio Code'da <em>B2C-WebApi/**appsettings.json** </em> dosyasını açın.
1. `AzureAdB2C` Kiracı adınızı, web API uygulamanızın uygulama kimliğini, kaydolma/kaydolma ilkenizin adını ve daha önce tanımladığınız kapsamları yansıtacak şekilde bloğu değiştirin. Blok aşağıdaki örneğe benzer olmalıdır `Tenant` (uygun `ClientId` ve değerlerle):

    ```json
    "AzureAdB2C": {
      "Tenant": "<your-tenant-name>.onmicrosoft.com",
      "ClientId": "<webapi-application-ID>",
      "Policy": "B2C_1_signupsignin1",

      "ScopeRead": "demo.read",
      "ScopeWrite": "demo.write"
    },
    ```

#### <a name="enable-cors"></a>CORS'yi etkinleştirme

Tek sayfalı uygulamanızın ASP.NET Core web API'yi aramasına izin vermek için, WEB API'sinde [CORS'u](https://docs.microsoft.com/aspnet/core/security/cors) etkinleştirmeniz gerekir.

1. *Startup.cs* dosyasında `ConfigureServices()` yöntemine CORS ekleyin.

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddCors();
    ```

1. Ayrıca `ConfigureServices()` yöntem içinde, `jwtOptions.Authority` aşağıdaki belirteç veren URI değerini ayarlayın.

    B2C kiracınızın adıyla değiştirin. `<your-tenant-name>`

    ```csharp
    jwtOptions.Authority = $"https://<your-tenant-name>.b2clogin.com/{Configuration["AzureAdB2C:Tenant"]}/{Configuration["AzureAdB2C:Policy"]}/v2.0";
    ```

1. Yöntemde `Configure()` CORS'i yapılandırın.

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
    {
        app.UseCors(builder =>
            builder.WithOrigins("http://localhost:6420").AllowAnyHeader().AllowAnyMethod());
    ```

1. (Sadece Visual Studio) Çözüm Gezgini'ndeki **Özellikler** altında *launchSettings.json* dosyasını `iisExpress` açın ve bloğu bulun.
1. (Sadece Visual Studio) `applicationURL` *Webapi1* uygulamasını daha önceki bir adımda kaydettirdiğinizde belirttiğiniz bağlantı noktası numarasıyla değeri güncelleştirin. Örnek:

    ```json
    "iisExpress": {
      "applicationUrl": "http://localhost:5000/",
      "sslPort": 0
    }
    ```

### <a name="configure-the-single-page-application"></a>Tek sayfalı uygulamayı yapılandırma

Serinin [önceki öğreticisinden](tutorial-single-page-app.md) tek sayfalık uygulama (SPA), kullanıcı kaydolma ve kaydolma için Azure AD B2C kullanır ve *frabrikamb2c* demo kiracı tarafından korunan ASP.NET Core web API'sını çağırır.

Bu bölümde, Azure AD B2C *kiracınız* tarafından korunan ve yerel makinenizde çalıştırdığınız ASP.NET Core web API'sını aramak için tek sayfalık uygulamayı güncellersiniz.

SPA'daki ayarları değiştirmek için:

1. Önceki öğreticide indirdiğiniz veya klonladığınız [active-directory-b2c-javascript-msal-singlepageapp][github-js-spa] projesinde *index.html* dosyasını açın.
1. Örneği daha önce oluşturduğunuz *demo.read* kapsamı ve web API'sının URL'si için URI ile yapılandırın.
    1. `appConfig` Tanımda, `b2cScopes` değeri kapsam için tam URI ile değiştirin (daha önce kaydettiğiniz **KAPSAM** değeri).
    1. `webApi` Web API uygulamasını daha önceki bir adımda kaydettiğinizde eklediğiniz yeniden yönlendirme URI değeriyle değiştirin.

    Tanım `appConfig` aşağıdaki kod bloğuna benzer olmalıdır (kiracı adınız `<your-tenant-name>`yerine:

    ```javascript
    // The current application coordinates were pre-registered in a B2C tenant.
    var appConfig = {
      b2cScopes: ["https://<your-tenant-name>.onmicrosoft.com/api/demo.read"],
      webApi: "http://localhost:5000/"
    };
    ```

## <a name="run-the-spa-and-web-api"></a>SPA ve web API'yi çalıştırın

Son olarak, hem ASP.NET Core web API'sını hem de Yerel makinenizde Düğüm.js tek sayfalık uygulamayı çalıştırın. Ardından, tek sayfalı uygulamada oturum açın ve korumalı API'ye istek başlatmak için bir düğmeye basın.

Her iki uygulama da bu öğreticide yerel olarak çalışmasına rağmen, güvenli kaydolma/kaydolma ve korumalı web API'sine erişim izni vermek için Azure AD B2C'yi kullanır.

### <a name="run-the-aspnet-core-web-api"></a>ASP.NET Core web API'sini çalıştırın

Visual Studio'da, *B2C-WebAPI.sln* çözümlerini oluşturmak ve hata ayıklamak için **F5** tuşuna basın. Proje başlatıldığında, varsayılan tarayıcınızda web API'sinin istekler için kullanılabilen bir web sayfası görüntülenir.

Visual Studio yerine `dotnet` CLI'yi kullanmayı tercih ederseniz:

1. Konsol penceresini açın ve * \*.csproj* dosyasını içeren dizine değiştirin. Örnek:

    `cd active-directory-b2c-dotnetcore-webapi/B2C-WebApi`

1. Web API'sini çalıştırarak `dotnet run`oluşturun ve çalıştırın.

    API çalışır durumdayken, aşağıdakilere benzer çıktı görmeniz gerekir (öğretici için, herhangi `NETSDK1059` bir uyarıyı güvenle yoksayabilirsiniz):

    ```console
    $ dotnet run
    Hosting environment: Production
    Content root path: /home/user/active-directory-b2c-dotnetcore-webapi/B2C-WebApi
    Now listening on: http://localhost:5000
    Application started. Press Ctrl+C to shut down.
    ```

### <a name="run-the-single-page-app"></a>Tek sayfalı uygulamayı çalıştırma

1. Bir konsol penceresi açın ve Düğüm.js örneğini içeren dizine değiştirin. Örnek:

    `cd active-directory-b2c-javascript-msal-singlepageapp`

1. Aşağıdaki komutları çalıştırın:

    ```console
    npm install && npm update
    node server.js
    ```

    Konsol penceresi, uygulamanın barındırıldığı yerin bağlantı noktası numarasını görüntüler.

    ```console
    Listening on port 6420...
    ```

1. Uygulamayı `http://localhost:6420` görüntülemek için tarayıcınızda gidin.
1. [Önceki öğreticide](tutorial-single-page-app.md)kullandığınız e-posta adresini ve parolayı kullanarak oturum açın. Başarılı bir oturum açtıktan `User 'Your Username' logged-in` sonra, iletiyi görmeniz gerekir.
1. Arama **Web API düğmesini** seçin. SPA, Azure AD B2C'den yetkilendirme hibesi alır ve dizin sayfasının içeriğini görüntülemek için korumalı web API'sine erişir:

    ```Output
    Web APi returned:
    "<html>\r\n<head>\r\n  <title>Azure AD B2C API Sample</title>\r\n ...
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Web API'si uygulaması ekleme
> * Web API'si için kapsamları yapılandırma
> * Web API'sine izin verme
> * Uygulamayı kullanacak şekilde örneği yapılandırma

Artık bir SPA'nın korumalı bir web API'sinden kaynak istediğini gördüğünüze göre, bu uygulama türlerinin birbirleriyle ve Azure AD B2C ile nasıl etkileşimde bulunduğunu daha iyi anlayabilirsiniz.

> [!div class="nextstepaction"]
> [Active Directory B2C >'da kullanılabilecek uygulama türleri](application-types.md)

<!-- Links - EXTERNAL -->
[github-js-spa]: https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp
