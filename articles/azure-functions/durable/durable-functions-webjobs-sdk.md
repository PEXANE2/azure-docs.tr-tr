---
title: Web İşler olarak Dayanıklı İşlevler nasıl çalıştırılır - Azure
description: WebJobs SDK'yı kullanarak Webİşler'de çalışacak Dayanıklı İşlevleri nasıl kodlayıp yapılandırıştırmayı öğrenin.
ms.topic: conceptual
ms.date: 04/25/2018
ms.author: azfuncdf
ms.openlocfilehash: d8dd0c86fbc520d0bd3ef6034891bd9871774b4a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74232733"
---
# <a name="how-to-run-durable-functions-as-webjobs"></a>Webİşler olarak Dayanıklı İşlevler nasıl çalıştırılır?

Varsayılan olarak, Dayanıklı Işlevler, düzenlemebarındırmak için Azure İşlevleri çalışma zamanını kullanır. Ancak, olayları dinleyen kod üzerinde daha fazla denetime ihtiyacınız olan belirli senaryolar olabilir. Bu makalede, WebJobs SDK kullanarak orkestrasyon nasıl uygulanacağını gösterir. İşlevler ve Web İşleri arasında daha ayrıntılı bir karşılaştırma görmek için [bkz.](../functions-compare-logic-apps-ms-flow-webjobs.md#compare-functions-and-webjobs)

[Azure İşlevler](../functions-overview.md) ve [Dayanıklı İşlevler](durable-functions-overview.md) uzantısı [WebJobs SDK'da](../../app-service/webjobs-sdk-how-to.md)oluşturulur. WebJobs SDK'daki iş ana bilgisayar, Azure İşlevleri'nde çalışma zamanıdır. Azure İşlevleri'nde davranışı mümkün olmayan şekillerde denetlemeniz gerekiyorsa, WebJobs SDK'yı kullanarak Dayanıklı İşlevler geliştirebilir ve çalıştırabilirsiniz.

WebJobs SDK sürümünde 3.x, ana bilgisayar `IHost`bir uygulamadır , ve sürüm `JobHost` 2.x nesnekullanın.

Zincirleme Dayanıklı Fonksiyonlar örneği Bir WebJobs SDK 2.x sürümünde mevcuttur: karşıdan yükleme veya Kalıcı [Fonksiyonlar deposu](https://github.com/azure/azure-functions-durable-extension/)klonlamak ve *örnekler\\\\webjobssdk zincirleme* klasörüne gidin.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, Web İşler SDK, Azure İşlevleri için C# sınıfı kitaplık geliştirme ve Dayanıklı Işlevler temellerini bildiğiniz varsayar. Bu konulara giriş yapmanız gerekiyorsa, aşağıdaki kaynaklara bakın:

* [WebJobs SDK ile başlayın](../../app-service/webjobs-sdk-get-started.md)
* [Visual Studio kullanarak ilk işlevinizi oluşturma](../functions-create-your-first-function-visual-studio.md)
* [Dayanıklı İşlevler](durable-functions-sequence.md)

Bu makaledeki adımları tamamlamak için:

* **Azure geliştirme** iş yüküyle Visual [Studio 2019'u yükleyin.](https://docs.microsoft.com/visualstudio/install/)

  Visual Studio'nuz zaten varsa, ancak bu iş yükünün yoksa, **Araçlar** > **Araçları ve Özellikleri Al'ı**seçerek iş yükünü ekleyin.

  (Bunun yerine [Visual Studio Code'u](https://code.visualstudio.com/) kullanabilirsiniz, ancak bazı talimatlar Visual Studio'ya özgüdir.)

* [Azure Depolama Emülatör](../../storage/common/storage-use-emulator.md) sürümü 5.2 veya sonraki sürüm'u yükleyin ve çalıştırın. Alternatif olarak *App.config* dosyasını Azure Depolama bağlantı dizesiyle güncelleştirmek de mümkündür.

## <a name="webjobs-sdk-versions"></a>WebJobs SDK sürümleri

Bu makalede, bir WebJobs SDK 2.x projesinin nasıl geliştirilen (Azure İşlerİ sürüm 1.x'e eşdeğer) nasıl geliştirilen açıklanmaktadır. Sürüm 3.x hakkında daha fazla bilgi için bu makalenin ilerleyen saatlerinde [WebJobs SDK 3.x'e](#webjobs-sdk-3x) bakın.

## <a name="create-a-console-app"></a>Konsol uygulaması oluşturma

Dayanıklı İşlevler'i Web İşler olarak çalıştırmak için önce bir konsol uygulaması oluşturmanız gerekir. Bir WebJobs SDK projesi, uygun NuGet paketlerinin yüklü olduğu bir konsol uygulaması projesidir.

Visual Studio **New Project** iletişim kutusunda **Windows Classic Desktop** > **Console App (.NET Framework) seçeneğini belirleyin.** Proje dosyasında, `TargetFrameworkVersion` olmalıdır. `v4.6.1`

Visual Studio ayrıca **Cloud** > **Azure WebJob (.NET Framework)** seçerek kullanabileceğiniz bir WebJob proje şablonuna sahiptir. Bu şablon, bazılarına gerek olmayan birçok paket yükler.

## <a name="install-nuget-packages"></a>NuGet paketlerini yükleme

WebJobs SDK, temel bağlamalar, günlük çerçevesi ve Dayanıklı Görev uzantısı için NuGet paketlerine ihtiyacınız vardır. Bu paketler için **Paket Yöneticisi Konsol** komutları, bu makalenin yazıldığı tarih itibariyle en son kararlı sürüm numaraları ile şunlardır:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions -version 2.2.0
Install-Package Microsoft.Extensions.Logging -version 2.0.1
Install-Package Microsoft.Azure.WebJobs.Extensions.DurableTask -version 1.8.3
```

Ayrıca günlük sağlayıcıları gerekir. Aşağıdaki komutlar Azure Application Insights sağlayıcısını ve `ConfigurationManager`. Uygulama `ConfigurationManager` Öngörüleri enstrümantasyon anahtarını uygulama ayarlarından almanızı sağlar.

```powershell
Install-Package Microsoft.Azure.WebJobs.Logging.ApplicationInsights -version 2.2.0
Install-Package System.Configuration.ConfigurationManager -version 4.4.1
```

Aşağıdaki komut konsol sağlayıcısını yükler:

```powershell
Install-Package Microsoft.Extensions.Logging.Console -version 2.0.1
```

## <a name="jobhost-code"></a>JobHost kodu

Konsol uygulamasını oluşturduktan ve ihtiyacınız olan NuGet paketlerini yükledikten sonra Dayanıklı İşlevler'i kullanmaya hazırsınız. Bunu JobHost kodunu kullanarak yaparsınız.

Dayanıklı İşlevler uzantısını `UseDurableTask` kullanmak `JobHostConfiguration` için `Main` yönteminizin nesnesini arayın:

```cs
var config = new JobHostConfiguration();
config.UseDurableTask(new DurableTaskExtension
{
    HubName = "MyTaskHub",
};
```

`DurableTaskExtension` Nesnede ayarlayabildiğiniz özelliklerin listesi için [host.json'a](../functions-host-json.md#durabletask)bakın.

Yöntem `Main` aynı zamanda günlük sağlayıcıları kurmak için yerdir. Aşağıdaki örnekkonsolu ve Application Insights sağlayıcılarını yapılandırır.

```cs
static void Main(string[] args)
{
    using (var loggerFactory = new LoggerFactory())
    {
        var config = new JobHostConfiguration();

        config.DashboardConnectionString = "";

        var instrumentationKey =
            ConfigurationManager.AppSettings["APPINSIGHTS_INSTRUMENTATIONKEY"];

        config.LoggerFactory = loggerFactory
            .AddApplicationInsights(instrumentationKey, null)
            .AddConsole();

        config.UseTimers();
        config.UseDurableTask(new DurableTaskExtension
        {
            HubName = "MyTaskHub",
        });
        var host = new JobHost(config);
        host.RunAndBlock();
    }
}
```

## <a name="functions"></a>İşlevler

Webİşler bağlamında Dayanıklı İşlevler, Azure İşlevleri bağlamında Dayanıklı İşlevler'den biraz farklıdır. Kodunuzu yazarken farklılıkların farkında olmak önemlidir.

WebJobs SDK aşağıdaki Azure İşleri özelliklerini desteklemez:

* [FunctionName özniteliği](#functionname-attribute)
* [HTTP tetikleyicisi](#http-trigger)
* [Dayanıklı Fonksiyonlar HTTP yönetim API](#http-management-api)

### <a name="functionname-attribute"></a>FunctionName özniteliği

Bir WebJobs SDK projesinde, bir işlevin yöntem adı işlev adıdır. Öznitelik `FunctionName` yalnızca Azure İşlevlerinde kullanılır.

### <a name="http-trigger"></a>HTTP tetikleyicisi

WebJobs SDK'nın BIR HTTP tetikleyicisi yoktur. Örnek projenin düzenleme istemcisi bir zamanlayıcı tetikleyicisi kullanır:

```cs
public static async Task CronJob(
    [TimerTrigger("0 */2 * * * *")] TimerInfo timer,
    [OrchestrationClient] DurableOrchestrationClient client,
    ILogger logger)
{
  ...
}
```

### <a name="http-management-api"></a>HTTP yönetimi API

Hiçbir HTTP tetikleyici olduğundan, WebJobs SDK hiçbir [HTTP yönetimi API](durable-functions-http-api.md)vardır.

Bir WebJobs SDK projesinde, HTTP isteklerigöndermek yerine düzenleme istemcisi nesnesi üzerindeki yöntemleri arayabilirsiniz. Aşağıdaki yöntemler, HTTP yönetim API'si ile yapabileceğiniz üç görevle karşılık gelir:

* `GetStatusAsync`
* `RaiseEventAsync`
* `TerminateAsync`

Örnek projedeki orkestrasyon istemcisi işlevi orkestratör işlevini başlatır `GetStatusAsync` ve sonra her 2 saniyede bir çağıran bir döngüye girer:

```cs
string instanceId = await client.StartNewAsync(nameof(HelloSequence), input: null);
logger.LogInformation($"Started new instance with ID = {instanceId}.");

DurableOrchestrationStatus status;
while (true)
{
    status = await client.GetStatusAsync(instanceId);
    logger.LogInformation($"Status: {status.RuntimeStatus}, Last update: {status.LastUpdatedTime}.");

    if (status.RuntimeStatus == OrchestrationRuntimeStatus.Completed ||
        status.RuntimeStatus == OrchestrationRuntimeStatus.Failed ||
        status.RuntimeStatus == OrchestrationRuntimeStatus.Terminated)
    {
        break;
    }

    await Task.Delay(TimeSpan.FromSeconds(2));
}
```

## <a name="run-the-sample"></a>Örneği çalıştırma

Webİş olarak çalışacak Şekilde Ayarlanmış Dayanıklı İşlevler'e sahipsiniz ve artık bunun Dayanıklı İşlevler'i bağımsız Azure İşlevleri olarak çalıştırmaktan nasıl farklı olacağını anlayabilirsiniz. Bu noktada, bir örnekte çalıştığını görmek yararlı olabilir.

Bu [bölümde, örnek projenin](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/webjobssdk/chaining)nasıl çalıştırılalanabildiğini anlatan bir genel bakış bulunmaktadır. Bir Webİşler SDK projesinin yerel olarak nasıl çalıştırılabildiğini ve azure webjob'a nasıl dağıtılacak yapılacağını açıklayan ayrıntılı yönergeler [için](../../app-service/webjobs-sdk-get-started.md#deploy-as-a-webjob)bkz.

### <a name="run-locally"></a>Yerel olarak çalıştırma

1. Depolama emülatörü çalışıyor emin olun [(Önkoşullar'a](#prerequisites)bakın).

1. Projeyi yerel olarak çalıştırdığınızda Uygulama İstatistikleri'ndeki günlükleri görmek istiyorsanız:

    a. Bir Application Insights kaynağı oluşturun ve bunun için **Genel** uygulama türünü kullanın.

    b. Enstrümantasyon anahtarını *App.config* dosyasına kaydedin.

1. Projeyi çalıştırın.

### <a name="run-in-azure"></a>Azure'da Çalıştır

1. Bir web uygulaması ve depolama hesabı oluşturun.

1. Web uygulamasında, depolama bağlantı dizesini ' `AzureWebJobsStorage`adlı bir uygulama ayarına kaydedin.

1. Bir Application Insights kaynağı oluşturun ve bunun için **Genel** uygulama türünü kullanın.

1. Enstrümantasyon anahtarını bir uygulama `APPINSIGHTS_INSTRUMENTATIONKEY`ayarında kaydedin.

1. Webİş olarak dağıtın.

## <a name="webjobs-sdk-3x"></a>WebJobs SDK 3.x

Bu makalede, bir WebJobs SDK 2.x projesi nasıl geliştirilir açıklanmaktadır. [Bir WebJobs SDK 3.x](../../app-service/webjobs-sdk-get-started.md) projesi geliştiriyorsanız, bu bölüm farklılıkları anlamanıza yardımcı olur.

Tanıtılan temel değişiklik .NET Framework yerine .NET Core kullanımıdır. Bir WebJobs SDK 3.x projesi oluşturmak için, aşağıdaki durumlar dışında yönergeler aynıdır:

1. Bir .NET Core konsol uygulaması oluşturun. Visual Studio **New Project** iletişim kutusunda **.NET Core** > **Console App (.NET Core)** seçeneğini belirleyin. Proje dosyasında şu `TargetFramework` belirte `netcoreapp2.x`

1. Aşağıdaki paketlerin sürüm sürümünü WebJobs SDK 3.x seçin:

    * `Microsoft.Azure.WebJobs.Extensions`
    * `Microsoft.Azure.WebJobs.Extensions.Storage`
    * `Microsoft.Azure.WebJobs.Logging.ApplicationInsights`

1. .NET Core yapılandırma çerçevesini kullanarak, bir *appsettings.json* dosyasındaki depolama bağlantı dizesini ve Application Insights enstrümantasyon anahtarını ayarlayın. Bir örneği aşağıda verilmiştir:

    ```json
        {
            "AzureWebJobsStorage": "<replace with storage connection string>",
            "APPINSIGHTS_INSTRUMENTATIONKEY": "<replace with Application Insights instrumentation key>"
        }
    ```

1. Bunu `Main` yapmak için yöntem kodunu değiştirin. Bir örneği aşağıda verilmiştir:

   ```cs
   static void Main(string[] args)
   {
        var hostBuilder = new HostBuilder()
            .ConfigureWebJobs(config =>
            {
                config.AddAzureStorageCoreServices();
                config.AddAzureStorage();
                config.AddTimers();
                config.AddDurableTask(options =>
                {
                    options.HubName = "MyTaskHub";
                    options.AzureStorageConnectionStringName = "AzureWebJobsStorage";
                });
            })
            .ConfigureLogging((context, logging) =>
            {
                logging.AddConsole();
                logging.AddApplicationInsights(config =>
                {
                    config.InstrumentationKey = context.Configuration["APPINSIGHTS_INSTRUMENTATIONKEY"];
                });
            })
            .UseConsoleLifetime();

        var host = hostBuilder.Build();

        using (host)
        {
            host.Run();
        }
   }
   ```

## <a name="next-steps"></a>Sonraki adımlar

WebJobs SDK hakkında daha fazla bilgi edinmek için [Webİşler SDK'yı nasıl kullanacağınızı](../../app-service/webjobs-sdk-how-to.md)öğrenin.
