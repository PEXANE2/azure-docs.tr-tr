---
title: Öğretici-tek sayfalı bir uygulamadan ASP.NET Core Web API 'sine erişim Izni verme-Azure Active Directory B2C | Microsoft Docs
description: Bir .NET Core web API’sini korumak ve tek sayfalı bir uygulamadan çağırmak için Active Directory B2C kullanmaya yönelik öğretici.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: cbea29320c896637190766d1b2b60c09f7db5163
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68347172"
---
# <a name="tutorial-grant-access-to-an-aspnet-core-web-api-from-a-single-page-application-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C kullanarak tek sayfalı bir uygulamadan ASP.NET Core Web API 'sine erişim izni verme

Bu öğreticide, tek sayfalı bir uygulamadan Azure Active Directory (Azure AD) B2C korumalı ASP.NET Core Web API kaynağını çağırma gösterilmektedir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Web API'si uygulaması ekleme
> * Bir Web API 'SI için Kapsamları yapılandırma
> * Web API 'sine izin verme
> * Uygulamayı kullanmak için örneği yapılandırma

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

[Öğreticideki adımları ve önkoşulları doldurun: Azure Active Directory B2C](active-directory-b2c-tutorials-spa.md)kullanarak hesaplarla tek sayfalı uygulama kimlik doğrulamasını etkinleştirin.

## <a name="add-a-web-api-application"></a>Web API'si uygulaması ekleme

Web API kaynakları, erişim belirteci sunan istemci uygulamalarına göre korunan kaynak isteklerini kabul etmeden ve bunlara yanıt verebilmeleri için kiracınızda kayıtlı olmalıdır.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
4. **Uygulamalar**' ı seçin ve ardından **Ekle**' yi seçin.
5. Uygulama için bir ad girin. Örneğin, *webapi1*.
6. **Web uygulaması/Web API 'Si Ekle** ve **örtük akışa Izin ver**için **Evet**' i seçin.
7. **Yanıt URL 'si**için Azure AD B2C uygulamanızın isteklerinizin belirteçleri döndürmesi gereken bir uç nokta girin. Bu öğreticide, örnek yerel olarak çalışır ve tarihinde `https://localhost:5000`dinler.
8. **Uygulama kimliği URI 'si**için, Web API 'niz için kullanılan tanımlayıcıyı girin. Tam etki alanı ile birlikte URI tanımlayıcısı sizin için oluşturulur. Örneğin: `https://contosotenant.onmicrosoft.com/api`.
9.           **Oluştur**'a tıklayın.
10. Özellikler sayfasında, Web uygulamasını yapılandırırken kullanacağınız uygulama KIMLIĞINI kaydedin.

## <a name="configure-scopes"></a>Kapsamları yapılandırma

Kapsamlar, korumalı kaynaklara erişimi yönetmek için bir yol sağlar. Kapsamlar web API’si tarafından kapsam tabanlı erişim denetimi uygulamak için kullanılır. Örneğin, bazı kullanıcılar hem okuma hem de yazma erişimine sahipken diğer kullanıcıların salt okunur izinleri olabilir. Bu öğreticide web API’si için okuma izinlerini tanımlayacaksınız.

1. **Uygulamalar**' ı seçin ve ardından *webapi1*' ı seçin.
2. **Yayımlanan kapsamları**seçin.
3. **Kapsam**için girin `Hello.Read`ve açıklama için girin. `Read access to hello`
4. **Kapsam**için girin `Hello.Write`ve açıklama için girin. `Write access to hello`
5. **Kaydet**’e tıklayın.

Yayımlanan kapsamlar web API’sine bir istemci uygulama izni vermek için kullanılabilir.

## <a name="grant-permissions"></a>İzinleri verme

Bir uygulamadan korumalı bir Web API 'SI çağırmak için uygulamanızın API 'sine izin vermeniz gerekir. Önkoşul öğreticisinde, Azure AD B2C adlı *WebApp1*adlı bir Web uygulaması oluşturdunuz. Web API 'sini çağırmak için bu uygulamayı kullanın.

1. **Uygulamalar**' ı seçin ve ardından *WebApp1*' ı seçin.
2. **API erişimi**' ni seçin ve ardından **Ekle**' yi seçin.
3. **API Seç** açılan menüsünde *webapi1*' yi seçin.
4. **Kapsamları Seç** açılan menüsünde, daha önce tanımladığınız **Merhaba. Read** ve **Hello. Write** kapsamlarını seçin.
5. **Tamam**'ı tıklatın.

**Örnek tek sayfalı uygulamam**, korumalı **Hello Core API**’sini çağırmak için kaydedilir. Bir kullanıcı tek sayfalı uygulamayı kullanmak üzere Azure AD B2C kimliğini doğrular. Tek sayfalı uygulama, korumalı Web API 'sine erişmek için Azure AD B2C bir yetkilendirme izni alır.

## <a name="configure-the-sample"></a>Örneği yapılandırma

