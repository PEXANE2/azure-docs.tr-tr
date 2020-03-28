---
title: Azure İşlevler uygulamasında Azure Uygulama Yapılandırması dinamik yapılandırması kullanma için öğretici | Microsoft Dokümanlar
description: Bu eğitimde, Azure İşlevleri uygulamalarının yapılandırma verilerini dinamik olarak nasıl güncelleştirdiğinizi öğreneceksiniz
services: azure-app-configuration
documentationcenter: ''
author: zhenlan
manager: qingye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 11/17/2019
ms.author: zhenlwa
ms.custom: azure-functions
ms.tgt_pltfrm: Azure Functions
ms.openlocfilehash: ba70d5f186c1424b2019716ab7a87aeae85f8913
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74185453"
---
# <a name="tutorial-use-dynamic-configuration-in-an-azure-functions-app"></a>Öğretici: Azure İşlevler uygulamasında dinamik yapılandırma kullanın

App Configuration .NET Standard yapılandırma sağlayıcısı, uygulama etkinliği tarafından dinamik olarak yönlendirilen önbelleğe alma ve yenileme yapılandırmasını destekler. Bu öğretici, kodunuzda dinamik yapılandırma güncelleştirmelerini nasıl uygulayabileceğinizi gösterir. Hızlı başlangıçlarda tanıtılan Azure İşlevler uygulamasına dayanıyor. Devam etmeden önce, [önce Azure Uygulama Yapılandırması ile bir Azure işlevleri oluştur uygulamasını](./quickstart-azure-functions-csharp.md) bitirin.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure İşlevler uygulamanızı, bir Uygulama Yapılandırma mağazasındaki değişikliklere yanıt olarak yapılandırmasını güncelleştirecek şekilde ayarlayın.
> * Azure İşlevleri aramalarınıza en son yapılandırmayı enjekte edin.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
- **Azure geliştirme** iş yüküyle Visual [Studio 2019](https://visualstudio.microsoft.com/vs)
- [Azure Fonksiyonları araçları](../azure-functions/functions-develop-vs.md#check-your-tools-version)
- Azure [Uygulama Yapılandırması ile](./quickstart-azure-functions-csharp.md) hızlı bir şekilde Azure işlevleri uygulaması oluşturun

## <a name="reload-data-from-app-configuration"></a>Uygulama Yapılandırmasından verileri yeniden yükleme

1. Açık *Function1.cs*. `static` Özelliğine `Configuration`ek olarak, daha `static` `ConfigurationRefresher` sonra Fonksiyonlar aramaları sırasında `IConfigurationRefresher` yapılandırma güncelleştirmelerini sinyallemek için kullanılacak olan singleton örneğini tutmak için yeni bir özellik ekleyin.

    ```csharp
    private static IConfiguration Configuration { set; get; }
    private static IConfigurationRefresher ConfigurationRefresher { set; get; }
    ```

2. Oluşturucuyu güncelleştirin `ConfigureRefresh` ve Uygulama Yapılandırma deposundan yenilenecek ayarı belirtmek için yöntemi kullanın. Bir örneği `IConfigurationRefresher` yöntem kullanılarak `GetRefresher` alınır. İsteğe bağlı olarak, yapılandırma önbelleği son kullanma süresini varsayılan 30 saniyeden 1 dakikaya değiştiririz.

    ```csharp
    static Function1()
    {
        var builder = new ConfigurationBuilder();
        builder.AddAzureAppConfiguration(options =>
        {
            options.Connect(Environment.GetEnvironmentVariable("ConnectionString"))
                   .ConfigureRefresh(refreshOptions =>
                        refreshOptions.Register("TestApp:Settings:Message")
                                      .SetCacheExpiration(TimeSpan.FromSeconds(60))
            );
            ConfigurationRefresher = options.GetRefresher();
        });
        Configuration = builder.Build();
    }
    ```

3. İşlevler çağrısının `Run` başındaki `Refresh` yöntemi kullanarak yapılandırmayı yenilemek için yöntemi ve sinyali güncelleştirin. Önbellek son kullanma süresi penceresine ulaşılmazsa bu işlem yapılmaz. Yapılandırmanın `await` engellenmeden yenilenmesini tercih ediyorsanız operatörü kaldırın.

    ```csharp
    public static async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)] HttpRequest req, ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");

        await ConfigurationRefresher.Refresh();

        string keyName = "TestApp:Settings:Message";
        string message = Configuration[keyName];
            
        return message != null
            ? (ActionResult)new OkObjectResult(message)
            : new BadRequestObjectResult($"Please create a key-value with the key '{keyName}' in App Configuration.");
    }
    ```

## <a name="test-the-function-locally"></a>İşlevi yerel olarak test etme

1. ConnectionString adında bir ortam **değişkeni**ayarlayın ve uygulama yapılandırma mağazanızın erişim anahtarına ayarlayın. Windows komut istemini kullanıyorsanız, değişikliğin etkili olmasını sağlamak için aşağıdaki komutu çalıştırın ve komut istemini yeniden başlatın:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Windows PowerShell kullanıyorsanız, aşağıdaki komutu çalıştırın:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

    macOS veya Linux kullanıyorsanız, aşağıdaki komutu çalıştırın:

        export ConnectionString='connection-string-of-your-app-configuration-store'

2. İşlevinizi test etmek için F5’e basın. İstenirse, Visual Studio'dan **Azure İşlemesi Çekirdeği (CLI)** araçlarını indirme ve yükleme isteğini kabul edin. Araçların HTTP isteklerini işleyebileceği için bir güvenlik duvarı özel durumu etkinleştirmeniz de gerekebilir.

3. Azure İşlevleri çalışma zamanı çıktısından işlevinizin URL'sini kopyalayın.

    ![VS'de Hızlı Başlatma Fonksiyonu hata ayıklama](./media/quickstarts/function-visual-studio-debugging.png)

4. HTTP isteğinin URL’sini tarayıcınızın adres çubuğuna yapıştırın. Aşağıdaki resim, tarayıcıdaki işlevi tarafından döndürülen yerel GET isteğine verilen yanıtı gösterir.

    ![Quickstart Fonksiyonu yerel başlatma](./media/quickstarts/dotnet-core-function-launch-local.png)

5. [Azure portalında](https://portal.azure.com)oturum açın. **Tüm kaynakları**seçin ve hızlı başlangıçta oluşturduğunuz Uygulama Yapılandırma mağazası örneğini seçin.

6. **Configuration Explorer'ı**seçin ve aşağıdaki anahtarın değerlerini güncelleştirin:

    | Anahtar | Değer |
    |---|---|
    | TestApp:Ayarlar:Mesaj | Azure Uygulama Yapılandırmasından Veriler - Güncellendi |

7. Tarayıcıyı birkaç kez yenileyin. Önbelleğe alınmış ayar bir dakika sonra sona erdiğinde, sayfa güncelleştirilmiş değere sahip İşlevler çağrısının yanıtını gösterir.

    ![Quickstart Fonksiyonu yerel yenileme](./media/quickstarts/dotnet-core-function-refresh-local.png)

Bu öğreticide kullanılan örnek kod [Uygulama Yapılandırma GitHub repo](https://github.com/Azure/AppConfiguration/tree/master/examples/DotNetCore/AzureFunction) indirilebilir

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure İşlevler uygulamanızın Uygulama Yapılandırması'ndan yapılandırma ayarlarını dinamik olarak yenilemesini sağladınız. Uygulama Yapılandırması'na erişimi kolaylaştırmak için Azure yönetilen bir kimliği nasıl kullanacağınızı öğrenmek için bir sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Yönetilen kimlik tümleştirmesi](./howto-integrate-azure-managed-service-identity.md)
