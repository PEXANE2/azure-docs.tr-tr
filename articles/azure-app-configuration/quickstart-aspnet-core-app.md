---
title: Azure Uygulama yapılandırması için ASP.NET Core ile hızlı başlangıç | Microsoft Docs
description: Bir ASP.NET Core uygulamasının uygulama ayarlarının depolanmasını ve yönetimini merkezileştirmek için Azure Uygulama yapılandırması ile bir ASP.NET Core uygulaması oluşturun.
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.devlang: csharp
ms.custom: devx-track-csharp
ms.topic: quickstart
ms.date: 02/19/2020
ms.author: lcozzens
ms.openlocfilehash: 41675eb1911eede750b5a9cdc19cfe49e4699bac
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88590311"
---
# <a name="quickstart-create-an-aspnet-core-app-with-azure-app-configuration"></a>Hızlı başlangıç: Azure Uygulama yapılandırmasıyla ASP.NET Core uygulaması oluşturma

Bu hızlı başlangıçta, bir ASP.NET Core uygulaması için uygulama ayarlarının depolanmasını ve yönetimini merkezileştirmek üzere Azure Uygulama yapılandırması 'nı kullanacaksınız. ASP.NET Core, bir uygulama tarafından belirtilen bir veya daha fazla veri kaynağından ayarları kullanarak tek bir anahtar-değer tabanlı yapılandırma nesnesi oluşturur. Bu veri kaynakları *yapılandırma sağlayıcıları*olarak bilinir. Uygulama yapılandırmasının .NET Core istemcisi bir yapılandırma sağlayıcısı olarak uygulandığından, hizmet başka bir veri kaynağı gibi görünür.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
- [.NET Core SDK](https://dotnet.microsoft.com/download)

>[!TIP]
> Azure Cloud Shell, bu makaledeki komut satırı talimatlarını çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur.  .NET Core SDK dahil olmak üzere önceden yüklenmiş ortak Azure araçları vardır. Azure aboneliğinizde oturum açtıysanız, shell.azure.com adresinden [Azure Cloud Shell](https://shell.azure.com) başlatın.  [Belgelerimizi okuyarak](../cloud-shell/overview.md) Azure Cloud Shell hakkında daha fazla bilgi edinebilirsiniz

## <a name="create-an-app-configuration-store"></a>Uygulama yapılandırma deposu oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. **Configuration Explorer**  >  **Create**  >  Aşağıdaki anahtar-değer çiftlerini eklemek için yapılandırma Gezgini**anahtar değeri** oluştur ' u seçin:

    | Anahtar | Değer |
    |---|---|
    | TestApp: ayarlar: BackgroundColor | Beyaz |
    | TestApp: ayarlar: FontSize | 24 |
    | TestApp: ayarlar: FontColor | Siyahi |
    | TestApp: ayarlar: Ileti | Azure Uygulama yapılandırmasından veriler |

    **Etiket** ve **içerik türü** şimdilik boş bırakın. **Uygula**’yı seçin.

## <a name="create-an-aspnet-core-web-app"></a>ASP.NET Core web uygulaması oluşturma

Yeni bir ASP.NET Core MVC web uygulaması projesi oluşturmak için [.NET Core komut satırı arabirimi 'ni (CLI)](https://docs.microsoft.com/dotnet/core/tools/) kullanın. [Azure Cloud Shell](https://shell.azure.com) , sizin için bu araçları sağlar.  Bunlar ayrıca Windows, macOS ve Linux platformları genelinde de mevcuttur.

1. Projeniz için yeni bir klasör oluşturun. Bu hızlı başlangıç için, *Testappconfig*olarak adlandırın.

1. Yeni klasörde, yeni bir ASP.NET Core MVC web uygulaması projesi oluşturmak için aşağıdaki komutu çalıştırın:

```dotnetcli
dotnet new mvc --no-https
```

## <a name="add-secret-manager"></a>Gizli dizi Yöneticisi ekleme

Gizli dizi Yöneticisi 'ni kullanmak için `UserSecretsId` *. csproj* dosyanıza bir öğesi ekleyin.

1. *. Csproj* dosyasını açın.

1.  `UserSecretsId`Burada gösterildiği gibi bir öğe ekleyin. Aynı GUID 'i kullanabilir veya bu değeri kendi kendinizinkini kullanarak değiştirebilirsiniz.

    > [!IMPORTANT]
    > `CreateHostBuilder``CreateWebHostBuilder`, .NET Core 3,0 ' de yer alır.  Ortamınıza göre doğru söz dizimini seçin.
    
    #### <a name="net-core-2x"></a>[.NET Core 2. x](#tab/core2x)
    
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
    
    #### <a name="net-core-3x"></a>[.NET Core 3. x](#tab/core3x)
    
    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">
        
        <PropertyGroup>
            <TargetFramework>netcoreapp3.1</TargetFramework>
            <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
        </PropertyGroup>
    
    </Project>
    ```
    ---

1. *. Csproj* dosyasını kaydedin.

Gizli Dizi Yöneticisi aracı, geliştirme işine yönelik hassas verileri proje ağacınızın dışında depolar. Bu yaklaşım, uygulama gizli dizilerini kaynak kodunun içinde yanlışlıkla paylaşmayı önlemeye yardımcı olur.

> [!TIP]
> Gizli yönetici hakkında daha fazla bilgi edinmek için lütfen [ASP.NET Core geliştirme sırasında uygulama gizli dizileri Için güvenli depolama](https://docs.microsoft.com/aspnet/core/security/app-secrets) bölümüne bakın

## <a name="connect-to-an-app-configuration-store"></a>Uygulama yapılandırma deposuna bağlanma

1. `Microsoft.Azure.AppConfiguration.AspNetCore`Aşağıdaki komutu çalıştırarak NuGet paketine bir başvuru ekleyin:

    ```dotnetcli
    dotnet add package Microsoft.Azure.AppConfiguration.AspNetCore
    ```

1. Projenizin paketlerini geri yüklemek için aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet restore
    ```

1. Gizli dizi adlı *ConnectionString: AppConfig* adında bir gizli dizi ekleyin.

    Bu gizli anahtar, uygulama yapılandırma deponuza erişmek için bağlantı dizesini içerir. Aşağıdaki komutta bulunan değeri, uygulama yapılandırma deponuzdaki bağlantı dizesiyle değiştirin. Bağlantı dizesini Azure portal **erişim tuşları** altında bulabilirsiniz.

    Bu komut, *.csproj* dosyası ile aynı dizinde yürütülmelidir.

    ```dotnetcli
    dotnet user-secrets set ConnectionStrings:AppConfig <your_connection_string>
    ```

    > [!IMPORTANT]
    > Bazı kabuklar, tırnak içine alınmadığı takdirde bağlantı dizesini keser. `dotnet user-secrets`Komutun çıktısının tüm bağlantı dizesini belirttiğinden emin olun. Aksi takdirde, bağlantı dizesini tırnak içine alarak komutu yeniden çalıştırın.

    Gizli dizi Yöneticisi yalnızca Web uygulamasını yerel olarak test etmek için kullanılır. Uygulama [Azure App Service](https://azure.microsoft.com/services/app-service/web)dağıtıldığında, bağlantı dizesini depolamak Için gizli yönetici yerine App Service **bağlantı dizeleri**  uygulama ayarını kullanın.

    Yapılandırma API 'sini kullanarak bu gizliliğe erişin. İki nokta (:) yapılandırma adında, desteklenen tüm platformlarda Yapılandırma API 'SI ile birlikte kullanılır. Bkz. [ortama göre yapılandırma](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/index?tabs=basicconfiguration&view=aspnetcore-2.0).

1. *Program.cs*'i açın ve .NET Core uygulama yapılandırma sağlayıcısına bir başvuru ekleyin.

    ```csharp
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

1. Yöntemini `CreateWebHostBuilder` çağırarak uygulama yapılandırmasını kullanmak için yöntemi güncelleştirin `config.AddAzureAppConfiguration()` .

    > [!IMPORTANT]
    > `CreateHostBuilder``CreateWebHostBuilder`, .NET Core 3,0 ' de yer alır.  Ortamınıza göre doğru söz dizimini seçin.

    #### <a name="net-core-2x"></a>[.NET Core 2. x](#tab/core2x)

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

    #### <a name="net-core-3x"></a>[.NET Core 3. x](#tab/core3x)

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

1. * <app root> /Views/Home* ' a gidin ve *Index. cshtml*dosyasını açın. İçeriğini şu kodla değiştirin:

    ```HTML
    @using Microsoft.Extensions.Configuration
    @inject IConfiguration Configuration

    <style>
        body {
            background-color: @Configuration["TestApp:Settings:BackgroundColor"]
        }
        h1 {
            color: @Configuration["TestApp:Settings:FontColor"];
            font-size: @Configuration["TestApp:Settings:FontSize"]px;
        }
    </style>

    <h1>@Configuration["TestApp:Settings:Message"]</h1>
    ```

1. * <app root> /Views/Shared* ' a gidin ve *_Layout. cshtml*dosyasını açın. İçeriğini şu kodla değiştirin:

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

## <a name="build-and-run-the-app-locally"></a>Uygulamayı yerel olarak derleyin ve çalıştırın

1. .NET Core CLI kullanarak uygulamayı derlemek için uygulamanızın kök dizinine gidin ve komut kabuğu 'nda aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet build
    ```

1. Oluşturma başarıyla tamamlandıktan sonra, Web uygulamasını yerel olarak çalıştırmak için aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet run
    ```

1. Yerel makinenizde çalışıyorsanız, ' ye gitmek için bir tarayıcı kullanın `http://localhost:5000` . Bu, yerel olarak barındırılan Web uygulamasının varsayılan URL 'sidir.  

Azure Cloud Shell çalışıyorsanız, *Web önizleme* düğmesini ve ardından *Yapılandır*' ı seçin.  

![Web önizlemesi düğmesini bulun](./media/quickstarts/cloud-shell-web-preview.png)

Önizleme için bağlantı noktasını yapılandırmak isteyip istemediğiniz sorulduğunda, ' 5000 ' girin ve *Aç ve araştır*' ı seçin.  Web sayfası "Azure Uygulama yapılandırmasından verileri" okuyacaktır.

![Hızlı başlangıç uygulaması başlatılıyor](./media/quickstarts/aspnet-core-app-launch-local-before.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir uygulama yapılandırma deposu oluşturdunuz ve bunu [uygulama yapılandırma sağlayıcısı](https://go.microsoft.com/fwlink/?linkid=2074664)aracılığıyla bir ASP.NET Core Web uygulamasıyla kullandınız. Yapılandırma ayarlarını dinamik olarak yenilemek üzere ASP.NET Core uygulamanızı nasıl yapılandıracağınızı öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Dinamik yapılandırmayı etkinleştirme](./enable-dynamic-configuration-aspnet-core.md)