Web API 'SI kayıtlı olduğuna ve kapsamlarınızın tanımlandığından, Web API kodunu Azure AD B2C kiracınızı kullanacak şekilde yapılandırırsınız. Bu öğreticide, GitHub 'dan indirebileceğiniz örnek bir .NET Core Web uygulaması yapılandırırsınız. GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi/archive/master.zip) veya örnek web uygulamasını kopyalayın.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi.git
```

### <a name="configure-the-web-api"></a>Web API’sini yapılandırma

1. **B2C-WebAPI.sln** çözümünü Visual Studio’da açın.
2. **appsettings.json** dosyasını açın. Web api’sini kiracınızı kullanacak şekilde yapılandırmak için aşağıdaki değerleri güncelleştirin:

    ```javascript
    "AzureAdB2C": 
      {
        "Tenant": "<your tenant name>.onmicrosoft.com", 
        "ClientId": "<application-ID>",
        "Policy": "B2C_1_signupsignin1>",
        "ScopeRead": "Hello.Read"  
      },
    ```

#### <a name="enable-cors"></a>CORS'yi etkinleştirme

Tek sayfalı uygulamanızın ASP.NET Core Web API 'sini çağırmasını sağlamak için [CORS](https://docs.microsoft.com/aspnet/core/security/cors)'yi etkinleştirmeniz gerekir.

1. **Startup.cs** dosyasında `ConfigureServices()` yöntemine CORS ekleyin.

    ```csharp
    public void ConfigureServices(IServiceCollection services) {
      services.AddCors();
    ```

2. **Startup.cs** dosyasında, `Configure()` yönteminde CORS’yi yapılandırın.

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory) {
      app.UseCors(builder =>
        builder.WithOrigins("http://localhost:6420").AllowAnyHeader().AllowAnyMethod());
    ```

3. **Özellikler** bölümünden **launchSettings.json** dosyasını açın, **iisSettings** *applicationURL* ayarını bulun ve bağlantı noktası numarasını, API Yanıtlama URL’si `http://localhost:5000` için kayıtlı numaraya ayarlayın.

### <a name="configure-the-single-page-application"></a>Tek sayfalı uygulamayı yapılandırma

Tek sayfalı uygulama, Kullanıcı kaydı, oturum açma için Azure AD B2C kullanır ve korunan ASP.NET Core Web API 'sini çağırır. .NET Core Web API 'sini çağırmak için tek sayfalı uygulamayı güncelleştirin.

Uygulama ayarlarını değiştirmek için:

1. `index.html` dosyasını açın.
2. Örneği Azure AD B2C kiracı kayıt bilgileriyle yapılandırın. Aşağıdaki kodda, kiracı adınızı **b2cScopes** bölümüne ekleyin ve **webApi** değerini daha önce kaydetmiş olduğunuz *applicationURL* değeriyle değiştirin:

    ```javascript
    // The current application coordinates were pre-registered in a B2C tenant.
    var applicationConfig = {
        clientID: '<application-ID>',
        authority: "https://<your-tenant-name>.b2clogin.com/tfp/<your-tenant-name>.onmicrosoft.com/B2C_1_signupsignin1",
        b2cScopes: ["https://<Your tenant name>.onmicrosoft.com/api/Hello.Read"],
        webApi: 'http://localhost:5000/api/values',
    };
    ```

## <a name="run-the-spa-application-and-web-api"></a>SPA uygulamasını ve Web API 'sini çalıştırma

Hem Node. js tek sayfalı uygulamayı hem de .NET Core Web API 'sini çalıştırmanız gerekir.

### <a name="run-the-aspnet-core-web-api"></a>ASP.NET Core Web API’sini çalıştırma 

**F5**’e basarak Visual Studio’da **B2C-WebAPI.sln** çözümünün hatalarını ayıklayın.

Proje başlatıldığında varsayılan tarayıcınızda web api’sinin istekler için kullanılabilir olduğunu duyuran bir web sayfası gösterilir.

### <a name="run-the-single-page-app"></a>Tek sayfalı uygulamayı çalıştırma

1. Bir Node.js komut istemi başlatın.
2. Node.js örneğini içeren dizine değiştirin. Örneğin `cd c:\active-directory-b2c-javascript-msal-singlepageapp`
3. Aşağıdaki komutları çalıştırın:
    ```
    npm install && npm update
    node server.js
    ```

    Konsol penceresi, uygulamanın barındırıldığı bağlantı noktası numarasını görüntüler.
    
    ```
    Listening on port 6420...
    ```

4. Uygulamayı görüntülemek için adrese `http://localhost:6420` gitmek üzere bir tarayıcı kullanın.
5. [Tek sayfalı bir uygulamada Azure Active Directory B2C ile kullanıcılar için kimlik doğrulaması gerçekleştirme (Javascript)](active-directory-b2c-tutorials-spa.md) kullanılan e-posta adresi ve parolayı kullanarak oturum açın.
6. **API çağır**' a tıklayın.

Kaydolduktan veya bir kullanıcı hesabıyla oturum açtıktan sonra örnek, korumalı web api’sini çağırır ve bir sonuç döndürür.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Web API'si uygulaması ekleme
> * Bir Web API 'SI için Kapsamları yapılandırma
> * Web API 'sine izin verme
> * Uygulamayı kullanmak için örneği yapılandırma

> [!div class="nextstepaction"]
> [Öğretici: Azure Active Directory B2C ' deki uygulamalarınıza kimlik sağlayıcıları ekleyin](tutorial-add-identity-providers.md)
