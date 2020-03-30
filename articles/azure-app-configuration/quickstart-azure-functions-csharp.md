---
title: Azure İşlevleriyle Azure Uygulama Yapılandırması için Hızlı Başlatma | Microsoft Dokümanlar
description: Azure İşlevleriyle Azure Uygulama Yapılandırması'nı kullanmak için hızlı bir başlangıç.
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: quickstart
ms.date: 1/9/2019
ms.author: lcozzens
ms.openlocfilehash: 2f6efdad7ab0685e58d2edd73bc36b758e8dbae2
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80245506"
---
# <a name="quickstart-create-an-azure-functions-app-with-azure-app-configuration"></a>Hızlı başlangıç: Azure Uygulama Yapılandırması ile Bir Azure İşlevler uygulaması oluşturun

Bu hızlı başlangıçta, kodunuzdan ayrı tüm uygulama ayarlarınızın depolama ve yönetimini merkezileştirmek için Azure Uygulama Yapılandırma hizmetini bir Azure İşlevleri uygulamasına dahil edersiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
- **Azure geliştirme** iş yüküyle Visual [Studio 2019.](https://visualstudio.microsoft.com/vs)
- [Azure Fonksiyonları araçları](../azure-functions/functions-develop-vs.md#check-your-tools-version)

## <a name="create-an-app-configuration-store"></a>Uygulama Yapılandırma mağazası oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Aşağıdaki anahtar değeri çiftleri eklemek için **Configuration Explorer** > **+ Create** > **Key-value'ı** seçin:

    | Anahtar | Değer |
    |---|---|
    | TestApp:Ayarlar:Mesaj | Azure Uygulama Yapılandırmasından Veriler |

    **Etiket** ve **İçerik Türünü** şimdilik boş bırakın.

7. **Uygula**’yı seçin.

## <a name="create-a-functions-app"></a>Fonksiyonlar uygulaması oluşturma

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

## <a name="connect-to-an-app-configuration-store"></a>Uygulama Yapılandırma mağazasına bağlanma

1. Projenize sağ tıklayın ve **NuGet Paketlerini Yönet'i**seçin. **Gözat** sekmesinde, NuGet paketini `Microsoft.Extensions.Configuration.AzureAppConfiguration` arayın ve projenize ekleyin. Bulamıyorsanız, **Yayın Öncesi Ekle** onay kutusunu seçin.

2. *Function1.cs*açın ve .NET Core yapılandırmasının ve Uygulama Yapılandırması yapılandırma sağlayıcısının ad boşluklarını ekleyin.

    ```csharp
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

3. Singleton `Configuration` örneğini oluşturmak için adlandırılmış bir `IConfiguration` `static` özellik ekleyin. Ardından arayarak `static` `AddAzureAppConfiguration()`App Configuration'a bağlanmak için bir oluşturucu ekleyin. Bu, uygulama nın başlatılmasında yapılandırmayı bir kez yükler. Aynı yapılandırma örneği daha sonra tüm İşlevler aramaları için kullanılacaktır.

    ```csharp
    private static IConfiguration Configuration { set; get; }

    static Function1()
    {
        var builder = new ConfigurationBuilder();
        builder.AddAzureAppConfiguration(Environment.GetEnvironmentVariable("ConnectionString"));
        Configuration = builder.Build();
    }
    ```

4. Yapılandırmadaki `Run` değerleri okumak için yöntemi güncelleştirin.

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

1. ConnectionString adında bir ortam **değişkeni**ayarlayın ve Uygulama Yapılandırma mağazanızın erişim anahtarına ayarlayın. Windows komut istemini kullanıyorsanız, değişikliğin etkili olmasını sağlamak için aşağıdaki komutu çalıştırın ve komut istemini yeniden başlatın:

    ```cmd
        setx ConnectionString "connection-string-of-your-app-configuration-store"
    ```

    Windows PowerShell kullanıyorsanız, aşağıdaki komutu çalıştırın:

    ```azurepowershell
        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"
    ```

    macOS veya Linux kullanıyorsanız, aşağıdaki komutu çalıştırın:

    ```bash
        export ConnectionString='connection-string-of-your-app-configuration-store'
    ```

2. İşlevinizi test etmek için F5 tuşuna basın. İstenirse, Visual Studio'dan **Azure İşlemesi Çekirdeği (CLI)** araçlarını indirme ve yükleme isteğini kabul edin. Araçların HTTP isteklerini işleyebileceği için bir güvenlik duvarı özel durumu etkinleştirmeniz de gerekebilir.

3. Azure İşlevleri çalışma zamanı çıktısından işlevinizin URL'sini kopyalayın.

    ![VS'de Hızlı Başlatma Fonksiyonu hata ayıklama](./media/quickstarts/function-visual-studio-debugging.png)

4. HTTP isteğinin URL’sini tarayıcınızın adres çubuğuna yapıştırın. Aşağıdaki resim, tarayıcıdaki işlevi tarafından döndürülen yerel GET isteğine verilen yanıtı gösterir.

    ![Quickstart Fonksiyonu yerel başlatma](./media/quickstarts/dotnet-core-function-launch-local.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir Uygulama Yapılandırma mağazası oluşturdunuz ve [Uygulama Yapılandırma sağlayıcısı](https://go.microsoft.com/fwlink/?linkid=2074664)aracılığıyla bir Azure İşlevler uygulamasıyla kullandınız. Yapılandırma ayarlarını dinamik olarak yenilecek şekilde Azure İşlevler uygulamanızı nasıl yapılandıracağımı öğrenmek için bir sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Dinamik yapılandırmayı etkinleştirme](./enable-dynamic-configuration-azure-functions-csharp.md)
