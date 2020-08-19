---
title: Azure Işlevleri ile Azure Uygulama yapılandırması için hızlı başlangıç | Microsoft Docs
description: Bu hızlı başlangıçta Azure Uygulama yapılandırması ve C# ile bir Azure Işlevleri uygulaması oluşturun. Uygulama yapılandırma deposu oluşturun ve bu depolama alanına bağlanın. İşlevi yerel olarak test edin.
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.custom: devx-track-csharp
ms.topic: quickstart
ms.date: 1/9/2019
ms.author: lcozzens
ms.openlocfilehash: e15c83504ee0eebb925c122d0efd4896e7b55916
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88590294"
---
# <a name="quickstart-create-an-azure-functions-app-with-azure-app-configuration"></a>Hızlı başlangıç: Azure Uygulama yapılandırması ile Azure Işlevleri uygulaması oluşturma

Bu hızlı başlangıçta, Azure App Configuration hizmetini bir Azure Işlevleri uygulamasına, tüm uygulama ayarlarınızı kodınızdan ayrı olarak depolamayı ve yönetimini merkezileştirmek için kullanacaksınız.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
- **Azure geliştirme** iş yüküyle [Visual Studio 2019](https://visualstudio.microsoft.com/vs) .
- [Azure Işlevleri araçları](../azure-functions/functions-develop-vs.md#check-your-tools-version)

## <a name="create-an-app-configuration-store"></a>Uygulama yapılandırma deposu oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. **Configuration Explorer**  >  **+ Create**  >  Aşağıdaki anahtar-değer çiftlerini eklemek için yapılandırma Gezgini +**anahtar-değer** oluştur ' u seçin:

    | Anahtar | Değer |
    |---|---|
    | TestApp: ayarlar: Ileti | Azure Uygulama yapılandırmasından veriler |

    **Etiket** ve **içerik türü** şimdilik boş bırakın.

7. **Uygula**’yı seçin.

## <a name="create-a-functions-app"></a>Işlevler uygulaması oluşturma

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

## <a name="connect-to-an-app-configuration-store"></a>Uygulama yapılandırma deposuna bağlanma

1. Projenize sağ tıklayın ve **NuGet Paketlerini Yönet**' i seçin. Araştır sekmesine **gidin** ve `Microsoft.Extensions.Configuration.AzureAppConfiguration` projenize NuGet paketini ekleyin. Bulamıyorsanız, **ön sürümü dahil** et onay kutusunu seçin.

2. *Function1.cs*'i açın ve .NET Core yapılandırması ve uygulama yapılandırma yapılandırma sağlayıcısının ad alanlarını ekleyin.

    ```csharp
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

3. `static` `Configuration` Tek bir örneği oluşturmak için adlı bir özellik ekleyin `IConfiguration` . Ardından `static` çağırarak uygulama yapılandırmasına bağlanmak için bir Oluşturucu ekleyin `AddAzureAppConfiguration()` . Bu, uygulama başlangıcında yapılandırmayı bir kez yükler. Aynı yapılandırma örneği, daha sonra yapılan çağrılar için de kullanılır.

    ```csharp
    private static IConfiguration Configuration { set; get; }

    static Function1()
    {
        var builder = new ConfigurationBuilder();
        builder.AddAzureAppConfiguration(Environment.GetEnvironmentVariable("ConnectionString"));
        Configuration = builder.Build();
    }
    ```

4. `Run`Yapılandırmadan değerleri okumak için yöntemini güncelleştirin.

    ```csharp
    public static async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)] HttpRequest req, ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");

        string keyName = "TestApp:Settings:Message";
        string message = Configuration[keyName];

        return message != null
            ? (ActionResult)new OkObjectResult(message)
            : new BadRequestObjectResult($"Please create a key-value with the key '{keyName}' in App Configuration.");
    }
    ```

## <a name="test-the-function-locally"></a>İşlevi yerel olarak test etme

1. **ConnectionString**adlı bir ortam değişkeni ayarlayın ve uygulama yapılandırma deponuzu için erişim anahtarı olarak ayarlayın. Windows komut istemi 'ni kullanırsanız, aşağıdaki komutu çalıştırın ve değişikliğin etkili olması için komut istemi ' ni yeniden başlatın:

    ```cmd
        setx ConnectionString "connection-string-of-your-app-configuration-store"
    ```

    Windows PowerShell kullanıyorsanız şu komutu çalıştırın:

    ```azurepowershell
        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"
    ```

    MacOS veya Linux kullanıyorsanız şu komutu çalıştırın:

    ```bash
        export ConnectionString='connection-string-of-your-app-configuration-store'
    ```

2. İşlevinizi test etmek için F5 tuşuna basın. İstenirse, **Azure Functions Core (CLI)** araçlarını indirmek ve yüklemek Için Visual Studio 'daki isteği kabul edin. Ayrıca, araçların HTTP isteklerini işleyebilmesi için bir güvenlik duvarı özel durumu etkinleştirmeniz gerekebilir.

3. Azure İşlevleri çalışma zamanı çıktısından işlevinizin URL'sini kopyalayın.

    ![VS 'de hızlı başlangıç Işlevi hata ayıklaması](./media/quickstarts/function-visual-studio-debugging.png)

4. HTTP isteğinin URL’sini tarayıcınızın adres çubuğuna yapıştırın. Aşağıdaki görüntüde, bu işlevin döndürdüğü yerel GET isteğine tarayıcıda yapılan yanıt gösterilmektedir.

    ![Hızlı başlangıç Işlevi yerel başlatma](./media/quickstarts/dotnet-core-function-launch-local.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir uygulama yapılandırma deposu oluşturdunuz ve bunu [uygulama yapılandırma sağlayıcısı](https://go.microsoft.com/fwlink/?linkid=2074664)aracılığıyla bir Azure işlevleri uygulamasıyla kullandınız. Yapılandırma ayarlarını dinamik olarak yenilemek üzere Azure Işlevleri uygulamanızı yapılandırma hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Dinamik yapılandırmayı etkinleştirme](./enable-dynamic-configuration-azure-functions-csharp.md)
