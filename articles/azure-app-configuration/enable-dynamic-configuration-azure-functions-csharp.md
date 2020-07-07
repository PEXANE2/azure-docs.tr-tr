---
title: Azure Işlevleri uygulamasında Azure Uygulama yapılandırması dinamik yapılandırmasını kullanma öğreticisi | Microsoft Docs
description: Bu öğreticide, Azure Işlevleri uygulamaları için yapılandırma verilerini dinamik olarak güncelleştirme hakkında bilgi edineceksiniz.
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
ms.openlocfilehash: e8f5b21189007d2b15c2ff31b778144d9a672318
ms.sourcegitcommit: 9b5c20fb5e904684dc6dd9059d62429b52cb39bc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85856471"
---
# <a name="tutorial-use-dynamic-configuration-in-an-azure-functions-app"></a>Öğretici: Azure Işlevleri uygulamasında dinamik yapılandırma kullanma

Uygulama yapılandırma .NET Standard yapılandırma sağlayıcısı, uygulama etkinliğine göre dinamik olarak yönetilen yapılandırmayı önbelleğe almayı ve yenilemeyi destekler. Bu öğreticide, kodunuzda dinamik yapılandırma güncelleştirmelerini nasıl uygulayabileceğinizi gösterir. Hızlı başlangıçlarda tanıtılan Azure Işlevleri uygulamasında oluşturulur. Devam etmeden önce, önce [Azure Uygulama yapılandırması ile bir Azure işlevleri uygulaması oluşturun](./quickstart-azure-functions-csharp.md) .

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir uygulama yapılandırma deposundaki değişikliklere yanıt olarak yapılandırmasını güncelleştirmek için Azure Işlevleri uygulamanızı ayarlayın.
> * Azure Işlevleri çağrılarınız için en son yapılandırmayı ekleme.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
- **Azure geliştirme** iş yüküyle [Visual Studio 2019](https://visualstudio.microsoft.com/vs)
- [Azure Işlevleri araçları](../azure-functions/functions-develop-vs.md#check-your-tools-version)
- Son hızlı başlangıç [Azure Uygulama yapılandırması ile bir Azure işlevleri uygulaması oluşturma](./quickstart-azure-functions-csharp.md)

## <a name="reload-data-from-app-configuration"></a>Uygulama yapılandırmasından verileri yeniden yükleme

1. *Function1.cs*'i açın. Özelliğine ek olarak `static` `Configuration` , `static` `ConfigurationRefresher` `IConfigurationRefresher` işlevler daha sonra çağrılar sırasında yapılandırma güncelleştirmelerini işaret etmek için kullanılacak tek bir örneğini tutmak üzere yeni bir özellik ekleyin.

    ```csharp
    private static IConfiguration Configuration { set; get; }
    private static IConfigurationRefresher ConfigurationRefresher { set; get; }
    ```

2. Oluşturucuyu güncelleştirin ve `ConfigureRefresh` uygulama yapılandırma deposundan yenilenecek ayarı belirtmek için yöntemini kullanın. Bir örneği `IConfigurationRefresher` yöntemi kullanılarak alınır `GetRefresher` . İsteğe bağlı olarak, yapılandırma önbelleği sona erme zamanı penceresini varsayılan 30 saniyeden 1 dakika olarak değiştiririz.

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

3. `Run`İşlev çağrısının başlangıcında yöntemi kullanarak yapılandırmayı yenilemek için yöntemi ve sinyali güncelleştirin `TryRefreshAsync` . Önbellek sona erme saati penceresine ulaşılırsa bu işlem yapılmaz. `await`Yapılandırmanın engellenmeden yenilenmesini tercih ediyorsanız işleci kaldırın.

    ```csharp
    public static async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)] HttpRequest req, ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");

        await ConfigurationRefresher.TryRefreshAsync(); 

        string keyName = "TestApp:Settings:Message";
        string message = Configuration[keyName];
            
        return message != null
            ? (ActionResult)new OkObjectResult(message)
            : new BadRequestObjectResult($"Please create a key-value with the key '{keyName}' in App Configuration.");
    }
    ```

## <a name="test-the-function-locally"></a>İşlevi yerel olarak test etme

1. **ConnectionString**adlı bir ortam değişkeni ayarlayın ve uygulama yapılandırma deponuzu için erişim anahtarı olarak ayarlayın. Windows komut istemi 'ni kullanırsanız, aşağıdaki komutu çalıştırın ve değişikliğin etkili olması için komut istemi ' ni yeniden başlatın:

    ```console
    setx ConnectionString "connection-string-of-your-app-configuration-store"
    ```

    Windows PowerShell kullanıyorsanız şu komutu çalıştırın:

    ```powershell
    $Env:ConnectionString = "connection-string-of-your-app-configuration-store"
    ```

    MacOS veya Linux kullanıyorsanız şu komutu çalıştırın:

    ```console
    export ConnectionString='connection-string-of-your-app-configuration-store'
    ```

2. İşlevinizi test etmek için F5’e basın. İstenirse, **Azure Functions Core (CLI)** araçlarını indirmek ve yüklemek Için Visual Studio 'daki isteği kabul edin. Ayrıca, araçların HTTP isteklerini işleyebilmesi için bir güvenlik duvarı özel durumu etkinleştirmeniz gerekebilir.

3. Azure İşlevleri çalışma zamanı çıktısından işlevinizin URL'sini kopyalayın.

    ![VS 'de hızlı başlangıç Işlevi hata ayıklaması](./media/quickstarts/function-visual-studio-debugging.png)

4. HTTP isteğinin URL’sini tarayıcınızın adres çubuğuna yapıştırın. Aşağıdaki görüntüde, bu işlevin döndürdüğü yerel GET isteğine tarayıcıda yapılan yanıt gösterilmektedir.

    ![Hızlı başlangıç Işlevi yerel başlatma](./media/quickstarts/dotnet-core-function-launch-local.png)

5. [Azure Portal](https://portal.azure.com) oturum açın. **Tüm kaynaklar**' ı seçin ve hızlı başlangıçta oluşturduğunuz uygulama yapılandırma deposu örneğini seçin.

6. **Yapılandırma Gezgini**' ni seçin ve aşağıdaki anahtarın değerlerini güncelleştirin:

    | Anahtar | Değer |
    |---|---|
    | TestApp: ayarlar: Ileti | Azure Uygulama yapılandırmasından alınan veriler-güncelleştirildi |

7. Tarayıcıyı birkaç kez yenileyin. Önbellek ayarı bir dakika sonra sona erdiğinde, sayfa, güncelleştirilmiş değer ile Işlev çağrısının yanıtını gösterir.

    ![Hızlı başlangıç Işlevi yerel olarak Yenile](./media/quickstarts/dotnet-core-function-refresh-local.png)

Bu öğreticide kullanılan örnek kod, [uygulama yapılandırması GitHub deposundan](https://github.com/Azure/AppConfiguration/tree/master/examples/DotNetCore/AzureFunction) indirilebilir

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, uygulama yapılandırmasından yapılandırma ayarlarını dinamik olarak yenilemek için Azure Işlevleri uygulamanızı etkinleştirdiniz. Azure yönetilen kimliğin uygulama yapılandırmasına erişimi kolaylaştırmak için nasıl kullanılacağını öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Yönetilen kimlik tümleştirmesi](./howto-integrate-azure-managed-service-identity.md)
