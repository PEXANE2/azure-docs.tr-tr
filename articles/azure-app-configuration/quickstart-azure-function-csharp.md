---
title: Azure Işlevleri ile Azure Uygulama yapılandırması için hızlı başlangıç | Microsoft Docs
description: Azure Işlevleri ile Azure Uygulama yapılandırması 'nı kullanmaya yönelik hızlı başlangıç.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: Azure Functions
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: 5eb9d0631a4d5f4221b5184198290a5109655408
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68326572"
---
# <a name="quickstart-create-an-azure-function-with-azure-app-configuration"></a>Hızlı Başlangıç: Azure Uygulama yapılandırması ile bir Azure işlevi oluşturma

Bu hızlı başlangıçta, Azure App Configuration hizmetini bir Azure işlevine, tüm uygulama ayarlarınızı kodınızdan ayrı olarak depolamayı ve yönetimini merkezileştirmek için kullanacaksınız.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
- **Azure geliştirme** iş yüküyle [Visual Studio 2019](https://visualstudio.microsoft.com/vs) .
- [Azure Işlevleri araçları](../azure-functions/functions-develop-vs.md#check-your-tools-version)

## <a name="create-an-app-configuration-store"></a>Uygulama yapılandırma deposu oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Aşağıdaki anahtar-değer çiftlerini eklemek için **yapılandırma Gezgini** >  **+ Oluştur** ' u seçin:

    | Anahtar | Değer |
    |---|---|
    | TestApp: ayarlar: Ileti | Azure Uygulama yapılandırmasından veriler |

    **Etiket** ve **içerik türü** şimdilik boş bırakın.

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

## <a name="connect-to-an-app-configuration-store"></a>Uygulama yapılandırma deposuna bağlanma

1. Projenize sağ tıklayın ve **NuGet Paketlerini Yönet**' i seçin. **Araştır** sekmesinde, aşağıdaki NuGet paketlerini arayıp projenize ekleyin. Bunları bulamıyorsanız, **ön sürümü dahil et** onay kutusunu seçin.

    ```
    Microsoft.Extensions.Configuration.AzureAppConfiguration 2.0.0-preview-009200001-1437 or later
    ```

2. *Function1.cs*'i açın ve .NET Core uygulama yapılandırma sağlayıcısına bir başvuru ekleyin.

    ```csharp
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

3. `Run` Yöntemini çağırarak`builder.AddAzureAppConfiguration()`uygulama yapılandırmasını kullanacak şekilde güncelleştirin.

    ```csharp
    public static async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)] HttpRequest req, ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");

        var builder = new ConfigurationBuilder();
        builder.AddAzureAppConfiguration(Environment.GetEnvironmentVariable("ConnectionString"));
        var config = builder.Build();
        string message = config["TestApp:Settings:Message"];
        message = message ?? req.Query["message"];

        string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
        dynamic data = JsonConvert.DeserializeObject(requestBody);
        message = message ?? data?.message;

        return message != null
            ? (ActionResult) new OkObjectResult(message)
            : new BadRequestObjectResult("Please pass a message from a configuration store, on the query string or in the request body");
    }
    ```

## <a name="test-the-function-locally"></a>İşlevi yerel olarak test etme

1. **ConnectionString**adlı bir ortam değişkeni ayarlayın ve uygulama yapılandırma deponuzu için erişim anahtarı olarak ayarlayın. Windows komut istemi 'ni kullanırsanız, aşağıdaki komutu çalıştırın ve değişikliğin etkili olması için komut istemi ' ni yeniden başlatın:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Windows PowerShell kullanıyorsanız şu komutu çalıştırın:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

    MacOS veya Linux kullanıyorsanız şu komutu çalıştırın:

        export ConnectionString='connection-string-of-your-app-configuration-store'

2. İşlevinizi test etmek için F5’e basın. İstenirse, **Azure Functions Core (CLI)** araçlarını indirmek ve yüklemek Için Visual Studio 'daki isteği kabul edin. Ayrıca, araçların HTTP isteklerini işleyebilmesi için bir güvenlik duvarı özel durumu etkinleştirmeniz gerekebilir.

3. Azure İşlevleri çalışma zamanı çıktısından işlevinizin URL'sini kopyalayın.

    ![VS 'de hızlı başlangıç Işlevi hata ayıklaması](./media/quickstarts/function-visual-studio-debugging.png)

4. HTTP isteğinin URL’sini tarayıcınızın adres çubuğuna yapıştırın. Aşağıdaki görüntüde, bu işlevin döndürdüğü yerel GET isteğine tarayıcıda yapılan yanıt gösterilmektedir.

    ![Hızlı başlangıç Işlevi yerel başlatma](./media/quickstarts/dotnet-core-function-launch-local.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta yeni bir uygulama yapılandırma deposu oluşturdunuz ve bunu bir Azure işlevi ile kullandınız. Uygulama yapılandırmasını kullanma hakkında daha fazla bilgi edinmek için, kimlik doğrulamasını gösteren bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Yönetilen kimlik tümleştirmesi](./howto-integrate-azure-managed-service-identity.md)
