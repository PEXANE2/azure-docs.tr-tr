---
title: 'Hızlı başlangıç: ile modern okuyucuyu Başlatan bir Web uygulaması oluşturmaC#'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, sıfırdan bir Web uygulaması oluşturup tam ekran okuyucusu API işlevini eklersiniz.
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: quickstart
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: a219d83551b63d564ee4296a7c28b0ac99adc159
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76122313"
---
# <a name="quickstart-create-a-web-app-that-launches-the-immersive-reader-c"></a>Hızlı başlangıç: tam ekran okuyucuyu (C#) başlatan bir Web uygulaması oluşturma

[Tam ekran okuyucu](https://www.onenote.com/learningtools) , okuma kavramasını geliştirmek için kendini kanıtlamış teknikler uygulayan, ve dahil tasarlanmış bir araçtır.

Bu hızlı başlangıçta, sıfırdan bir Web uygulaması oluşturur ve modern Okuyucu SDK 'sını kullanarak tam ekran okuyucuyu tümleştirin. Bu hızlı başlangıç için tam bir çalışma örneğine [buradan](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples/quickstart-csharp)ulaşabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads)
* Azure Active Directory kimlik doğrulaması için yapılandırılmış bir tam ekran okuyucu kaynağı. Kurulumunu yapmak için [Bu yönergeleri](./how-to-create-immersive-reader.md) izleyin. Örnek proje özellikleri yapılandırılırken burada oluşturulan bazı değerler gerekir. Daha sonra başvurmak üzere oturumunuzun çıkışını bir metin dosyasına kaydedin.

## <a name="create-a-web-app-project"></a>Web uygulaması projesi oluşturma

Visual Studio 'da, yerleşik Model-View-Controller ve ASP.NET Core 2,1 ASP.NET Core Web uygulaması şablonunu kullanarak yeni bir proje oluşturun. Projeyi "QuickstartSampleWebApp" olarak adlandırın.

![Yeni Proje](./media/quickstart-csharp/1-createproject.png)

![Yeni proje yapılandırma](./media/quickstart-csharp/2-configureproject.png)

![Yeni ASP.NET Core Web uygulaması](./media/quickstart-csharp/3-createmvc.png)

## <a name="set-up-authentication"></a>Kimlik doğrulamasını ayarlama

### <a name="configure-authentication-values"></a>Kimlik doğrulama değerlerini yapılandırma

_Çözüm Gezgini_ projeye sağ tıklayın ve **Kullanıcı gizli dizilerini Yönet**' i seçin. Bu, _gizli dizileri. JSON_adlı bir dosya açar. Bu dosya kaynak denetimine iade edilmedi. [Burada](https://docs.microsoft.com/aspnet/core/security/app-secrets?view=aspnetcore-3.1&tabs=windows) daha fazla bilgi edinin. Gizli dizi okuyucu kaynağınızı oluştururken verilen değerleri sağlayarak _gizli dizileri. JSON_ içeriğini aşağıdaki ile değiştirin.

```json
{
  "TenantId": "YOUR_TENANT_ID",
  "ClientId": "YOUR_CLIENT_ID",
  "ClientSecret": "YOUR_CLIENT_SECRET",
  "Subdomain": "YOUR_SUBDOMAIN"
}
```

### <a name="add-the-microsoftidentitymodelclientsactivedirectory-nuget-package"></a>Microsoft. IdentityModel. clients. ActiveDirectory NuGet paketini ekleyin

Aşağıdaki kod, **Microsoft. IdentityModel. clients. ActiveDirectory** NuGet paketindeki nesneleri kullanarak projenizdeki bu pakete bir başvuru eklemeniz gerekir.

**Araçlar-> NuGet Paket Yöneticisi-> paket yöneticisi konsolundan** NuGet Paket Yöneticisi konsolunu açın ve şu komutu çalıştırın:

```powershell
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 5.2.0
```

### <a name="update-the-controller-to-acquire-the-token"></a>Belirteci almak için denetleyiciyi güncelleştirme 

_Controllers\homecontroller.cs_dosyasını açın ve dosyanın en üstündeki _using_ deyimlerinden sonra aşağıdaki kodu ekleyin.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Şimdi, denetleyiciyi _gizli dizi. JSON_' dan Azure AD değerlerini alacak şekilde yapılandıracağız. _HomeController_ sınıfının üst kısmında, ```public class HomeController : Controller {```sonra aşağıdaki kodu ekleyin.

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

## <a name="add-sample-content"></a>Örnek içerik Ekle
İlk olarak, _Views\shared\layout.exe. cshtml_dosyasını açın. Satırı ```</head>```önce aşağıdaki kodu ekleyin:

```html
@RenderSection("Styles", required: false)
```

Şimdi bu Web uygulamasına örnek içerik ekleyeceğiz. _Views\home\ındex.cshtml_ dosyasını açın ve otomatik olarak oluşturulan tüm kodu şu örnekle değiştirin:

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

Metnin tümünde, metnin dillerini açıklayan bir **lang** özniteliği olduğuna dikkat edin. Bu öznitelik, modern okuyucunun ilgili dil ve dil bilgisi özelliklerini sağlamasına yardımcı olur.

## <a name="add-javascript-to-handle-launching-the-immersive-reader"></a>Derinlikli okuyucu başlatma tanıtıcısına JavaScript ekleme

Modern okuyucu kitaplığı, derinlikli okuyucu başlatma ve tam ekran okuyucu düğmelerini işleme gibi işlevler sağlar. [Burada](https://docs.microsoft.com/azure/cognitive-services/immersive-reader/reference) daha fazla bilgi edinin.

_Views\home\ındex.cshtml_altında aşağıdaki kodu ekleyin:

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

Menü çubuğundan **hata ayıkla > hata ayıklamayı Başlat**' ı seçin veya **F5** ' e basarak uygulamayı başlatın.

Tarayıcınızda şunları görmeniz gerekir:

![Örnek uygulama](./media/quickstart-csharp/4-buildapp.png)

## <a name="launch-the-immersive-reader"></a>Tam ekran okuyucuyu başlatın

"Modern okuyucu" düğmesine tıkladığınızda, sayfadaki içerikle birlikte modern okuyucu başlatılır.

![Tam Ekran Okuyucu](./media/quickstart-csharp/5-viewimmersivereader.png)

## <a name="next-steps"></a>Sonraki adımlar

* Node. js kullanarak modern Okuyucu SDK 'Sı ile neler yapabileceğinizi görmek için [Node. js hızlı](./quickstart-nodejs.md) başlangıcını görüntüleyin
* Python kullanarak modern Okuyucu SDK 'Sı ile neler yapabileceğinizi öğrenmek için [Python öğreticisini](./tutorial-python.md) görüntüleyin
* Swift kullanarak tam ekran okuyucu SDK 'Sı ile neler yapabileceğinizi öğrenmek için [iOS öğreticisini](./tutorial-ios-picture-immersive-reader.md) görüntüleyin
* [Modern Okuyucu SDK 'sını](https://github.com/microsoft/immersive-reader-sdk) ve [tam ekran okuyucu SDK başvurusunu](./reference.md) keşfet