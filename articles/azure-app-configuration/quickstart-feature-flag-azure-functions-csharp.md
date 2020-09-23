---
title: Azure Işlevlerine özellik bayrakları eklemeye yönelik hızlı başlangıç | Microsoft Docs
description: Bu hızlı başlangıçta Azure App Configuration 'ın özellik bayrakları ile Azure Işlevleri 'ni kullanın ve işlevi yerel olarak test edin.
services: azure-app-configuration
author: AlexandraKemperMS
ms.service: azure-app-configuration
ms.custom: devx-track-csharp
ms.topic: quickstart
ms.date: 8/26/2020
ms.author: alkemper
ms.openlocfilehash: 5542dca6d9b1ab18cf1b9b93e20fa64da0eb1dd4
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "91001058"
---
# <a name="quickstart-add-feature-flags-to-an-azure-functions-app"></a>Hızlı başlangıç: Azure Işlevleri uygulamasına özellik bayrakları ekleme

Bu hızlı başlangıçta, Azure Uygulama yapılandırması 'nı kullanarak bir Azure Işlevleri uygulamasında bir özellik yönetimi uygulaması oluşturacaksınız. Tüm özellik Bayraklarınızı merkezi olarak depolamak ve durumlarını denetlemek için uygulama yapılandırma hizmeti kullanacaksınız. 

