---
title: 'Quickstart: C ile Sürükleyici Okuyucu başlattı bir web uygulaması oluşturun #'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, sıfırdan bir web uygulaması oluşturur ve Sürükleyici Reader API işlevselliğini eklersiniz.
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: quickstart
ms.date: 01/14/2020
ms.author: metan
ms.openlocfilehash: 8dd8459922caa9f765d59bc28fbf050b86834b46
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76845252"
---
# <a name="quickstart-create-a-web-app-that-launches-the-immersive-reader-c"></a>Quickstart: Sürükleyici Okuyucu (C#) başlatan bir web uygulaması oluşturun

[Immersive Reader,](https://www.onenote.com/learningtools) okuma anlama geliştirmek için kanıtlanmış teknikleri uygulayan kapsayıcı bir şekilde tasarlanmış bir araçtır.

Bu hızlı başlangıçta, sıfırdan bir web uygulaması oluşturun ve Sürükleyici Reader SDK'yı kullanarak Sürükleyici Okuyucu'yu entegre edebilirsiniz. Bu quickstart tam bir çalışma örneği [burada](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples/quickstart-csharp)mevcuttur.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads)
* Azure Etkin Dizin kimlik doğrulaması için yapılandırılan Sürükleyici Bir Reader kaynağı. Kurulumu için [bu yönergeleri](./how-to-create-immersive-reader.md) izleyin. Örnek proje özelliklerini yapılandırırken burada oluşturulan bazı değerlere ihtiyacınız olacaktır. Oturumunuzun çıktısını ileride başvurmak için bir metin dosyasına kaydedin.

## <a name="create-a-web-app-project"></a>Bir web uygulaması projesi oluşturma

Yerleşik Model-View-Controller ile ASP.NET Çekirdek Web Uygulaması şablonu ve Core 2.1'i ASP.NET kullanarak Visual Studio'da yeni bir proje oluşturun. Projeye "QuickstartSampleWebApp" adını verin.

![Yeni Proje](./media/quickstart-csharp/1-createproject.png)

![Yeni Projeyi Yapılandır](./media/quickstart-csharp/2-configureproject.png)

![Yeni ASP.NET Çekirdek Web Uygulaması](./media/quickstart-csharp/3-createmvc.png)

## <a name="set-up-authentication"></a>Kimlik doğrulamasını ayarlama

### <a name="configure-authentication-values"></a>Kimlik doğrulama değerlerini yapılandırma

