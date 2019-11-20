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
ms.openlocfilehash: 6329cf0e74bbcf57164afeab5b04e2af4ee43943
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74186198"
---
# <a name="quickstart-create-an-azure-functions-app-with-azure-app-configuration"></a>Hızlı başlangıç: Azure Uygulama yapılandırması ile Azure Işlevleri uygulaması oluşturma

Bu hızlı başlangıçta, Azure App Configuration hizmetini bir Azure Işlevleri uygulamasına, tüm uygulama ayarlarınızı kodınızdan ayrı olarak depolamayı ve yönetimini merkezileştirmek için kullanacaksınız.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
- **Azure geliştirme** iş yüküyle [Visual Studio 2019](https://visualstudio.microsoft.com/vs) .
- [Azure Işlevleri araçları](../azure-functions/functions-develop-vs.md#check-your-tools-version)

## <a name="create-an-app-configuration-store"></a>Uygulama yapılandırma deposu oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Aşağıdaki anahtar-değer çiftlerini eklemek için **yapılandırma gezgini** >  **+ Oluştur** ' u seçin:

    | Anahtar | Değer |
    |---|---|
    | TestApp: ayarlar: Ileti | Azure Uygulama yapılandırmasından veriler |

    **Etiket** ve **içerik türü** şimdilik boş bırakın.

## <a name="create-a-functions-app"></a>Işlevler uygulaması oluşturma

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

## <a name="connect-to-an-app-configuration-store"></a>Uygulama yapılandırma deposuna bağlanma

1. Projenize sağ tıklayın ve **NuGet Paketlerini Yönet**' i seçin. **Araştır** sekmesinde, aşağıdaki NuGet paketlerini arayıp projenize ekleyin. Bunları bulamıyorsanız, **ön sürümü dahil et** onay kutusunu seçin.

    ```
    Microsoft.Extensions.Configuration.AzureAppConfiguration 2.1.0-preview-010380001-1099 or later
    ```

2. *Function1.cs*'i açın ve .NET Core yapılandırması ve uygulama yapılandırma yapılandırma sağlayıcısının ad alanlarını ekleyin.

    ```csharp
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```
3. `IConfiguration`tek bir örneğini oluşturmak için `Configuration` `static` özelliği ekleyin. Sonra, `AddAzureAppConfiguration()`çağırarak uygulama yapılandırmasına bağlanmak için bir `static` Oluşturucu ekleyin. Bu, uygulama başlangıcında yapılandırmayı bir kez yükler. Aynı yapılandırma örneği, daha sonra yapılan çağrılar için de kullanılır.

    ```csharp
    private static IConfiguration Configuration { set; get; }

    static Function1()
    {
        var builder = new ConfigurationBuilder();
        builder.AddAzureAppConfiguration(Environment.GetEnvironmentVariable("ConnectionString"));
        Configuration = builder.Build();
    }
    ```
4. Yapılandırma değerlerini okumak için `Run` yöntemini güncelleştirin.

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

Bu hızlı başlangıçta, yeni bir uygulama yapılandırma deposu oluşturdunuz ve bunu [uygulama yapılandırma sağlayıcısı](https://go.microsoft.com/fwlink/?linkid=2074664)aracılığıyla bir Azure işlevleri uygulamasıyla kullandınız. Yapılandırma ayarlarını dinamik olarak yenilemek üzere Azure Işlevleri uygulamanızı yapılandırma hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Dinamik yapılandırmayı etkinleştir](./enable-dynamic-configuration-azure-functions-csharp.md)