.NET Özellik Yönetimi kitaplıkları çerçeveyi Özellik bayrağı desteğiyle genişletir. Bu kitaplıklar, .NET yapılandırma sisteminin üzerine kurulmuştur. Bunlar, .NET yapılandırma sağlayıcısı aracılığıyla uygulama yapılandırmasıyla tümleştirilir.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
- **Azure geliştirme** iş yüküyle [Visual Studio 2019](https://visualstudio.microsoft.com/vs) .
- [Azure Işlevleri araçları](../azure-functions/functions-develop-vs.md#check-your-tools-version)

## <a name="create-an-app-configuration-store"></a>Uygulama yapılandırma deposu oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Özellik **Yöneticisi**  >  **+ Ekle** ' yi seçerek Özellik bayrağı ekleyin `Beta` .

    > [!div class="mx-imgBorder"]
    > ![Beta adlı özellik bayrağını etkinleştir](media/add-beta-feature-flag.png)

    `label`Şimdilik bırakın ve `Description` tanımsız olur.

7. Yeni özellik bayrağını kaydetmek için **Uygula** ' yı seçin.

## <a name="create-a-functions-app"></a>Işlevler uygulaması oluşturma

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

## <a name="connect-to-an-app-configuration-store"></a>Uygulama yapılandırma deposuna bağlanma

1. Projenize sağ tıklayın ve **NuGet Paketlerini Yönet**' i seçin. **Araştır** sekmesinde, aşağıdaki NuGet paketlerini arayıp projenize ekleyin. `Microsoft.Extensions.DependencyInjection`En son kararlı yapıda olduğunuzdan emin olun. 

    ```
    Microsoft.Extensions.DependencyInjection
    Microsoft.Extensions.Configuration
    Microsoft.FeatureManagement
    ```


1. *Function1.cs*'i açın ve bu paketlerin ad alanlarını ekleyin.

    ```csharp
    using Microsoft.Extensions.Configuration;
    using Microsoft.FeatureManagement;
    using Microsoft.Extensions.DependencyInjection;
    ```

1. `Function1`Azure uygulama yapılandırma sağlayıcısı 'nı önyüklemek için aşağıdaki statik oluşturucuyu ekleyin. Sonra, tek bir örneği oluşturmak için `static` adlı bir alan `ServiceProvider` olan `ServiceProvider` ve ' `Function1` `FeatureManager` ın tek bir örneğini oluşturmak için adlı bir özellik `IFeatureManager` olan iki üye ekleyin. Ardından ' de çağırarak uygulama yapılandırmasına bağlanın `Function1` `AddAzureAppConfiguration()` . Bu işlem, uygulama başlangıcında yapılandırmayı yükleyecek. Aynı yapılandırma örneği, daha sonra yapılan çağrılar için de kullanılır. 

    ```csharp
        // Implements IDisposable, cached for life time of function
        private static ServiceProvider ServiceProvider; 

        static Function1()
        {
            IConfigurationRoot configuration = new ConfigurationBuilder()
                .AddAzureAppConfiguration(options =>
                {
                    options.Connect(Environment.GetEnvironmentVariable("ConnectionString"))
                           .UseFeatureFlags();
                }).Build();

            var services = new ServiceCollection();                                                                             
            services.AddSingleton<IConfiguration>(configuration).AddFeatureManagement();

            ServiceProvider = services.BuildServiceProvider(); 
        }

        private static IFeatureManager FeatureManager => ServiceProvider.GetRequiredService<IFeatureManager>();
    ```

1. `Run`Özellik bayrağının durumuna bağlı olarak, görüntülenecek iletinin değerini değiştirmek için yöntemini güncelleştirin.

    ```csharp
        [FunctionName("Function1")]
        public static async Task<IActionResult> Run(
                [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)] HttpRequest req,
                ILogger log)
            {
                string message = await FeatureManager.IsEnabledAsync("Beta")
                     ? "The Feature Flag 'Beta' is turned ON"
                     : "The Feature Flag 'Beta' is turned OFF";
                
                return (ActionResult)new OkObjectResult(message); 
            }
    ```

## <a name="test-the-function-locally"></a>İşlevi yerel olarak test etme

1. Değer, **erişim anahtarları**altında uygulama yapılandırma deponuzda daha önce aldığınız erişim anahtarı olan **ConnectionString**adlı bir ortam değişkeni ayarlayın. Windows komut istemi 'ni kullanırsanız, aşağıdaki komutu çalıştırın ve değişikliğin etkili olması için komut istemi ' ni yeniden başlatın:

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

1. İşlevinizi test etmek için F5 tuşuna basın. İstenirse, **Azure Functions Core (CLI)** araçlarını indirmek ve yüklemek Için Visual Studio 'daki isteği kabul edin. Ayrıca, araçların HTTP isteklerini işleyebilmesi için bir güvenlik duvarı özel durumu etkinleştirmeniz gerekebilir.

1. Azure İşlevleri çalışma zamanı çıktısından işlevinizin URL'sini kopyalayın.

    ![VS 'de hızlı başlangıç Işlevi hata ayıklaması](./media/quickstarts/function-visual-studio-debugging.png)

1. HTTP isteğinin URL’sini tarayıcınızın adres çubuğuna yapıştırın. Aşağıdaki görüntüde Özellik bayrağının `Beta` devre dışı bırakıldığını belirten yanıt gösterilmektedir. 

    ![Hızlı başlangıç Işlevi Özellik bayrağı devre dışı](./media/quickstarts/functions-launch-ff-disabled.png)

1. [Azure Portal](https://portal.azure.com) oturum açın. **Tüm kaynaklar**' ı seçin ve oluşturduğunuz uygulama yapılandırma deposu örneğini seçin.

1. **Özellik Yöneticisi**' ni seçin ve **Beta** anahtarının durumunu **Açık**olarak değiştirin.

1. Komut isteminizde dönün ve ' e basarak çalışan işlemi iptal edin `Ctrl-C` .  F5 tuşuna basarak uygulamanızı yeniden başlatın. 

1. İşlevinizin URL 'sini Azure Işlevleri çalışma zamanı çıktısından, adım 3 ' teki ile aynı işlemi kullanarak kopyalayın. HTTP isteğinin URL’sini tarayıcınızın adres çubuğuna yapıştırın. Aşağıdaki görüntüde gösterildiği gibi, tarayıcı yanıtı Özellik bayrağının açık olduğunu belirtecek şekilde değiştirilmelidir `Beta` .
 
    ![Hızlı başlangıç Işlevi özelliği bayrağı etkin](./media/quickstarts/functions-launch-ff-enabled.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir özellik bayrağı oluşturdunuz ve bunu [uygulama yapılandırma sağlayıcısı](https://go.microsoft.com/fwlink/?linkid=2074664)aracılığıyla bir Azure işlevleri uygulamasıyla kullandınız.

- [Özellik yönetimi](./concept-feature-management.md)hakkında daha fazla bilgi edinin.
- [Özellik bayraklarını yönetin](./manage-feature-flags.md).
- [Azure Işlevleri uygulamasında dinamik yapılandırma kullanma](./enable-dynamic-configuration-azure-functions-csharp.md)
