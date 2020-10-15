---
title: Azure Uygulama yapılandırması için ASP.NET Core ile hızlı başlangıç | Microsoft Docs
description: Bir ASP.NET Core uygulamasının uygulama ayarlarının depolanmasını ve yönetimini merkezileştirmek için Azure Uygulama yapılandırması ile bir ASP.NET Core uygulaması oluşturun.
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.devlang: csharp
ms.custom: devx-track-csharp, contperfq1
ms.topic: quickstart
ms.date: 09/25/2020
ms.author: lcozzens
ms.openlocfilehash: 13283a9531804502b8a8d72e615be955b413658c
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92075850"
---
# <a name="quickstart-create-an-aspnet-core-app-with-azure-app-configuration"></a>Hızlı başlangıç: Azure Uygulama yapılandırmasıyla ASP.NET Core uygulaması oluşturma

Bu hızlı başlangıçta, bir ASP.NET Core uygulamasının uygulama ayarlarının depolanmasını ve yönetimini merkezileştirmek için Azure Uygulama yapılandırması 'nı kullanacaksınız. ASP.NET Core, bir uygulama tarafından belirtilen bir veya daha fazla veri kaynağından ayarları kullanarak tek, anahtar-değer tabanlı bir yapılandırma nesnesi oluşturur. Bu veri kaynakları *yapılandırma sağlayıcıları*olarak bilinir. Uygulama yapılandırmasının .NET Core istemcisi bir yapılandırma sağlayıcısı olarak uygulandığından, hizmet başka bir veri kaynağı gibi görünür.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/dotnet)
* [.NET Core SDK](https://dotnet.microsoft.com/download)

> [!TIP]
> Azure Cloud Shell, bu makaledeki komut satırı talimatlarını çalıştırmak için kullanabileceğiniz ücretsiz ve etkileşimli bir kabuktur. .NET Core SDK dahil olmak üzere önceden yüklenmiş ortak Azure araçları vardır. Azure aboneliğinizde oturum açıyorsanız, shell.azure.com adresinden [Azure Cloud Shell](https://shell.azure.com) başlatın. [Belgelerimizi okuyarak](../cloud-shell/overview.md) Azure Cloud Shell hakkında daha fazla bilgi edinebilirsiniz

## <a name="create-an-app-configuration-store"></a>Uygulama yapılandırma deposu oluşturma

[!INCLUDE[Azure App Configuration resource creation steps](../../includes/azure-app-configuration-create.md)]

7. **Operations**  >  **Configuration explorer**  >  **Create**  >  Aşağıdaki anahtar-değer çiftlerini eklemek için Operations Configuration Explorer**anahtar değeri** oluştur ' u seçin:

    | Anahtar                                | Değer                               |
    |------------------------------------|-------------------------------------|
    | `TestApp:Settings:BackgroundColor` | *#FFF*                              |
    | `TestApp:Settings:FontColor`       | *#000*                              |
    | `TestApp:Settings:FontSize`        | *24*                                |
    | `TestApp:Settings:Message`         | *Azure Uygulama yapılandırmasından veriler* |

    **Etiket** ve **içerik türü** şimdilik boş bırakın. **Apply** (Uygula) seçeneğini belirleyin.

## <a name="create-an-aspnet-core-web-app"></a>ASP.NET Core web uygulaması oluşturma

Yeni bir ASP.NET Core MVC projesi oluşturmak için [.NET Core komut satırı arabirimi 'ni (CLI)](/dotnet/core/tools) kullanın. [Azure Cloud Shell](https://shell.azure.com) , sizin için bu araçları sağlar. Bunlar, Windows, macOS ve Linux platformları genelinde da mevcuttur.

Yeni bir *Testappconfig* KLASÖRÜNDE ASP.NET Core MVC projesi oluşturmak için aşağıdaki komutu çalıştırın:

```dotnetcli
dotnet new mvc --no-https --output TestAppConfig
```

[!INCLUDE[Add Secret Manager support to an ASP.NET Core project](../../includes/azure-app-configuration-add-secret-manager.md)]

## <a name="connect-to-the-app-configuration-store"></a>Uygulama yapılandırma deposuna Bağlan

1. [Microsoft. Azure. AppConfiguration. AspNetCore](https://www.nuget.org/packages/Microsoft.Azure.AppConfiguration.AspNetCore) NuGet paketi başvurusu eklemek için aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet add package Microsoft.Azure.AppConfiguration.AspNetCore
    ```

1. Aşağıdaki komutu *. csproj* dosyasıyla aynı dizinde çalıştırın. Komutu `ConnectionStrings:AppConfig` , uygulama yapılandırma deponuzu için bağlantı dizesini depolayan adlı bir gizli dizi depolamak Için gizli bir yönetici kullanır. `<your_connection_string>`Yer tutucusunu uygulama yapılandırma deponuzdaki bağlantı dizesiyle değiştirin. Bağlantı dizesini Azure portal **erişim tuşları** altında bulabilirsiniz.

    ```dotnetcli
    dotnet user-secrets set ConnectionStrings:AppConfig "<your_connection_string>"
    ```

    > [!IMPORTANT]
    > Bazı kabuklar tırnak içine alınmadığı takdirde bağlantı dizesini keser. `dotnet user-secrets`Komutun çıktısının tüm bağlantı dizesini belirttiğinden emin olun. Aksi takdirde, bağlantı dizesini tırnak içine alarak komutu yeniden çalıştırın.

    Gizli dizi Yöneticisi yalnızca Web uygulamasını yerel olarak test etmek için kullanılır. Uygulama [Azure App Service](https://azure.microsoft.com/services/app-service/web)dağıtıldığında, bağlantı dizesini depolamak Için gizli yönetici yerine App Service **bağlantı dizeleri** uygulama ayarını kullanın.

    .NET Core Yapılandırma API 'sini kullanarak bu gizli dizi ile erişin. İki nokta üst üste ( `:` ) yapılandırma adında desteklenen tüm platformlarda Yapılandırma API 'si ile birlikte kullanılır. Daha fazla bilgi için bkz. [yapılandırma anahtarları ve değerleri](/aspnet/core/fundamentals/configuration#configuration-keys-and-values).

1. *Program.cs*' de .NET Core yapılandırması API 'si ad alanına bir başvuru ekleyin:

    ```csharp
    using Microsoft.Extensions.Configuration;
    ```

1. Yöntemini `CreateWebHostBuilder` çağırarak uygulama yapılandırmasını kullanmak için yöntemi güncelleştirin `AddAzureAppConfiguration` .

    > [!IMPORTANT]
    > `CreateHostBuilder``CreateWebHostBuilder`.NET Core 3. x içindeki yerini alır. Ortamınıza göre doğru söz dizimini seçin.

    #### <a name="net-core-3x"></a>[.NET Core 3. x](#tab/core3x)

    ```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
                webBuilder.ConfigureAppConfiguration(config =>
                {
                    var settings = config.Build();
                    var connection = settings.GetConnectionString("AppConfig");
                    config.AddAzureAppConfiguration(connection);
                }).UseStartup<Startup>());
    ```

    #### <a name="net-core-2x"></a>[.NET Core 2. x](#tab/core2x)

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration(config =>
            {
                var settings = config.Build();
                var connection = settings.GetConnectionString("AppConfig");
                config.AddAzureAppConfiguration(connection);
            })
            .UseStartup<Startup>();
    ```

    ---

    Önceki değişiklikten sonra, [uygulama yapılandırması için yapılandırma sağlayıcısı](/dotnet/api/Microsoft.Extensions.Configuration.AzureAppConfiguration) .NET Core Yapılandırma API 'sine kaydedilir.

## <a name="read-from-the-app-configuration-store"></a>Uygulama yapılandırma deposundan okuma

Uygulama yapılandırma deposunda depolanan değerleri okumak ve göstermek için aşağıdaki adımları izleyin. .NET Core Configuration API 'SI mağazaya erişmek için kullanılacaktır. Razor söz dizimi, anahtarların değerlerini göstermek için kullanılacaktır.

* \<app root> /Views/Home/Index.cshtml*dosyasını açın ve içeriğini şu kodla değiştirin:

```cshtml
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

Yukarıdaki kodda, uygulama yapılandırma deposunun anahtarları aşağıdaki gibi kullanılır:

* `TestApp:Settings:BackgroundColor`Anahtarın değeri, CSS `background-color` özelliğine atanır.
* `TestApp:Settings:FontColor`Anahtarın değeri, CSS `color` özelliğine atanır.
* `TestApp:Settings:FontSize`Anahtarın değeri, CSS `font-size` özelliğine atanır.
* `TestApp:Settings:Message`Anahtarın değeri bir başlık olarak görüntülenir.

## <a name="build-and-run-the-app-locally"></a>Uygulamayı yerel olarak derleyin ve çalıştırın

1. .NET Core CLI kullanarak uygulamayı derlemek için, projenizin kök dizinine gidin. Komut kabuğunda aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet build
    ```

1. Oluşturma işlemi başarıyla tamamlandıktan sonra, Web uygulamasını yerel olarak çalıştırmak için aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet run
    ```

1. Yerel makinenizde çalışıyorsanız, ' ye gitmek için bir tarayıcı kullanın `http://localhost:5000` . Bu adres, yerel olarak barındırılan Web uygulamasının varsayılan URL 'sidir. Azure Cloud Shell çalışıyorsanız, **Web önizleme** düğmesini ve ardından **Yapılandır**' ı seçin.

    ![Web önizlemesi düğmesini bulun](./media/quickstarts/cloud-shell-web-preview.png)

    Önizleme için bağlantı noktasını yapılandırmak isteyip istemediğiniz sorulduğunda *5000* girin ve **Aç ve araştır**' ı seçin. Web sayfası "Azure Uygulama yapılandırmasından verileri" okuyacaktır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE[Azure App Configuration cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta:

* Yeni bir uygulama yapılandırma deposu sağlandı.
* Uygulama yapılandırma deposunun .NET Core yapılandırma sağlayıcısı kaydedildi.
* Yapılandırma sağlayıcısıyla uygulama yapılandırma deposunun anahtarlarını okuyun.
* Razor söz dizimi kullanılarak uygulama yapılandırma deposunun anahtar değerleri görüntülendi.

Yapılandırma ayarlarını dinamik olarak yenilemek üzere ASP.NET Core uygulamanızı nasıl yapılandıracağınızı öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Dinamik yapılandırmayı etkinleştirme](./enable-dynamic-configuration-aspnet-core.md)