_Solution Explorer'daki_ projeye sağ tıklayın ve Kullanıcı **Sırlarını Yönet'i**seçin. Bu _secrets.json_adlı bir dosya açılacaktır. Bu dosya kaynak denetiminde denetlenmez. [Burada](https://docs.microsoft.com/aspnet/core/security/app-secrets?view=aspnetcore-3.1&tabs=windows) daha fazla bilgi edinin. _Secrets.json_ içeriğini aşağıdakilerle değiştirerek, Sürükleyici Okuyucu kaynağınızı oluşturduğunuzda verilen değerleri sağlar.

```json
{
  "TenantId": "YOUR_TENANT_ID",
  "ClientId": "YOUR_CLIENT_ID",
  "ClientSecret": "YOUR_CLIENT_SECRET",
  "Subdomain": "YOUR_SUBDOMAIN"
}
```

### <a name="add-the-microsoftidentitymodelclientsactivedirectory-nuget-package"></a>Microsoft.IdentityModel.Clients.ActiveDirectory NuGet paketini ekleyin

Aşağıdaki kod **Microsoft.IdentityModel.Clients.ActiveDirectory** NuGet paketinden gelen nesneleri kullanır, bu nedenle projenizde bu pakete bir başvuru eklemeniz gerekir.

NuGet Paket Yöneticisi Konsolunu Araçlardan açın **-> NuGet Package Manager -> Paket Yöneticisi Konsolu** ve aşağıdaki komutu çalıştırın:

```powershell
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 5.2.0
```

### <a name="update-the-controller-to-acquire-the-token"></a>Belirteci elde etmek için denetleyiciyi güncelleştirme 

_Denetleyicileri\HomeController.cs'yi_açın ve dosyanın üst _kısmındaki ifadeleri kullandıktan_ sonra aşağıdaki kodu ekleyin.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Şimdi, denetleyiciyi _secrets.json'dan_Azure AD değerlerini elde edecek şekilde yapılandıracağız. _HomeController_ sınıfının üst kısmında, ```public class HomeController : Controller {```sonra , aşağıdaki kodu ekleyin.

```csharp
private readonly string TenantId;     // Azure subscription TenantId
private readonly string ClientId;     // Azure AD ApplicationId
private readonly string ClientSecret; // Azure AD Application Service Principal password
private readonly string Subdomain;    // Immersive Reader resource subdomain (resource 'Name' if the resource was created in the Azure portal, or 'CustomSubDomain' option if the resource was created with Azure CLI Powershell. Check the Azure portal for the subdomain on the Endpoint in the resource Overview page, for example, 'https://[SUBDOMAIN].cognitiveservices.azure.com/')

public HomeController(Microsoft.Extensions.Configuration.IConfiguration configuration)
{
    TenantId = configuration["TenantId"];
    ClientId = configuration["ClientId"];
    ClientSecret = configuration["ClientSecret"];
    Subdomain = configuration["Subdomain"];

    if (string.IsNullOrWhiteSpace(TenantId))
    {
        throw new ArgumentNullException("TenantId is null! Did you add that info to secrets.json?");
    }

    if (string.IsNullOrWhiteSpace(ClientId))
    {
        throw new ArgumentNullException("ClientId is null! Did you add that info to secrets.json?");
    }

    if (string.IsNullOrWhiteSpace(ClientSecret))
    {
        throw new ArgumentNullException("ClientSecret is null! Did you add that info to secrets.json?");
    }

    if (string.IsNullOrWhiteSpace(Subdomain))
    {
        throw new ArgumentNullException("Subdomain is null! Did you add that info to secrets.json?");
    }
}

/// <summary>
/// Get an Azure AD authentication token
/// </summary>
private async Task<string> GetTokenAsync()
{
    string authority = $"https://login.windows.net/{TenantId}";
    const string resource = "https://cognitiveservices.azure.com/";

    AuthenticationContext authContext = new AuthenticationContext(authority);
    ClientCredential clientCredential = new ClientCredential(ClientId, ClientSecret);

    AuthenticationResult authResult = await authContext.AcquireTokenAsync(resource, clientCredential);

    return authResult.AccessToken;
}

[HttpGet]
public async Task<JsonResult> GetTokenAndSubdomain()
{
    try
    {
        string tokenResult = await GetTokenAsync();

        return new JsonResult(new { token = tokenResult, subdomain = Subdomain });
    }
    catch (Exception e)
    {
        string message = "Unable to acquire Azure AD token. Check the debugger for more information.";
        Debug.WriteLine(message, e);
        return new JsonResult(new { error = message });
    }
}
```

## <a name="add-sample-content"></a>Örnek içerik ekleme
İlk olarak, _Açık Görünümler\Paylaşılan\Layout.cshtml_. Satırdan ```</head>```önce, aşağıdaki kodu ekleyin:

```html
@RenderSection("Styles", required: false)
```

Şimdi, bu web uygulamasına örnek içerik ekleyeceğiz. _Görünümler\Home\Index.cshtml'i_ açın ve otomatik olarak oluşturulan tüm kodu bu örnekle değiştirin:

```html
@{
    ViewData["Title"] = "Immersive Reader C# Quickstart";
}

@section Styles {
    <style type="text/css">
        .immersive-reader-button {
            background-color: white;
            margin-top: 5px;
            border: 1px solid black;
            float: right;
        }
    </style>
}

<div class="container">
    <button class="immersive-reader-button" data-button-style="iconAndText" data-locale="en"></button>

    <h1 id="ir-title">About Immersive Reader</h1>
    <div id="ir-content" lang="en-us">
        <p>
            Immersive Reader is a tool that implements proven techniques to improve reading comprehension for emerging readers, language learners, and people with learning differences.
            The Immersive Reader is designed to make reading more accessible for everyone. The Immersive Reader
            <ul>
                <li>
                    Shows content in a minimal reading view
                </li>
                <li>
                    Displays pictures of commonly used words
                </li>
                <li>
                    Highlights nouns, verbs, adjectives, and adverbs
                </li>
                <li>
                    Reads your content out loud to you
                </li>
                <li>
                    Translates your content into another language
                </li>
                <li>
                    Breaks down words into syllables
                </li>
            </ul>
        </p>
        <h3>
            The Immersive Reader is available in many languages.
        </h3>
        <p lang="es-es">
            El Lector inmersivo está disponible en varios idiomas.
        </p>
        <p lang="zh-cn">
            沉浸式阅读器支持许多语言
        </p>
        <p lang="de-de">
            Der plastische Reader ist in vielen Sprachen verfügbar.
        </p>
        <p lang="ar-eg" dir="rtl" style="text-align:right">
            يتوفر \"القارئ الشامل\" في العديد من اللغات.
        </p>
    </div>
</div>
```

Metnin tümünün, metnin dillerini açıklayan bir **lang** özniteliği ne sahip olduğuna dikkat edin. Bu öznitelik, Immersive Reader'ın ilgili dil ve dilbilgisi özellikleri sağlamasına yardımcı olur.

## <a name="add-javascript-to-handle-launching-the-immersive-reader"></a>Sürükleyici Reader'ı başlatmayı işlemek için JavaScript ekleyin

Immersive Reader kitaplığı, Sürükleyici Okuyucu'yu başlatma ve Sürükleyici Okuyucu düğmelerini oluşturma gibi işlevler sağlar. [Burada](https://docs.microsoft.com/azure/cognitive-services/immersive-reader/reference) daha fazla bilgi edinin.

_Views\Home\Index.cshtml'in_alt kısmında aşağıdaki kodu ekleyin:

```html
@section Scripts
{
    <script src="https://contentstorage.onenote.office.net/onenoteltir/immersivereadersdk/immersive-reader-sdk.1.0.0.js"></script>
    <script>
        function getTokenAndSubdomainAsync() {
            return new Promise(function (resolve, reject) {
                $.ajax({
                    url: "@Url.Action("GetTokenAndSubdomain", "Home")",
                    type: "GET",
                    success: function (data) {
                        if (data.error) {
                            reject(data.error);
                        } else {
                            resolve(data);
                        }
                    },
                    error: function (err) {
                        reject(err);
                    }
                });
            });
        }
    
        $(".immersive-reader-button").click(function () {
            handleLaunchImmersiveReader();
        });
    
        function handleLaunchImmersiveReader() {
            getTokenAndSubdomainAsync()
                .then(function (response) {
                    const token = response["token"];
                    const subdomain = response["subdomain"];
    
                    // Learn more about chunk usage and supported MIME types https://docs.microsoft.com/en-us/azure/cognitive-services/immersive-reader/reference#chunk
                    const data = {
                        title: $("#ir-title").text(),
                        chunks: [{
                            content: $("#ir-content").html(),
                            mimeType: "text/html"
                        }]
                    };
    
                    // Learn more about options https://docs.microsoft.com/en-us/azure/cognitive-services/immersive-reader/reference#options
                    const options = {
                        "onExit": exitCallback,
                        "uiZIndex": 2000
                    };
    
                    ImmersiveReader.launchAsync(token, subdomain, data, options)
                        .catch(function (error) {
                            alert("Error in launching the Immersive Reader. Check the console.");
                            console.log(error);
                        });
                })
                .catch(function (error) {
                    alert("Error in getting the Immersive Reader token and subdomain. Check the console.");
                    console.log(error);
                });
        }
    
        function exitCallback() {
            console.log("This is the callback function. It is executed when the Immersive Reader closes.");
        }
    </script>
}
```

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

Menü çubuğundan **Hata Ayıklama > Başlat Hata Ayıklama'yı**seçin veya uygulamayı başlatmak için **F5** tuşuna basın.

Tarayıcınızda şunları görmeniz gerekir:

![Örnek uygulama](./media/quickstart-csharp/4-buildapp.png)

## <a name="launch-the-immersive-reader"></a>Sürükleyici Okuyucuyu Başlat

"Sürükleyici Okuyucu" düğmesine tıkladığınızda, sayfadaki içerikle başlatılan Sürükleyici Okuyucuyu görürsünüz.

![Tam Ekran Okuyucu](./media/quickstart-csharp/5-viewimmersivereader.png)

## <a name="next-steps"></a>Sonraki adımlar

* Node.js kullanarak Sürükleyici Reader SDK ile başka neler yapabileceğinizi görmek için [Düğüm.js quickstart'ı](./quickstart-nodejs.md) görüntüleyin
* Python kullanarak Sürükleyici Reader SDK ile başka neler yapabileceğinizi görmek için [Python öğreticisini](./tutorial-python.md) görüntüleyin
* Swift'i kullanarak Sürükleyici Reader SDK ile başka neler yapabileceğinizi görmek için [iOS öğreticisini](./tutorial-ios-picture-immersive-reader.md) görüntüleyin
* [Immersive Reader SDK](https://github.com/microsoft/immersive-reader-sdk) ve [Immersive Reader SDK Referans](./reference.md) keşfedin