---
title: ASP.NET Core ile Azure Uygulama Yapılandırması için hızlı başlatma | Microsoft Dokümanlar
description: ASP.NET Core uygulamalarıyla Azure Uygulama Yapılandırması'nı kullanmak için hızlı başlatma
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.date: 02/19/2020
ms.author: lcozzens
ms.openlocfilehash: 537dabe09c41012b9e15998ce3af8198dcfb62d3
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80245783"
---
# <a name="quickstart-create-an-aspnet-core-app-with-azure-app-configuration"></a>Quickstart: Azure Uygulama Yapılandırması ile ASP.NET Core uygulaması oluşturun

Bu hızlı başlatmada, ASP.NET Core uygulaması için depolama ve uygulama ayarlarının yönetimini merkezileştirmek için Azure Uygulama Yapılandırmasını kullanırsınız. ASP.NET Core, bir uygulama tarafından belirtilen bir veya daha fazla veri kaynağından gelen ayarları kullanarak tek bir anahtar değeri tabanlı yapılandırma nesnesi oluşturur. Bu veri kaynakları *yapılandırma sağlayıcıları*olarak bilinir. App Configuration'ın .NET Core istemcisi bir yapılandırma sağlayıcısı olarak uygulandığından, hizmet başka bir veri kaynağı gibi görünür.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
- [.NET Çekirdek SDK](https://dotnet.microsoft.com/download)

>[!TIP]
> Azure Bulut Kabuğu, bu makaledeki komut satırı yönergelerini çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur.  .NET Core SDK de dahil olmak üzere önceden yüklenmiş ortak Azure araçları vardır. Azure aboneliğinizde oturum açtıysanız, Azure [Bulut Shell'inizi](https://shell.azure.com) shell.azure.com başlatın.  [Belgelerimizi okuyarak](../cloud-shell/overview.md) Azure Bulut BulutU Hakkında Daha Fazla Bilgi Edinebilirsiniz

## <a name="create-an-app-configuration-store"></a>Uygulama Yapılandırma mağazası oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Aşağıdaki anahtar değeri çiftleri eklemek için **Configuration Explorer** > **Create** > Key**değerini** seçin:

    | Anahtar | Değer |
    |---|---|
    | TestApp:Ayarlar:BackgroundColor | Beyaz |
    | TestApp:Ayarlar:FontSize | 24 |
    | TestApp:Ayarlar:FontColor | Siyah |
    | TestApp:Ayarlar:Mesaj | Azure Uygulama Yapılandırmasından Veriler |

    **Etiket** ve **İçerik Türünü** şimdilik boş bırakın. **Uygula**’yı seçin.

## <a name="create-an-aspnet-core-web-app"></a>ASP.NET Core web uygulaması oluşturma

Yeni bir ASP.NET Core MVC web uygulaması projesi oluşturmak için [.NET Core komut satırı arabirimini (CLI)](https://docs.microsoft.com/dotnet/core/tools/) kullanın. [Azure Bulut Kabuğu](https://shell.azure.com) bu araçları sizin için sağlar.  Ayrıca Windows, macOS ve Linux platformlarında da kullanılabilir.

1. Projeniz için yeni bir klasör oluşturun. Bu hızlı başlangıç için, *testAppConfig*adını.

1. Yeni klasörde, yeni bir ASP.NET Core MVC web uygulaması projesi oluşturmak için aşağıdaki komutu çalıştırın:

```dotnetcli
dotnet new mvc --no-https
```

## <a name="add-secret-manager"></a>Gizli Yönetici Ekle

Gizli Yönetici'yi kullanmak `UserSecretsId` için *.csproj* dosyanıza bir öğe ekleyin.

1. *.csproj* dosyasını açın.

1.  Burada `UserSecretsId` gösterildiği gibi bir öğe ekleyin. Aynı GUID'i kullanabilir veya bu değeri kendi GUID'inizle değiştirebilirsiniz.

    > [!IMPORTANT]
    > `CreateHostBuilder`.NET `CreateWebHostBuilder` Core 3.0'ın yerini alır.  Ortamınıza göre doğru sözdizimini seçin.
    
    #### <a name="net-core-2x"></a>[.NET Çekirdek 2.x](#tab/core2x)
    
    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">
    
        <PropertyGroup>
            <TargetFramework>netcoreapp2.1</TargetFramework>
            <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
        </PropertyGroup>
    
        <ItemGroup>
            <PackageReference Include="Microsoft.AspNetCore.App" />
            <PackageReference Include="Microsoft.AspNetCore.Razor.Design" Version="2.1.2" PrivateAssets="All" />
        </ItemGroup>
    
    </Project>
    ```
    
    #### <a name="net-core-3x"></a>[.NET Çekirdek 3.x](#tab/core3x)
    
    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">
        
        <PropertyGroup>
            <TargetFramework>netcoreapp3.1</TargetFramework>
            <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
        </PropertyGroup>
    
    </Project>
    ```
    ---

1. *.csproj* dosyasını kaydedin.

Gizli Dizi Yöneticisi aracı, geliştirme işine yönelik hassas verileri proje ağacınızın dışında depolar. Bu yaklaşım, uygulama gizli dizilerini kaynak kodunun içinde yanlışlıkla paylaşmayı önlemeye yardımcı olur.

> [!TIP]
> Secret Manager hakkında daha fazla bilgi edinmek için lütfen [ASP.NET Core'da uygulama sırlarının güvenli bir şekilde saklanması bölümüne](https://docs.microsoft.com/aspnet/core/security/app-secrets) bakın

## <a name="connect-to-an-app-configuration-store"></a>Uygulama Yapılandırma mağazasına bağlanma

1. `Microsoft.Azure.AppConfiguration.AspNetCore` Aşağıdaki komutu çalıştırarak NuGet paketine bir başvuru ekleyin:

    ```dotnetcli
    dotnet add package Microsoft.Azure.AppConfiguration.AspNetCore
    ```

1. Projeniz için paketleri geri yüklemek için aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet restore
    ```

1. Secret Manager'a *ConnectionStrings:AppConfig* adlı bir sır ekleyin.

    Bu gizli, Uygulama Yapılandırma mağazanıza erişmek için bağlantı dizesini içerir. Aşağıdaki komuttaki değeri Uygulama Yapılandırma mağazanızın bağlantı dizesiyle değiştirin. Bağlantı dizesini Azure portalında **Access Keys** altında bulabilirsiniz.

    Bu komut, *.csproj* dosyası ile aynı dizinde yürütülmelidir.

    ```dotnetcli
    dotnet user-secrets set ConnectionStrings:AppConfig <your_connection_string>
    ```

    > [!IMPORTANT]
    > Bazı kabuklar, tırnak içinde ekte olmadığı sürece bağlantı dizesini bağlar. `dotnet user-secrets` Komutçıktısının tüm bağlantı dizesini gösterdiğinden emin olun. Yoksa, bağlantı dizesini tırnak içinde ekleyerek komutu yeniden çalıştırın.

    Gizli Yönetici yalnızca web uygulamasını yerel olarak test etmek için kullanılır. Uygulama [Azure Uygulama Hizmeti'ne](https://azure.microsoft.com/services/app-service/web)dağıtıldığında (örneğin, bağlantı dizesini depolamak için Gizli Yönetici yerine Uygulama Hizmeti'nde **Bağlantı Dizeleri** uygulama ayarını kullanırsınız).

    Yapılandırma API'sini kullanarak bu gizliye erişin. Bir kolon (:) desteklenen tüm platformlarda yapılandırma API'si ile yapılandırma adında çalışır. [Ortama göre Yapılandırma'ya](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/index?tabs=basicconfiguration&view=aspnetcore-2.0)bakın.

1. *Program.cs*açın ve .NET Core App Configuration sağlayıcısına bir başvuru ekleyin.

    ```csharp
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

1. Yöntemi `CreateWebHostBuilder` arayarak Uygulama Yapılandırması'nı kullanmak için yöntemi güncelleştirin. `config.AddAzureAppConfiguration()`

    > [!IMPORTANT]
    > `CreateHostBuilder`.NET `CreateWebHostBuilder` Core 3.0'ın yerini alır.  Ortamınıza göre doğru sözdizimini seçin.

    #### <a name="net-core-2x"></a>[.NET Çekirdek 2.x](#tab/core2x)

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(settings["ConnectionStrings:AppConfig"]);
            })
            .UseStartup<Startup>();
    ```

    #### <a name="net-core-3x"></a>[.NET Çekirdek 3.x](#tab/core3x)

    ```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        webBuilder.ConfigureAppConfiguration((hostingContext, config) =>
        {
            var settings = config.Build();
            config.AddAzureAppConfiguration(settings["ConnectionStrings:AppConfig"]);
        })
        .UseStartup<Startup>());
    ```

    ---

1. * <app root>/Views/Home'a* gidin ve *Index.cshtml'i*açın. İçeriğini aşağıdaki kodla değiştirin:

    ```HTML
    @using Microsoft.Extensions.Configuration
    @inject IConfiguration Configuration

    <style>
        body {
            background-color: @Configuration["TestApp:Settings:BackgroundColor"]
        }
        h1 {
            color: @Configuration["TestApp:Settings:FontColor"];
            font-size: @Configuration["TestApp:Settings:FontSize"];
        }
    </style>

    <h1>@Configuration["TestApp:Settings:Message"]</h1>
    ```

1. * <app root>/Views/Shared'a* gidin ve *_Layout.cshtml'i*açın. İçeriğini aşağıdaki kodla değiştirin:

    ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>@ViewData["Title"] - hello_world</title>

        <link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
        <link rel="stylesheet" href="~/css/site.css" />
    </head>
    <body>
        <div class="container body-content">
            @RenderBody()
        </div>

        <script src="~/lib/jquery/dist/jquery.js"></script>
        <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
        <script src="~/js/site.js" asp-append-version="true"></script>

        @RenderSection("Scripts", required: false)
    </body>
    </html>
    ```

## <a name="build-and-run-the-app-locally"></a>Uygulamayı yerel olarak oluşturma ve çalıştırma

1. .NET Core CLI'yi kullanarak uygulamayı oluşturmak için uygulamanızın kök dizinine gidin ve komut kabuğunda aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet build
    ```

1. Yapı başarıyla tamamlandıktan sonra, web uygulamasını yerel olarak çalıştırmak için aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet run
    ```

1. Yerel makinenizde çalışıyorsanız, 'ye `http://localhost:5000`gitmek için bir tarayıcı kullanın. Bu, yerel olarak barındırılan web uygulamasının varsayılan URL'sidir.  

Azure Bulut Kabuğu'nda çalışıyorsanız, Web *Önizleme* düğmesini ve ardından *Yapılandırma'yı*seçin.  

![Web Önizleme düğmesini bulma](./media/quickstarts/cloud-shell-web-preview.png)

Önizleme için bağlantı noktasını yapılandırmak istendiğinde , '5000' girin ve *Aç'ı*seçin ve göz atın.  Web sayfasında "Azure Uygulama Yapılandırmasından Veriler" okunur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir Uygulama Yapılandırma mağazası oluşturdunuz ve [App Configuration sağlayıcısı](https://go.microsoft.com/fwlink/?linkid=2074664)aracılığıyla ASP.NET Core web uygulaması yla kullandınız. yapılandırma ayarlarını dinamik olarak yenilecek şekilde ASP.NET Core uygulamanızı nasıl yapılandıracağımıöğrenmek için bir sonraki eğitime devam edin.

> [!div class="nextstepaction"]
> [Dinamik yapılandırmayı etkinleştirme](./enable-dynamic-configuration-aspnet-core.md)
