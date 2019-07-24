---
title: Öğretici-tek sayfalı bir uygulamadan ASP.NET Core Web API 'sine erişim Izni verme-Azure Active Directory B2C
description: .NET Core Web API 'sini korumak ve tek sayfalı bir Node. js uygulamasından API 'yi çağırmak için Active Directory B2C kullanmayı öğrenin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 07/24/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: b53ce30f4c49580bcd8ad3e259adf0300d8bd4a6
ms.sourcegitcommit: c71306fb197b433f7b7d23662d013eaae269dc9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68369306"
---
# <a name="tutorial-grant-access-to-an-aspnet-core-web-api-from-a-single-page-application-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C kullanarak tek sayfalı bir uygulamadan ASP.NET Core Web API 'sine erişim izni verme

Bu öğreticide, tek sayfalı bir uygulamadan Azure Active Directory (Azure AD) B2C korumalı ASP.NET Core Web API kaynağını çağırma gösterilmektedir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Web API'si uygulaması ekleme
> * Bir Web API 'SI için Kapsamları yapılandırma
> * Web API 'sine izin verme
> * Uygulamayı kullanmak için örneği yapılandırma

## <a name="prerequisites"></a>Önkoşullar

* [Öğreticideki adımları ve önkoşulları doldurun: Azure Active Directory B2C](active-directory-b2c-tutorials-spa.md)kullanarak tek sayfalı bir uygulamada kimlik doğrulamasını etkinleştirin.
* Visual Studio 2019 veya üzeri ya da Visual Studio Code
* .NET Core 2,2 veya üzeri
* Node.js

## <a name="add-a-web-api-application"></a>Web API'si uygulaması ekleme

Web API kaynakları, erişim belirteci sunan istemci uygulamalarına göre korunan kaynak isteklerini kabul etmeden ve bunlara yanıt verebilmeleri için kiracınızda kayıtlı olmalıdır.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
1. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
1. **Uygulamalar**' ı seçin ve ardından **Ekle**' yi seçin.
1. Uygulama için bir ad girin. Örneğin, *webapi1*.
1. **Web uygulaması/Web API 'Si Ekle** ve **örtük akışa Izin ver**için **Evet**' i seçin.
1. **Yanıt URL 'si**için Azure AD B2C uygulamanızın isteklerinizin belirteçleri döndürmesi gereken bir uç nokta girin. Bu öğreticide, örnek yerel olarak çalışır ve tarihinde `https://localhost:5000`dinler.
1. **Uygulama kimliği URI 'si**IÇIN gösterilen URI 'ye bir API uç noktası tanımlayıcısı girin. Öğreticide, tam URI `api` `https://contosotenant.onmicrosoft.com/api`şuna benzer olacak şekilde yazın.
1.           **Oluştur**'a tıklayın.
1. Özellikler sayfasını açmak için *webapi1* uygulamasını seçin.
1. Özellikler sayfasında gösterilen **uygulama kimliğini** kaydedin. Web uygulamasını yapılandırırken bu KIMLIğIN daha sonraki bir adımda olması gerekir.

## <a name="configure-scopes"></a>Kapsamları yapılandırma

Kapsamlar, korumalı kaynaklara erişimi yönetmek için bir yol sağlar. Kapsamlar web API’si tarafından kapsam tabanlı erişim denetimi uygulamak için kullanılır. Örneğin, bazı kullanıcılar hem okuma hem de yazma erişimine sahipken diğer kullanıcıların salt okunur izinleri olabilir. Bu öğreticide Web API 'SI için hem okuma hem de yazma izinlerini tanımlarsınız.

1. **Uygulamalar**' ı seçin ve daha önce açık değilse, Özellikler sayfasını açmak için *webapi1* öğesini seçin.
1. **Yayımlanan kapsamları**seçin.
1. **Kapsam**için girin `Hello.Read`ve **Açıklama**için girin `Read access to hello`.
1. **Kapsam**için girin `Hello.Write`ve **Açıklama**için girin `Write access to hello`.
1. **Kaydet**’i seçin.
1. Tek sayfalı uygulamayı yapılandırırken daha sonraki bir `Hello.Read` adımda kullanılacak kapsamın **tam kapsam değerini** kaydedin. Tam kapsam değeri öğesine `https://yourtenant.onmicrosoft.com/api/Hello.Read`benzerdir.

Yayımlanan kapsamlar web API’sine bir istemci uygulama izni vermek için kullanılabilir.

## <a name="grant-permissions"></a>İzinleri verme

Başka bir uygulamadan korumalı bir Web API 'SI çağırmak için, bu uygulama izinlerini Web API 'sine vermeniz gerekir.

Önkoşul öğreticisinde, *WebApp1*adlı bir Web uygulaması oluşturdunuz. Bu öğreticide, *webapi1*önceki bir bölümünde oluşturduğunuz Web API 'sini çağırmak için bu uygulamayı yapılandırırsınız.

