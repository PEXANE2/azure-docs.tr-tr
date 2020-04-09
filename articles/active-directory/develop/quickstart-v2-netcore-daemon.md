---
title: Konsol uygulaması kimliğiyle Microsoft Graph araması & belirteç alın | Azure
titleSuffix: Microsoft identity platform
description: Bir jetonu nasıl alacağınızı ve bir .NET Core uygulamasından korunan bir Microsoft Graph API'sini nasıl arayacağınızı öğrenin
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 07/16/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: c4580708c827ebf700808974d01ed30ab9d2162d
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80883636"
---
# <a name="quickstart-acquire-a-token-and-call-microsoft-graph-api-using-console-apps-identity"></a>Quickstart: Konsol uygulamasının kimliğini kullanarak bir belirteç edinin ve Microsoft Graph API'yi arayın

Bu hızlı başlangıçta, uygulamanın kendi kimliğini kullanarak bir erişim jetonuna sahip bir .NET Core uygulaması yazmayı ve ardından dizindeki [kullanıcıların listesini](https://docs.microsoft.com/graph/api/user-list) görüntülemek için Microsoft Graph API'yi arayabilirsiniz. Bu senaryo, başsız, gözetimsiz iş veya windows hizmetinin kullanıcı kimliği yerine bir uygulama kimliğiyle çalışması gereken durumlar için yararlıdır. (Bkz. örnek bir resim için [nasıl çalışır?)](#how-the-sample-works)

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlatma gerektirir [.NET Core 2.2](https://www.microsoft.com/net/download/dotnet-core/2.2).

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme

> [!div renderon="docs" class="sxs-lookup"]
>
> Hızlı başlangıç uygulamanızı başlatmak için iki seçeneğiniz vardır: Express (Aşağıdaki Seçenek 1) ve Manuel (Seçenek 2)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırın ve ardından kod örneğinizi indirin
>
> 1. Yeni Azure [portalı - Uygulama kayıtları](https://portal.azure.com/?Microsoft_AAD_RegisteredApps=true#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/DotNetCoreDaemonQuickstartPage/sourceType/docs) bölmesine gidin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yönergeleri izleyerek yeni uygulamanızı yalnızca tek tıklamayla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma

> [!div renderon="docs"]
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
> 1. Geliştiriciler için Microsoft kimlik platformuna gidin [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfası.
> 1. **Yeni kayıt**seçin.
> 1. Bir başvuru sayfası **kaydedin,** başvurunuzun kayıt bilgilerini girin. 
> 1. **Ad** bölümüne, uygulama nın kullanıcılarına görüntülenecek anlamlı bir uygulama `Daemon-console`adı girin, örneğin , uygulamayı oluşturmak için **Kaydol'u** seçin.
> 1. Kaydedildikten sonra **Sertifikalar & Sırlar** menüsünü seçin.
> 1. **İstemci sırları**altında , seçin **+ Yeni istemci gizli**. Bir ad verin ve **Ekle'yi**seçin. Sırrı güvenli bir yerde kopyalayın. Kodunuzda kullanmak için ihtiyacınız olacaktır.
> 1. Şimdi, **API İzinleri** menüsünü seçin, **+ İzin düğmesi ekle'yi** seçin, **Microsoft Graph'ı**seçin.
> 1. **Uygulama izinlerini**seçin.
> 1. **Kullanıcı** düğümü altında **User.Read.All'ı**seçin ve **ardından İzin Ekle'yi** seçin

> [!div class="sxs-lookup" renderon="portal"]
> ### <a name="download-and-configure-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı indirin ve yapılandırın
> 
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>1. Adım: Uygulamanızı Azure portalında yapılandırma
> Bu hızlı başlatmanın işe yaraması için bir istemci sırrı oluşturmanız ve Grafik API'nin **User.Read.All** uygulama iznini eklemeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişiklikleri benim için yapın]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-netcore-daemon/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-your-visual-studio-project"></a>2. Adım: Visual Studio projenizi indirme

> [!div renderon="docs"]
> [Visual Studio projesini indirin](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/archive/master.zip)

> [!div class="sxs-lookup" renderon="portal"]
> Visual Studio 2019'u kullanarak projeyi çalıştırın.
> [!div renderon="portal" id="autoupdate" class="nextstepaction"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/archive/master.zip)

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`
    
> [!div renderon="docs"]
> #### <a name="step-3-configure-your-visual-studio-project"></a>3. Adım: Visual Studio projenizi yapılandırma
> 
> 1. Zip dosyasını diskin köküne yakın bir yerel klasöre (örneğin **C:\Azure-Samples**) ayıklayın.
> 1. Çözümü Visual Studio'da açın - **1-Call-MSGraph\daemon-console.sln** (isteğe bağlı).
> 1. **Appsettings.json'u** ve alanların `ClientId`değerlerini `Tenant` ve `ClientSecret` aşağıdakileri değiştirin:
> 
>    ```json
>    "Tenant": "Enter_the_Tenant_Id_Here",
>    "ClientId": "Enter_the_Application_Id_Here",
>    "ClientSecret": "Enter_the_Client_Secret_Here"
>    ```
>   Konumlar:
>   - `Enter_the_Application_Id_Here` - kaydettiğiniz uygulamanın **Uygulama (istemci) Kimliği** değeridir.
>   - `Enter_the_Tenant_Id_Here`- Bu değeri **Kiracı Kimliği** veya **Kiracı adı** ile değiştirin (örneğin, contoso.microsoft.com)
>   - `Enter_the_Client_Secret_Here`- Bu değeri adım 1'de oluşturulan istemci sırrıyla değiştirin.

> [!div renderon="docs"]
> > [!TIP]
> > **Uygulama (istemci) kimliği**, **Dizin (kiracı) Kimliği**değerlerini bulmak için, Azure portalındaki uygulamanın Genel **Bakış** sayfasına gidin. Yeni bir anahtar oluşturmak için **Sertifikalar & Sırlar** sayfasına gidin.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-admin-consent"></a>Adım 3: Yönetici onayı

> [!div renderon="docs"]
> #### <a name="step-4-admin-consent"></a>Adım 4: Yönetici onayı

Bu noktada uygulamayı çalıştırmayı denerseniz, *HTTP 403 -* Yasak `Insufficient privileges to complete the operation`hata: . Bunun nedeni, yalnızca *uygulama izninin* Yönetici onayı gerektirdiğinden olur, bu da dizininizin genel yöneticisinin uygulamanıza onay vermesi gerektiği anlamına gelir. Rolünüze bağlı olarak aşağıdaki seçeneklerden birini seçin:

##### <a name="global-tenant-administrator"></a>Genel kiracı yöneticisi

> [!div renderon="docs"]
> Genel bir kiracı yöneticiyseniz, Azure Portalı'nın Uygulama Kaydı'ndaki **(Önizleme) API İzinleri** sayfasına gidin ve **{Kiracı Adı} ({Kiracı Adı}** dizininizin adı için Hibe yöneticisi onayı seçin.

> [!div renderon="portal" class="sxs-lookup"]
> Genel bir yöneticiyseniz, **API İzinleri** sayfasına gidin **Enter_the_Tenant_Name_Here için Grant yönetici onayı** seçin
> > [!div id="apipermissionspage"]
> > [API İzinleri sayfasına gidin]()

##### <a name="standard-user"></a>Standart kullanıcı

Kiracınızın standart bir kullanıcısıysanız, genel bir yöneticiden uygulamanız için yönetici onayı vermesini istemeniz gerekir. Bunu yapmak için yöneticinize aşağıdaki URL'yi verin:

```url
https://login.microsoftonline.com/Enter_the_Tenant_Id_Here/adminconsent?client_id=Enter_the_Application_Id_Here
```

> [!div renderon="docs"]
>> Konumlar:
>> * `Enter_the_Tenant_Id_Here`- Bu değeri **Kiracı Kimliği** veya **Kiracı adı** ile değiştirin (örneğin, contoso.microsoft.com)
>> * `Enter_the_Application_Id_Here` - kaydettiğiniz uygulamanın **Uygulama (istemci) Kimliği** değeridir.

> [!NOTE]
> Önceki URL'yi kullanarak uygulamaya onay verdikten sonra *'AADSTS50011: Uygulama için yanıt adresi kayıtlı değildir'* hatasını görebilirsiniz. Bu uygulama ve URL bir yeniden yönlendirme URI yok çünkü bu olur - lütfen hatayı yoksay.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-4-run-the-application"></a>Adım 4: Uygulamayı çalıştırın

> [!div renderon="docs"]
> #### <a name="step-5-run-the-application"></a>Adım 5: Uygulamayı çalıştırın

Visual Studio kullanıyorsanız, uygulamayı çalıştırmak için **F5** tuşuna basın, aksi takdirde uygulamayı komut istemi veya konsol aracılığıyla çalıştırın:

```console
cd {ProjectFolder}\daemon-console\1-Call-Graph
dotnet run
```

> Konumlar:
> * *{ProjectFolder}* zip dosyasını ayıkladığınız klasördür. Örnek **C:\Azure-Örnekler\active-directory-dotnetcore-daemon-v2**

Sonuç olarak Azure REKLAM dizininizde bir kullanıcı listesi görmeniz gerekir.

> [!IMPORTANT]
> Bu hızlı başlatma uygulaması, kendisini gizli istemci olarak tanımlamak için bir istemci sırrı kullanır. İstemci sırrı, güvenlik nedenleriyle proje dosyalarınıza düz metin olarak eklenmiştir, çünkü uygulamayı üretim uygulaması olarak düşünmeden önce istemci sırrı yerine bir sertifika kullanmanız önerilir. Sertifikanın nasıl kullanılacağı hakkında daha fazla bilgi için bu örnek için GitHub deposunda [bu yönergeleri](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/#variation-daemon-application-using-client-credentials-with-certificates) görün.

## <a name="more-information"></a>Daha fazla bilgi

### <a name="how-the-sample-works"></a>Örnek nasıl çalışır?
![Bu hızlı başlatma tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-netcore-daemon/netcore-daemon-intro.svg)

### <a name="msalnet"></a>MSAL.NET

MSAL ([Microsoft.Identity.Client),](https://www.nuget.org/packages/Microsoft.Identity.Client)Microsoft kimlik platformu tarafından korunan bir API'ye erişmek için kullanılan kullanıcı ve istek belirteçleri oturum açmaiçin kullanılan kitaplıktır. Açıklandığı gibi, bu hızlı başlatma, yetkin izinler yerine uygulamanın kendi kimliğini kullanarak belirteçleri ister. Bu durumda kullanılan kimlik doğrulama akışı *[istemci kimlik bilgileri oauth akışı](v2-oauth2-client-creds-grant-flow.md)* olarak bilinir. İstemci kimlik bilgileri akışıile MSAL.NET nasıl kullanılacağı hakkında daha fazla bilgi için [bu makaleye](https://aka.ms/msal-net-client-credentials)bakın.

 Visual Studio'nun **Paket Yöneticisi Konsolunda**aşağıdaki komutu çalıştırarak MSAL.NET yükleyebilirsiniz:

```powershell
Install-Package Microsoft.Identity.Client
```

Alternatif olarak, Visual Studio kullanmıyorsanız, projenize MSAL eklemek için aşağıdaki komutu çalıştırabilirsiniz:

```console
dotnet add package Microsoft.Identity.Client
```

### <a name="msal-initialization"></a>MSAL başlatma

Şu kodu ekleyerek MSAL başvurusunu ekleyebilirsiniz:

```csharp
using Microsoft.Identity.Client;
```

Sonra şu kodu kullanarak MSAL'yi başlatın:

```csharp
IConfidentialClientApplication app;
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
                                          .WithClientSecret(config.ClientSecret)
                                          .WithAuthority(new Uri(config.Authority))
                                          .Build();
);
```

> | Konumlar: ||
> |---------|---------|
> | `config.ClientSecret` | Azure Portalı'ndaki uygulama için oluşturulan istemci sırrıdır. |
> | `config.ClientId` | **Uygulama (istemci) Kimliği**, Azure portalda kayıtlı uygulamadır. Bu değeri Azure portalda uygulamanın **Genel bakış** sayfasında bulabilirsiniz. |
> | `config.Authority`    | (İsteğe bağlı) Kullanıcının kimlik doğrulaması için STS bitiş noktası. Genellikle <https://login.microsoftonline.com/{tenant}> ,{kiracı} kiracınızın veya kiracı Kimliğinizin adı olan genel bulut için.|

Daha fazla bilgi için lütfen [başvuru belgelerine `ConfidentialClientApplication` ](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.iconfidentialclientapplication?view=azure-dotnet) bakın

### <a name="requesting-tokens"></a>Belirteç isteme

Uygulamanın kimliğini kullanarak belirteç istemek `AcquireTokenForClient` için şu yöntemi kullanın:

```csharp
result = await app.AcquireTokenForClient(scopes)
                  .ExecuteAsync();
```

> |Konumlar:| |
> |---------|---------|
> | `scopes` | İstenilen kapsamları içerir. Gizli istemciler için bu biçim, `{Application ID URI}/.default` istenen kapsamların Azure Portalı'nda ayarlanan uygulama nesnesinde statik olarak tanımlanan kapsamlar `{Application ID URI}` olduğunu `https://graph.microsoft.com`belirtmek için benzer biçimi kullanmalıdır (Microsoft Graph için, işaret eder). Özel Web API'leri için, `{Application ID URI}` Azure Portal'ın Uygulama Kaydı'ndaki (Önizleme) **api açığa** çıkarma bölümü altında tanımlanır. |

Daha fazla bilgi için lütfen [başvuru belgelerine `AcquireTokenForClient` ](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.confidentialclientapplication.acquiretokenforclient?view=azure-dotnet) bakın

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Daemon uygulamaları hakkında daha fazla bilgi edinmek için senaryo açılış sayfasına bakın

> [!div class="nextstepaction"]
> [Web API'leri çağıran Daemon uygulaması](scenario-daemon-overview.md)

Daemon uygulama öğreticisi için bkz:

> [!div class="nextstepaction"]
> [Daemon .NET Core konsol eğitimi](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2)

İzinler ve izinler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [İzinler ve İzinler](v2-permissions-and-consent.md)

Bu senaryonun auth akışı hakkında daha fazla bilgi için Oauth 2.0 istemci kimlik bilgileri akışına bakın:

> [!div class="nextstepaction"]
> [İstemci kimlik bilgileri Oauth akışı](v2-oauth2-client-creds-grant-flow.md)

Microsoft kimlik platformlarını geliştirmemize yardımcı olun. Kısa bir iki soruluk anketi tamamlayarak bize ne düşündüğünüzü söyleyin.

> [!div class="nextstepaction"]
> [Microsoft kimlik platformu anketi](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)