1. Azure portal 'de B2C kiracınıza gidin
1. **Uygulamalar**' ı seçin ve ardından *WebApp1*' ı seçin.
1. **API erişimi**' ni seçin ve ardından **Ekle**' yi seçin.
1. **API Seç** açılan menüsünde *webapi1*' yi seçin.
1. **Kapsamları Seç** açılan menüsünde, daha önce tanımladığınız **Merhaba. Read** ve **Hello. Write** kapsamlarını seçin.
1.           **Tamam**'ı tıklatın.

Tek sayfalı Web uygulamanız, korumalı Web API 'sini çağırmak için kaydedilir. Bir kullanıcı tek sayfalı uygulamayı kullanmak üzere Azure AD B2C kimliğini doğrular. Tek sayfalı uygulama, korumalı Web API 'sine erişmek için Azure AD B2C bir yetkilendirme izni alır.

## <a name="configure-the-sample"></a>Örneği yapılandırma

Web API 'SI kayıtlı olduğuna ve kapsamlarınızın tanımlandığından, Web API kodunu Azure AD B2C kiracınızı kullanacak şekilde yapılandırırsınız. Bu öğreticide, GitHub 'dan indirdiğinizde örnek bir .NET Core Web uygulaması yapılandırırsınız.

[ Bir\*. zip arşivi indirin](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi/archive/master.zip) veya örnek Web API projesini GitHub 'dan kopyalayın.

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi.git
```

### <a name="configure-the-web-api"></a>Web API’sini yapılandırma

1. Visual Studio 'da *B2C-WebApi/* * appSettings. JSON** * dosyasını açın veya Visual Studio Code.
1. `AzureAdB2C` Bloğu, kiracı adınızı, Web API 'si uygulamasının uygulama kimliğini, kaydolma/oturum açma ilkenizin adını ve daha önce tanımladığınız kapsamları yansıtacak şekilde değiştirin. Blok aşağıdaki örneğe benzer olmalıdır (uygun `Tenant` ve `ClientId` değerleri ile):

    ```json
    "AzureAdB2C": {
      "Tenant": "<your-tenant-name>.onmicrosoft.com",
      "ClientId": "<webapi-application-ID>",
      "Policy": "B2C_1_signupsignin1",

      "ScopeRead": "Hello.Read",
      "ScopeWrite": "Hello.Write"
    },
    ```

#### <a name="enable-cors"></a>CORS'yi etkinleştirme

Tek sayfalı uygulamanızın ASP.NET Core Web API 'sini çağırmasını sağlamak için, Web API 'sinde [CORS](https://docs.microsoft.com/aspnet/core/security/cors) 'yi etkinleştirmeniz gerekir.

1. *Startup.cs* dosyasında `ConfigureServices()` yöntemine CORS ekleyin.

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddCors();
    ```

1. Ayrıca `ConfigureServices()` yöntemi içinde, `jwtOptions.Authority` değeri aşağıdaki belirteç verenin URI 'sine ayarlayın.

    B2C `<your-tenant-name>` kiracınızın adıyla değiştirin.

    ```csharp
    jwtOptions.Authority = $"https://<your-tenant-name>.b2clogin.com/{Configuration["AzureAdB2C:Tenant"]}/{Configuration["AzureAdB2C:Policy"]}/v2.0";
    ```

1. `Configure()` Yönteminde CORS 'yi yapılandırın.

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
    {
        app.UseCors(builder =>
            builder.WithOrigins("http://localhost:6420").AllowAnyHeader().AllowAnyMethod());
    ```

1. (Yalnızca Visual Studio) Çözüm Gezgini **Özellikler** altında, *launchsettings. JSON* dosyasını açın ve `iisExpress` sonra bloğu bulun.
1. (Yalnızca Visual Studio) Değeri, webapi1 uygulamasını önceki bir adımda kaydettiğinizde belirttiğiniz bağlantı noktası numarasıyla güncelleştirin.  `applicationURL` Örneğin:

    ```json
    "iisExpress": {
      "applicationUrl": "http://localhost:5000/",
      "sslPort": 0
    }
    ```

### <a name="configure-the-single-page-application"></a>Tek sayfalı uygulamayı yapılandırma

Serideki [önceki öğreticideki](active-directory-b2c-tutorials-spa.md) tek sayfalı uygulama (Spa), Kullanıcı kaydı ve oturum açma için Azure AD B2C kullanır ve *frabrikamb2c* demo kiracısı tarafından korunan ASP.NET Core Web API 'sini çağırır.

Bu bölümde, tek sayfalı uygulamayı Azure AD B2C kiracınız tarafından korunan ve yerel makinenizde çalıştırdığınız ASP.NET Core Web API 'sini  çağırmak üzere güncelleşolursunuz.

SPA 'daki ayarları değiştirmek için:

1. Önceki öğreticide indirdiğiniz veya Klonladığınız [Active-Directory-B2C-JavaScript-msal-singlepageapp][github-js-spa] projesinde *index. html* dosyasını açın.
1. Örneği, daha önce oluşturduğunuz *Hello. Read* kapsamı ve Web API 'sinin URL 'si ile yapılandırın.
    1. Tanımında değeri, kapsamın tam URI 'siyle  değiştirin (daha önce kaydettiğiniz tam kapsam değeri). `b2cScopes` `appConfig`
    1. `webApi` Değeri`applicationURL` , önceki bölümde belirttiğiniz değerle değiştirin.

    Tanım aşağıdaki kod bloğuna benzer olmalıdır (bunun `<your-tenant-name>`yerine kiracı adınızla): `appConfig`

    ```javascript
    // The current application coordinates were pre-registered in a B2C tenant.
    var appConfig = {
      b2cScopes: ["https://<your-tenant-name>.onmicrosoft.com/api/Hello.Read"],
      webApi: "http://localhost:5000/"
    };
    ```

## <a name="run-the-spa-and-web-api"></a>SPA ve Web API 'sini çalıştırma

Son olarak, hem ASP.NET Core Web API 'sini hem de Node. js tek sayfalı uygulamayı yerel makinenizde çalıştırırsınız. Ardından, tek sayfalı uygulamada oturum açıp korunan API 'ye bir istek başlatmak için bir düğmeye basın.

Her iki uygulama da bu öğreticide yerel olarak çalıştırılsa da, güvenli kaydolma/oturum açma için Azure AD B2C kullanırlar ve korunan Web API 'sine erişim izni verir.

### <a name="run-the-aspnet-core-web-api"></a>ASP.NET Core Web API 'sini çalıştırma

Visual Studio 'da, *B2C-WebAPI. sln* çözümünü derlemek ve hata ayıklamak için **F5** 'e basın. Proje başlatıldığında, varsayılan tarayıcınızda Web API 'sinin istekler için kullanılabildiğini duyurduğunu bildiren bir Web sayfası görüntülenir.

Visual Studio yerine `dotnet` CLI kullanmayı tercih ediyorsanız:

1. Bir konsol penceresi açın ve  *\*. csproj* dosyasını içeren dizine geçin. Örneğin:

    `cd active-directory-b2c-dotnetcore-webapi/B2C-WebApi`

1. Web API 'sini yürüterek `dotnet run`oluşturun ve çalıştırın.

    API çalışır duruma geldiğinde aşağıdakine benzer bir `NETSDK1059` çıktı görmeniz gerekir (öğreticide, uyarıları güvenle yoksayabilirsiniz):

    ```console
    $ dotnet run
    Hosting environment: Production
    Content root path: /home/user/active-directory-b2c-dotnetcore-webapi/B2C-WebApi
    Now listening on: http://localhost:5000
    Application started. Press Ctrl+C to shut down.
    ```

### <a name="run-the-single-page-app"></a>Tek sayfalı uygulamayı çalıştırma

1. Bir konsol penceresi açın ve Node. js örneğini içeren dizine geçin. Örneğin:

    `cd active-directory-b2c-javascript-msal-singlepageapp`

1. Aşağıdaki komutları çalıştırın:

    ```console
    npm install && npm update
    node server.js
    ```

    Konsol penceresi, uygulamanın barındırıldığı bağlantı noktası numarasını görüntüler.

    ```console
    Listening on port 6420...
    ```

1. Uygulamayı görüntülemek `http://localhost:6420` için tarayıcınızda sayfasına gidin.
1. [Önceki öğreticide](active-directory-b2c-tutorials-spa.md)kullandığınız e-posta adresini ve parolayı kullanarak oturum açın. Başarılı oturum açma sırasında `User 'Your Username' logged-in` iletiyi görmeniz gerekir.
1. **Web API 'Si çağır** düğmesini seçin. SPA, Azure AD B2C bir yetkilendirme izni edinir ve sonra, dizin sayfasının içeriğini göstermek için korunan Web API 'sine erişir:

    ```Output
    Web APi returned:
    "<html>\r\n<head>\r\n  <title>Azure AD B2C API Sample</title>\r\n ...
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Web API'si uygulaması ekleme
> * Bir Web API 'SI için Kapsamları yapılandırma
> * Web API 'sine izin verme
> * Uygulamayı kullanmak için örneği yapılandırma

Artık bir SPA 'nın korumalı bir Web API 'sinden kaynak isteği gördüğünüze göre, bu uygulama türlerinin birbirleriyle ve Azure AD B2C nasıl etkileşime gireceğini daha ayrıntılı bir şekilde anlayın.

> [!div class="nextstepaction"]
> [Active Directory B2C > kullanılabilecek uygulama türleri](active-directory-b2c-apps.md)

<!-- Links - EXTERNAL -->
[github-js-spa]: https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp
