---
title: WebJobs SDK nasıl kullanılır?
description: WebJobs SDK için kod yazma hakkında daha fazla bilgi edinin. Azure ve üçüncü taraf hizmetlerinde verilere erişen olay odaklı arka plan işleme işleri oluşturun.
author: ggailey777
ms.devlang: dotnet
ms.topic: article
ms.date: 02/18/2019
ms.author: glenga
ms.openlocfilehash: 5a8d5f96449cfecd4628c38fa2788a1e06e96b07
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81758900"
---
# <a name="how-to-use-the-azure-webjobs-sdk-for-event-driven-background-processing"></a>Olay temelli arka plan işleme için Azure Web İşleri SDK'sını kullanma

Bu makalede, Azure Web İşlerI SDK ile nasıl çalışılabilenler hakkında kılavuz lar verilmektedir. Web İşler'e hemen başlamak [için, etkinlik odaklı arka plan işleme için Azure Web İşleri SDK'ya başlayın'ı](webjobs-sdk-get-started.md)görün. 

## <a name="webjobs-sdk-versions"></a>WebJobs SDK sürümleri

Bunlar sürüm 3 arasındaki temel farklar. *x* ve sürüm 2. WebJobs SDK *x:*

* Sürüm 3. *x* .NET Core için destek ekler.
* Sürüm 3'te. *x,* WebJobs SDK tarafından gerekli depolama bağlama uzantısını açıkça yüklemeniz gerekir. Sürüm 2'de. *x,* Depolama ciltleri SDK'ya dahil edilmiştir.
* .NET Core için Visual Studio aracı (3.* x*) projeleri .NET Framework için takımlamadan farklıdır (2.* x*) projeleri. Daha fazla bilgi edinmek için [Visual Studio - Azure Uygulama Hizmeti'ni kullanarak Web İşler geliştir in ve dağıt'a](webjobs-dotnet-deploy-vs.md)bakın.

Mümkün olduğunda, her iki sürüm 3 için de örnekler sağlanır. *x* ve sürüm 2. *x*.

> [!NOTE]
> [Azure İşlerİ](../azure-functions/functions-overview.md) Web İşler Iş. SDK'da oluşturulmuştür ve bu makale, bazı konular için Azure İşlevleri belgelerine bağlantılar sağlar. İşlevler ve WebJobs SDK arasındaki bu farkları not edin:
> * Azure İşlevler sürüm 2. *x,* WebJobs SDK sürüm 3'e karşılık gelir. *x*ve Azure İşlevleri 1. *x,* WebJobs SDK 2'ye karşılık gelir. *x*. Kaynak kodu depolarında WebJobs SDK numaralandırması kullanılır.
> * Azure İşlerİ C# sınıfı kitaplıkları için örnek kod, Webİşler `FunctionName` SDK kodu gibidir, ancak bir Webİşler SDK projesinde özniteliğe ihtiyacınız yoktur.
> * Bazı bağlama türleri yalnızca HTTP (Webhooks) ve Olay Izgarası (HTTP'ye dayalı) gibi işlevlerde desteklenir.
>
> Daha fazla bilgi için Bkz. [Web İşlerI SDK ve Azure İşlerini Karşılaştır.](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md#compare-functions-and-webjobs)

## <a name="webjobs-host"></a>WebJobs ana bilgisayar

Ana bilgisayar, işlevler için bir çalışma zamanı kapsayıcısır.  Tetikleyicileri dinler ve işlevleri çağırır. Sürüm 3'te. *x*, ana bilgisayar `IHost`bir uygulamadır . Sürüm 2'de. *x*, nesneyi `JobHost` kullanırsınız. Kodunuzda bir ana bilgisayar örneği oluşturur ve davranışını özelleştirmek için kod yazarsınız.

Bu, Web İşler SDK'sını doğrudan kullanmakla Azure İşlerinden dolaylı olarak kullanmak arasında önemli bir farktır. Azure İşlevler'de hizmet ana bilgisayarı denetler ve ana bilgisayarı kod yazarak özelleştiremezsiniz. Azure İşlevler, ana bilgisayar.json dosyasındaki ayarlar aracılığıyla ana bilgisayar davranışını özelleştirmenize olanak tanır. Bu ayarlar kod değil dizeleri dir ve bu yapabileceğiniz özelleştirme türlerini sınırlar.

### <a name="host-connection-strings"></a>Ana bilgisayar bağlantı dizeleri

WebJobs SDK, yerel olarak çalıştırdığınızda yerel ayarlar.json dosyasında veya Azure'da çalıştırdığınızda Webİş ortamında Azure Depolama ve Azure Hizmet Veri Yolunda bağlantı dizelerini arar. Varsayılan olarak, adlı `AzureWebJobsStorage` bir depolama bağlantısı dize ayarı gereklidir.  

Sürüm 2. *x* sdk bu bağlantı dizeleri için kendi adlarınızı kullanmanıza veya başka bir yerde saklamanızı sağlar. Burada gösterildiği [`JobHostConfiguration`]gibi, kodları koda ayarlayabilirsiniz:

```cs
static void Main(string[] args)
{
    var _storageConn = ConfigurationManager
        .ConnectionStrings["MyStorageConnection"].ConnectionString;

    //// Dashboard logging is deprecated; use Application Insights.
    //var _dashboardConn = ConfigurationManager
    //    .ConnectionStrings["MyDashboardConnection"].ConnectionString;

    JobHostConfiguration config = new JobHostConfiguration();
    config.StorageConnectionString = _storageConn;
    //config.DashboardConnectionString = _dashboardConn;
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

Çünkü sürüm 3. *x* varsayılan .NET Core yapılandırma API'lerini kullanır, bağlantı dize adlarını değiştirmek için API yoktur.

### <a name="host-development-settings"></a>Ana bilgisayar geliştirme ayarları

Yerel geliştirmeyi daha verimli hale getirmek için ana bilgisayarı geliştirme modunda çalıştırabilirsiniz. Geliştirme modunda çalıştırdığınızda değiştirilen ayarlardan bazıları şunlardır:

| Özellik | Geliştirme ayarı |
| ------------- | ------------- |
| `Tracing.ConsoleLevel` | `TraceLevel.Verbose`günlük çıktısını en üst düzeye çıkarmak için. |
| `Queues.MaxPollingInterval`  | Sıra yöntemlerinin hemen tetiklenmesini sağlamak için düşük bir değer.  |
| `Singleton.ListenerLockPeriod` | Hızlı yinelemeli gelişime yardımcı olmak için 15 saniye. |

Geliştirme modunu etkinleştirme işlemi SDK sürümüne bağlıdır. 

#### <a name="version-3x"></a>Sürüm 3. *x*

Sürüm 3. *x,* Core API'ASP.NET standartını kullanır. [`HostBuilder`](/dotnet/api/microsoft.extensions.hosting.hostbuilder) Örnekte [`UseEnvironment`](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useenvironment) yöntemi arayın. Bu örnekte `development`olduğu gibi, adlı bir dize geçirin:

```cs
static async Task Main()
{
    var builder = new HostBuilder();
    builder.UseEnvironment("development");
    builder.ConfigureWebJobs(b =>
            {
                b.AddAzureStorageCoreServices();
            });
    var host = builder.Build();
    using (host)
    {
        await host.RunAsync();
    }
}
```

#### <a name="version-2x"></a>Sürüm 2. *x*

Sınıfın `JobHostConfiguration` geliştirme `UseDevelopmentSettings` modunu etkinleştiren bir yöntemi vardır.  Aşağıdaki örnek, geliştirme ayarlarının nasıl kullanılacağını gösterir. Yerel `config.IsDevelopment` olarak `true` çalıştığında geri dönüş yapmak için, `AzureWebJobsEnv` değeri `Development`ile adlı yerel bir ortam değişkeni ayarlayın.

```cs
static void Main()
{
    config = new JobHostConfiguration();

    if (config.IsDevelopment)
    {
        config.UseDevelopmentSettings();
    }

    var host = new JobHost(config);
    host.RunAndBlock();
}
```

### <a name="managing-concurrent-connections-version-2x"></a><a name="jobhost-servicepointmanager-settings"></a>Eşzamanlı bağlantıları yönetme (sürüm 2.* x*)

Sürüm 3'te. *x*, bağlantı sınırı varsayılan sonsuz bağlantılar için. Bu sınırı değiştirmeniz gerekiyorsa, sınıfın özelliğini [`MaxConnectionsPerServer`](/dotnet/api/system.net.http.winhttphandler.maxconnectionsperserver) [`WinHttpHandler`](/dotnet/api/system.net.http.winhttphandler) kullanabilirsiniz.

Sürüm 2'de. *x*, [ServicePointManager.DefaultConnectionLimit](/dotnet/api/system.net.servicepointmanager.defaultconnectionlimit#System_Net_ServicePointManager_DefaultConnectionLimit) API'yi kullanarak bir ana bilgisayara eşzamanlı bağlantı sayısını denetlersiniz. 2'de. *x*, Webİşler ana bilgisayarını başlatmadan önce bu değeri 2 varsayılanından artırmalısınız.

Bir işlevden yaptığınız tüm giden HTTP `HttpClient` istekleri `ServicePointManager`akış kullanarak. Ayarlanan değere `DefaultConnectionLimit`ulaştıktan `ServicePointManager` sonra, göndermeden önce istekleri sıraya başlar. Sizin `DefaultConnectionLimit` 2 olarak ayarlanmış ve kod 1.000 HTTP istekleri yapar varsayalım. Başlangıçta, işletim sistemi için yalnızca iki istek izin verilir. Diğer 998'i onlara yer olana kadar sıraya girdi. Bu, `HttpClient` isteği yapmış gibi göründüğünden, ancak istek işletim sistemi tarafından hedef sunucuya gönderilmediği için zaman ınızı alabilirsiniz anlamına gelir. Bu nedenle, mantıklı görünmeyen davranışlar görebilirsiniz: `HttpClient` yerel cihazınızın bir isteği tamamlaması 10 saniye sürüyor, ancak hizmetiniz her isteği 200 ms'de iade ediyor. 

ASP.NET uygulamalar için varsayılan `Int32.MaxValue`değer, temel veya daha yüksek bir Uygulama Hizmeti Planı'nda çalışan Webİşler için iyi çalışması olasıdır. WebJobs genellikle Her Zaman Ayarı gerekir ve bu yalnızca Temel ve daha yüksek Uygulama Hizmeti Planları tarafından desteklenir.

WebJob'unuzun Ücretsiz veya Paylaşılan Uygulama Hizmet Planı'nda çalışması durumunda, uygulamanız şu anda [bağlantı sınırı 300](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#per-sandbox-per-appper-site-numerical-limits)olan App Service sandbox ile sınırlıdır. Sınırsız bağlantı sınırı yla, `ServicePointManager`kum havuzu bağlantı eşiğine ulaşılması ve sitenin kapatılması daha olasıdır. Bu durumda, `DefaultConnectionLimit` 50 veya 100 gibi daha düşük bir şeye ayar, bunun olmasını engelleyebilir ve yine de yeterli iş elde etmeye izin verebilir.

Herhangi bir HTTP isteği yapılmadan önce ayar yapılmalıdır. Bu nedenle, Web İşler ana bilgisayarı ayarı otomatik olarak ayarlamamalıdır. Ana bilgisayar başlamadan önce gerçekleşen VE beklenmeyen davranışlara yol açabilecek HTTP istekleri olabilir. En iyi `Main` `JobHost`yaklaşım, burada gösterildiği gibi, başlatmadan önce yönteminizdeki değeri hemen ayarlamaktır:

```csharp
static void Main(string[] args)
{
    // Set this immediately so that it's used by all requests.
    ServicePointManager.DefaultConnectionLimit = Int32.MaxValue;

    var host = new JobHost();
    host.RunAndBlock();
}
```

## <a name="triggers"></a>Tetikleyiciler

İşlevler ortak yöntemler olmalı ve bir [`NoAutomaticTrigger`](#manual-triggers) tetikleyici özniteliği veya özniteliği olmalıdır.

### <a name="automatic-triggers"></a>Otomatik tetikleyiciler

Otomatik tetikleyiciler bir olaya yanıt olarak bir işlevi çağırır. Azure Kuyruk depolama alanına eklenen bir ileti tarafından tetiklenen bir işlev örneği bu örneği göz önünde bulundurun. Azure Blob depolama dan bir blob okuyarak yanıt verir:

```cs
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{queueTrigger}", FileAccess.Read)] Stream myBlob,
    ILogger log)
{
    log.LogInformation($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}
```

Öznitelik, `QueueTrigger` `myqueue-items` kuyrukta bir kuyruk iletisi görüntülendiğinde işlevi çağırma saatini bildirir. Öznitelik, `Blob` *örnek çalışma öğeleri* kapsayıcısındaki bir blob'u okumak için sıra iletisini kullanma saatini söyler. `samples-workitems` Kapsayıcıdaki blob öğesinin adı, bağlama ifadesi olarak doğrudan sıra`{queueTrigger}`tetikleyicisinden elde edilir .

[!INCLUDE [webjobs-always-on-note](../../includes/webjobs-always-on-note.md)]

### <a name="manual-triggers"></a>Manuel tetikleyiciler

Bir işlevi el ile tetiklemek için, burada gösterildiği gibi özniteliği kullanın: `NoAutomaticTrigger`

```cs
[NoAutomaticTrigger]
public static void CreateQueueMessage(
ILogger logger,
string value,
[Queue("outputqueue")] out string message)
{
    message = value;
    logger.LogInformation("Creating queue message: ", message);
}
```

İşlevin el ile tetiklenmesi işlemi SDK sürümüne bağlıdır.

#### <a name="version-3x"></a>Sürüm 3. *x*

```cs
static async Task Main(string[] args)
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddAzureStorage();
    });
    var host = builder.Build();
    using (host)
    {
        var jobHost = host.Services.GetService(typeof(IJobHost)) as JobHost;
        var inputs = new Dictionary<string, object>
        {
            { "value", "Hello world!" }
        };

        await host.StartAsync();
        await jobHost.CallAsync("CreateQueueMessage", inputs);
        await host.StopAsync();
    }
}
```

#### <a name="version-2x"></a>Sürüm 2. *x*

```cs
static void Main(string[] args)
{
    JobHost host = new JobHost();
    host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
}
```

## <a name="input-and-output-bindings"></a>Giriş ve çıkış bağlamaları

Giriş bağlamaları, Azure veya üçüncü taraf hizmetlerinden gelen verileri kodunuzda kullanılabilir hale getirmek için bildirimsel bir yol sağlar. Çıktı bağlamaları verileri güncelleştirmek için bir yol sağlar. [Başlat](webjobs-sdk-get-started.md) makalesi her birinin bir örneğini gösterir.

Yöntem iade değeriözasyon uygulayarak çıktı bağlama için bir yöntem iade değeri kullanabilirsiniz. [Azure İşi return değerini kullanma](../azure-functions/functions-bindings-return-value.md)örneğine bakın.

## <a name="binding-types"></a>Bağlama türleri

Bağlama türlerini yükleme ve yönetme işlemi, sürüm 3'nü kullanıp kullanmadığınıza bağlıdır. *x* veya sürüm 2. SDK *x.* Belirli bir bağlama türü için yüklenmesi gereken paketi, bu bağlama türünün Azure İşlevleri [başvuru makalesinin](#binding-reference-information)"Paketler" bölümünde bulabilirsiniz. Bunun bir istisnası, Azure İşlevleri tarafından desteklenmeyen Dosyalar tetikleyicisi ve bağlama (yerel dosya sistemi için) dir.

#### <a name="version-3x"></a>Sürüm 3. *x*

Sürüm 3'te. *x,* depolama ciltleri `Microsoft.Azure.WebJobs.Extensions.Storage` pakete dahildir. Burada `AddAzureStorage` gösterildiği gibi, `ConfigureWebJobs` yöntemde uzantı yöntemini çağırın:

```cs
static async Task Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
            {
                b.AddAzureStorageCoreServices();
                b.AddAzureStorage();
            });
    var host = builder.Build();
    using (host)
    {
        await host.RunAsync();
    }
}
```

Diğer tetikleyici ve bağlama türlerini kullanmak için, bunları içeren `Add<binding>` NuGet paketini yükleyin ve uzantıda uygulanan uzantı yöntemini çağırın. Örneğin, bir Azure Cosmos DB bağlama kullanmak `Microsoft.Azure.WebJobs.Extensions.CosmosDB` istiyorsanız, `AddCosmosDB`yükleyin ve çağırın, bu gibi:

```cs
static async Task Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
            {
                b.AddAzureStorageCoreServices();
                b.AddCosmosDB();
            });
    var host = builder.Build();
    using (host)
    {
        await host.RunAsync();
    }
}
```

Temel hizmetlerin bir parçası olan Zamanlayıcı tetikleyicisini veya Dosyaları `AddTimers` bağlamayı kullanmak için sırasıyla veya `AddFiles` uzantı yöntemlerini arayın.

#### <a name="version-2x"></a>Sürüm 2. *x*

Bu tetikleyici ve bağlama türleri sürüm 2'ye dahildir. `Microsoft.Azure.WebJobs` paketin *x:*

* Blob depolama
* Kuyruk depolama
* Tablo depolama

Diğer tetikleyici ve bağlama türlerini kullanmak için, bunları içeren `Use<binding>` NuGet `JobHostConfiguration` paketini yükleyin ve nesneye bir yöntem çağırın. Örneğin, burada gösterildiği gibi `Microsoft.Azure.WebJobs.Extensions` `UseTimers` `Main` yöntemde bir Timer tetikleyicisi, yükleme ve arama yapmak istiyorsanız:

```cs
static void Main()
{
    config = new JobHostConfiguration();
    config.UseTimers();
    var host = new JobHost(config);
    host.RunAndBlock();
}
```

Dosyaları bağlamayı kullanmak `Microsoft.Azure.WebJobs.Extensions` için `UseFiles`yükleyin ve arayın.

### <a name="executioncontext"></a>Executioncontext

WebJobs bir [`ExecutionContext`]. Bu bağlama ile işlev [`ExecutionContext`] imzanızda parametre olarak erişebilirsiniz. Örneğin, aşağıdaki kod, belirli bir işlev çağırma tarafından üretilen tüm günlükleri ilişkilendirmek için kullanabileceğiniz çağırma kimliğine erişmek için bağlam nesnesini kullanır.  

```cs
public class Functions
{
    public static void ProcessQueueMessage([QueueTrigger("queue")] string message,
        ExecutionContext executionContext,
        ILogger logger)
    {
        logger.LogInformation($"{message}\n{executionContext.InvocationId}");
    }
}
```

Bağlama işlemi SDK [`ExecutionContext`] sürümünüze bağlıdır.

#### <a name="version-3x"></a>Sürüm 3. *x*

Burada `AddExecutionContextBinding` gösterildiği gibi, `ConfigureWebJobs` yöntemde uzantı yöntemini çağırın:

```cs
static async Task Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
            {
                b.AddAzureStorageCoreServices();
                b.AddExecutionContextBinding();
            });
    var host = builder.Build();
    using (host)
    {
        await host.RunAsync();
    }
}
```

#### <a name="version-2x"></a>Sürüm 2. *x*

Daha `Microsoft.Azure.WebJobs.Extensions` önce bahsedilen paket, `UseCore` yöntemi arayarak kaydolabileceğiniz özel bir bağlama türü de sağlar. Bu bağlama, işlev [`ExecutionContext`] imzanızda aşağıdaki gibi etkinleştirilen bir parametre tanımlamanızı sağlar:

```cs
class Program
{
    static void Main()
    {
        config = new JobHostConfiguration();
        config.UseCore();
        var host = new JobHost(config);
        host.RunAndBlock();
    }
}
```

## <a name="binding-configuration"></a>Bağlama yapılandırması

Bazı tetikleyicilerin ve bağlamaların davranışını yapılandırabilirsiniz. Bunları yapılandırma işlemi SDK sürümüne bağlıdır.

* **Sürüm 3. *x*:** Yöntem `Add<Binding>` çağrıldığında `ConfigureWebJobs`yapılandırmayı ayarlayın.
* **Sürüm 2. *x*:** `JobHost`' ye geçtiğiniz bir yapılandırma nesnesi özellikleri ayarlayarak yapılandırmayı ayarlayın

Bu bağlamaya özel ayarlar, Azure İşlevlerindeki [ana bilgisayar.json proje dosyasındaki](../azure-functions/functions-host-json.md) ayarlara eşdeğerdir.

Aşağıdaki bağlamaları yapılandırabilirsiniz:

* [Azure CosmosDB tetikleyicisi](#azure-cosmosdb-trigger-configuration-version-3x)
* [Olay Hub'ları tetikleyici](#event-hubs-trigger-configuration-version-3x)
* [Kuyruk depolama tetikleyicisi](#queue-storage-trigger-configuration)
* [SendGrid bağlama](#sendgrid-binding-configuration-version-3x)
* [Servis Veri Otobüsü tetikleyicisi](#service-bus-trigger-configuration-version-3x)

### <a name="azure-cosmosdb-trigger-configuration-version-3x"></a>Azure CosmosDB tetik yapılandırması (sürüm 3.* x*)

Bu örnek, Azure Cosmos DB tetikleyicisinin nasıl yapılandırılabildiğini gösterir:

```cs
static async Task Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddCosmosDB(a =>
        {
            a.ConnectionMode = ConnectionMode.Gateway;
            a.Protocol = Protocol.Https;
            a.LeaseOptions.LeasePrefix = "prefix1";

        });
    });
    var host = builder.Build();
    using (host)
    {
        await host.RunAsync();
    }
}
```

Daha fazla ayrıntı için [Azure CosmosDB bağlama](../azure-functions/functions-bindings-cosmosdb-v2-output.md#hostjson-settings) makalesine bakın.

### <a name="event-hubs-trigger-configuration-version-3x"></a>Olay Hub'ları yapılandırmayı tetikler (sürüm 3.* x*)

Bu örnek, Olay Hub'larının tetikleyicisinin nasıl yapılandırılabildiğini gösterir:

```cs
static async Task Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddEventHubs(a =>
        {
            a.BatchCheckpointFrequency = 5;
            a.EventProcessorOptions.MaxBatchSize = 256;
            a.EventProcessorOptions.PrefetchCount = 512;
        });
    });
    var host = builder.Build();
    using (host)
    {
        await host.RunAsync();
    }
}
```

Daha fazla ayrıntı için [Olay Hub'ları bağlayıcı](../azure-functions/functions-bindings-event-hubs-trigger.md#host-json) makaleye bakın.

### <a name="queue-storage-trigger-configuration"></a>Sıra depolama tetikleyici yapılandırması

Bu örnekler, Sıra depolama tetikleyicisinin nasıl yapılandırılabildiğini gösterir:

#### <a name="version-3x"></a>Sürüm 3. *x*

```cs
static async Task Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddAzureStorage(a => {
            a.BatchSize = 8;
            a.NewBatchThreshold = 4;
            a.MaxDequeueCount = 4;
            a.MaxPollingInterval = TimeSpan.FromSeconds(15);
        });
    });
    var host = builder.Build();
    using (host)
    {
        await host.RunAsync();
    }
}
```

Daha fazla ayrıntı için [Sıra depolama bağlama](../azure-functions/functions-bindings-storage-queue-trigger.md#hostjson-properties) makalesine bakın.

#### <a name="version-2x"></a>Sürüm 2. *x*

```cs
static void Main(string[] args)
{
    JobHostConfiguration config = new JobHostConfiguration();
    config.Queues.BatchSize = 8;
    config.Queues.NewBatchThreshold = 4;
    config.Queues.MaxDequeueCount = 4;
    config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

Daha fazla bilgi için [host.json v1.x referansına](../azure-functions/functions-host-json-v1.md#queues)bakın.

### <a name="sendgrid-binding-configuration-version-3x"></a>SendGrid bağlama yapılandırması (sürüm 3.* x*)

Bu örnek, SendGrid çıktı bağlamanasıl yapılandırılabildiğini gösterir:

```cs
static async Task Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddSendGrid(a =>
        {
            a.FromAddress.Email = "samples@functions.com";
            a.FromAddress.Name = "Azure Functions";
        });
    });
    var host = builder.Build();
    using (host)
    {
        await host.RunAsync();
    }
}
```

Daha fazla ayrıntı için [SendGrid bağlama](../azure-functions/functions-bindings-sendgrid.md#hostjson-settings) makalesine bakın.

### <a name="service-bus-trigger-configuration-version-3x"></a>Servis Veri Servisi tetik yapılandırması (sürüm 3.* x*)

Bu örnek, Hizmet Veri Servisi tetikleyicisinin nasıl yapılandırılabildiğini gösterir:

```cs
static async Task Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddServiceBus(sbOptions =>
        {
            sbOptions.MessageHandlerOptions.AutoComplete = true;
            sbOptions.MessageHandlerOptions.MaxConcurrentCalls = 16;
        });
    });
    var host = builder.Build();
    using (host)
    {
        await host.RunAsync();
    }
}
```

Daha fazla ayrıntı için [Servis Veri Servisi bağlayıcı](../azure-functions/functions-bindings-service-bus-output.md#hostjson-settings) makalesine bakın.

### <a name="configuration-for-other-bindings"></a>Diğer bağlamalar için yapılandırma

Bazı tetikleyici ve bağlama türleri kendi özel yapılandırma türlerini tanımlar. Örneğin, Dosya tetikleyicisi, aşağıdaki örneklerde olduğu gibi izlenecek kök yolu belirtmenize olanak tanır:

#### <a name="version-3x"></a>Sürüm 3. *x*

```cs
static async Task Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddFiles(a => a.RootPath = @"c:\data\import");
    });
    var host = builder.Build();
    using (host)
    {
        await host.RunAsync();
    }
}
```

#### <a name="version-2x"></a>Sürüm 2. *x*

```cs
static void Main()
{
    config = new JobHostConfiguration();
    var filesConfig = new FilesConfiguration
    {
        RootPath = @"c:\data\import"
    };
    config.UseFiles(filesConfig);
    var host = new JobHost(config);
    host.RunAndBlock();
}
```

## <a name="binding-expressions"></a>Bağlama ifadeleri

Öznitelik oluşturucu parametrelerinde, çeşitli kaynaklardan gelen değerlere çözüm veren ifadeler kullanabilirsiniz. Örneğin, aşağıdaki kodda, `BlobTrigger` öznitelik için yol adlı `filename`bir ifade oluşturur. Çıktı bağlama için kullanıldığında, `filename` tetikleyici blob adına giderir.

```cs
public static void CreateThumbnail(
    [BlobTrigger("sample-images/{filename}")] Stream image,
    [Blob("sample-images-sm/{filename}", FileAccess.Write)] Stream imageSmall,
    string filename,
    ILogger logger)
{
    logger.Info($"Blob trigger processing: {filename}");
    // ...
}
```

Bağlama ifadeleri hakkında daha fazla bilgi için Azure İşlevleri belgelerinde [bağlayıcı ifadeler ve desenler](../azure-functions/functions-bindings-expressions-patterns.md) ebakını görün.

### <a name="custom-binding-expressions"></a>Özel bağlama ifadeleri

Bazen bir sıra adı, bir blob adı veya kapsayıcı veya kod içinde bir tablo adı yerine zor kodlama belirtmek istiyorum. Örneğin, bir yapılandırma dosyasıveya ortam değişkeninde `QueueTrigger` öznitelik için sıra adını belirtmek isteyebilirsiniz.

Bunu nesneye `NameResolver` `JobHostConfiguration` bir nesne geçirerek yapabilirsiniz. Yer tutucuları tetikleyici veya bağlama öznitelik oluşturucu `NameResolver` parametrelerine eklersiniz ve kodunuz bu yer tutucuların yerine kullanılacak gerçek değerleri sağlar. Yer tutucuları yüzde (%) ile çevreleyerek tanımlarsınız işaretler, burada gösterildiği gibi:

```cs
public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
{
    Console.WriteLine(logMessage);
}
```

Bu kod, test ortamında `logqueuetest` ve üretimde adı `logqueueprod` geçen bir sıra kullanmanıza olanak sağlar. Sabit kodlanmış bir sıra adı yerine, `appSettings` koleksiyondaki bir girişin adını belirtirsiniz.

Özel bir varsayılan `NameResolver` sağlamazsanız etkili olan bir varsayılan vardır. Varsayılan değer, uygulama ayarlarından veya ortam değişkenlerinden değerler alır.

Sınıfınız, `NameResolver` burada gösterildiği `appSettings`gibi sıra adını alır:

```cs
public class CustomNameResolver : INameResolver
{
    public string Resolve(string name)
    {
        return ConfigurationManager.AppSettings[name].ToString();
    }
}
```

#### <a name="version-3x"></a>Sürüm 3. *x*

Bağımlılık enjeksiyonu kullanarak çözümleyiciyi yapılandırAbilirsiniz. Bu örnekler aşağıdaki `using` ifadeyi gerektirir:

```cs
using Microsoft.Extensions.DependencyInjection;
```

Bu örnekte olduğu gibi [`ConfigureServices`] [`HostBuilder`](/dotnet/api/microsoft.extensions.hosting.hostbuilder)uzantı yöntemini çağırarak çözümleyiciyi eklersiniz:

```cs
static async Task Main(string[] args)
{
    var builder = new HostBuilder();
    var resolver = new CustomNameResolver();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
    });
    builder.ConfigureServices(s => s.AddSingleton<INameResolver>(resolver));
    var host = builder.Build();
    using (host)
    {
        await host.RunAsync();
    }
}
```

#### <a name="version-2x"></a>Sürüm 2. *x*

Burada `NameResolver` gösterildiği gibi `JobHost` sınıfınızı nesneye geçirin:

```cs
 static void Main(string[] args)
{
    JobHostConfiguration config = new JobHostConfiguration();
    config.NameResolver = new CustomNameResolver();
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

Azure İşlevleri, `INameResolver` örnekte gösterildiği gibi uygulama ayarlarından değerler almak için uygular. WebJobs SDK'yı doğrudan kullandığınızda, tercih ettiğiniz kaynaktan yer tutucu değiştirme değerlerini alan özel bir uygulama yazabilirsiniz.

## <a name="binding-at-runtime"></a>Çalışma zamanında bağlama

", " veya `Table`, , gibi `IBinder` `Blob`bağlayıcı bir öznitelik kullanmadan önce işlevinizde bazı işler yapmanız gerekiyorsa, arabirimi kullanabilirsiniz. `Queue`

Aşağıdaki örnek, bir giriş sırası iletisi alır ve çıktı kuyruğunda aynı içeriğe sahip yeni bir ileti oluşturur. Çıkış sıra adı işlevin gövdesinde kod tarafından ayarlanır.

```cs
public static void CreateQueueMessage(
    [QueueTrigger("inputqueue")] string queueMessage,
    IBinder binder)
{
    string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
    QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
    CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
    outputQueue.AddMessageAsync(new CloudQueueMessage(queueMessage));
}
```

Daha fazla bilgi için Azure İşlevleri belgelerinde [çalışma zamanında Bağlama'ya](../azure-functions/functions-dotnet-class-library.md#binding-at-runtime) bakın.

## <a name="binding-reference-information"></a>Bağlayıcı referans bilgileri

Azure İşlevler belgeleri her bağlama türü hakkında başvuru bilgileri sağlar. Her bağlayıcı başvuru makalesinde aşağıdaki bilgileri bulacaksınız. (Bu örnek Depolama sırasına dayanır.)

* [Paketler](../azure-functions/functions-bindings-storage-queue.md). Bir WebJobs SDK projesinde bağlama desteği içerecek şekilde yüklemeniz gereken paket.
* [Örnekler](../azure-functions/functions-bindings-storage-queue-trigger.md). Kod örnekleri. C# sınıfı kitaplık örneği WebJobs SDK için geçerlidir. Sadece özniteliği `FunctionName` atla.
* [Öznitelikler](../azure-functions/functions-bindings-storage-queue-trigger.md#attributes-and-annotations). Bağlama türü için kullanılacak öznitelikler.
* [Yapılandırma](../azure-functions/functions-bindings-storage-queue-trigger.md#configuration). Öznitelik özellikleri ve yapıcı parametrelerin açıklamaları.
* [Kullanım](../azure-functions/functions-bindings-storage-queue-trigger.md#usage). Bağlayabileceğiniz türleri ve bağlamanın nasıl çalıştığı hakkında bilgi. Örneğin: yoklama algoritması, zehirli sıra işleme.
  
Bağlayıcı başvuru makalelerinin listesi için, Azure İşlevleri için [Tetikleyiciler ve bağlamalar](../azure-functions/functions-triggers-bindings.md#supported-bindings) makalesindeki "Desteklenen bağlamalar" bölümüne bakın. Bu listede, HTTP, Webhooks ve Olay Izgara bağlamaları WebJobs SDK tarafından değil, yalnızca Azure İşleri tarafından desteklenir.

## <a name="disable-attribute"></a>Özniteliği devre dışı 

Öznitelik, [`Disable`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/DisableAttribute.cs) bir işlevin tetiklenip tetiklenmeyeceğini denetlemenize olanak tanır. 

Aşağıdaki örnekte, uygulama ayarı `Disable_TestJob` bir `1` değere sahipse veya `True` (büyük/küçük harf duyarsızca) işlev çalışmaz. Bu durumda, çalışma zamanı bir günlük *iletisi Işlevi 'Functions.TestJob' devre dışı bırakılır*oluşturur.

```cs
[Disable("Disable_TestJob")]
public static void TestJob([QueueTrigger("testqueue2")] string message)
{
    Console.WriteLine("Function with Disable attribute executed!");
}
```

Azure portalında uygulama ayar değerlerini değiştirdiğinizde, WebJob yeni ayarı almak için yeniden başlar.

Öznitelik parametre, yöntem veya sınıf düzeyinde bildirilebilir. Ayar adı bağlayıcı ifadeler de içerebilir.

## <a name="timeout-attribute"></a>Zaman arası özniteliği

Öznitelik, [`Timeout`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TimeoutAttribute.cs) belirli bir süre içinde bitmezse işlevin iptal edilmesine neden olur. Aşağıdaki örnekte, işlev Timeout özniteliği olmadan bir gün boyunca çalışır. Zaman anına 15 saniye sonra işlevin iptal edilmesine neden olur.

```cs
[Timeout("00:00:15")]
public static async Task TimeoutJob(
    [QueueTrigger("testqueue2")] string message,
    CancellationToken token,
    TextWriter log)
{
    await log.WriteLineAsync("Job starting");
    await Task.Delay(TimeSpan.FromDays(1), token);
    await log.WriteLineAsync("Job completed");
}
```

Zaman Alameti özniteliğini sınıf veya yöntem düzeyinde uygulayabilir ve genel `JobHostConfiguration.FunctionTimeout`bir zaman açiğini kullanarak belirtebilirsiniz. Sınıf düzeyinde veya yöntem düzeyinde zaman ayarı genel zaman zaman larını geçersiz kılar.

## <a name="singleton-attribute"></a>Singleton özniteliği

Öznitelik, [`Singleton`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/SingletonAttribute.cs) ana bilgisayar web uygulamasının birden çok örneği olsa bile, bir işlevin yalnızca bir örneğinin çalışmasını sağlar. Bunu [dağıtılmış kilitleme](#viewing-lease-blobs)kullanarak yapar.

Bu örnekte, işlevin `ProcessImage` yalnızca tek bir örneği herhangi bir zamanda çalışır:

```cs
[Singleton]
public static async Task ProcessImage([BlobTrigger("images")] Stream image)
{
     // Process the image.
}
```

### <a name="singletonmodelistener"></a>SingletonMode.Dinleyici

Bazı tetikleyiciler eşzamanlılık yönetimi için yerleşik desteğe sahiptir:

* **QueueTrigger**. `1`Ayarlayın. `JobHostConfiguration.Queues.BatchSize`
* **ServiceBusTrigger**. `1`Ayarlayın. `ServiceBusConfiguration.MessageOptions.MaxConcurrentCalls`
* **FileTrigger**. `1`Ayarlayın. `FileProcessor.MaxDegreeOfParallelism`

Bu ayarları, işlevinizin tek bir örnekte tek ton olarak çalıştığından emin olmak için kullanabilirsiniz. Web uygulaması birden çok örneğe ölçeklendiğinde işlevin yalnızca tek bir örneğinin çalıştığından emin olmak için, işleve`[Singleton(Mode = SingletonMode.Listener)]`dinleyici düzeyinde singleton kilidi uygulayın ( ). Dinleyici kilitleri JobHost başladığında elde edilir. Üç ölçeklenmiş örnek aynı anda başlatılırsa, yalnızca bir örnek kilidi kazanır ve yalnızca bir dinleyici başlar.

### <a name="scope-values"></a>Kapsam değerleri

Bir singleton üzerinde bir *kapsam ifadesi/değeri* belirtebilirsiniz. İfade/değer, işlevin belirli bir kapsamdaki tüm yürütmelerinin serihale getirilmesini sağlar. Bu şekilde daha ayrıntılı kilitleme uygulamak, gereksinimleriniz tarafından dikte edilen diğer çağrıları seri hale getirirken işleviniz için bir miktar paralellik sağlar. Örneğin, aşağıdaki kodda, kapsam ifadesi gelen iletinin değerine `Region` bağlanır. Sıra, doğu, doğu ve batı bölgelerinde sırasıyla üç ileti içeriyorsa, Doğu bölgesi olan iletiler seri olarak çalıştırılırken, Bölge Batı'daki ileti Doğu'dakiyle paralel olarak çalıştırılır.

```csharp
[Singleton("{Region}")]
public static async Task ProcessWorkItem([QueueTrigger("workitems")] WorkItem workItem)
{
     // Process the work item.
}

public class WorkItem
{
     public int ID { get; set; }
     public string Region { get; set; }
     public int Category { get; set; }
     public string Description { get; set; }
}
```

### <a name="singletonscopehost"></a>SingletonScope.Host

Bir kilit için varsayılan `SingletonScope.Function`kapsam, kilit kapsamı (blob kiralama yolu) tam nitelikli işlev adına bağlı olduğu anlamına gelir. İşlevler arasında `SingletonScope.Host` kilitlemek için, aynı anda çalıştırmak istemediğiniz tüm işlevlerde aynı olan bir kapsam kimliği adı belirtin ve kullanın. Aşağıdaki örnekte, aynı anda yalnızca bir örneği `AddItem` veya `RemoveItem` çalışır:

```csharp
[Singleton("ItemsLock", SingletonScope.Host)]
public static void AddItem([QueueTrigger("add-item")] string message)
{
     // Perform the add operation.
}

[Singleton("ItemsLock", SingletonScope.Host)]
public static void RemoveItem([QueueTrigger("remove-item")] string message)
{
     // Perform the remove operation.
}
```

### <a name="viewing-lease-blobs"></a>Kira lekelerini görüntüleme

WebJobs SDK, dağıtılmış kilitlemeyi uygulamak için kapakların altında [Azure blob kiralamaları](../storage/common/storage-concurrency.md#pessimistic-concurrency-for-blobs) kullanır. Singleton tarafından kullanılan kira lekeleri yol `azure-webjobs-host` "kilitler" altında `AzureWebJobsStorage` depolama hesabında konteyner bulunabilir. Örneğin, daha önce gösterilen ilk `ProcessImage` örnek için kira `locks/061851c758f04938a4426aa9ab3869c0/WebJobs.Functions.ProcessImage`blob yolu olabilir. Tüm yollar JobHost ID, bu durumda 061851c758f04938a4426aaa9ab3869c0 içerir.

## <a name="async-functions"></a>Async fonksiyonları

Async işlevlerinin nasıl kodlanır hakkında bilgi için [Azure İşlevleri belgelerine](../azure-functions/functions-dotnet-class-library.md#async)bakın.

## <a name="cancellation-tokens"></a>İptal belirteçleri

İptal belirteçleri nasıl işleyeceğiniz hakkında bilgi [için, iptal belirteçleri ve zarif kapatma](../azure-functions/functions-dotnet-class-library.md#cancellation-tokens)yla ilgili Azure İşlevleri belgelerine bakın.

## <a name="multiple-instances"></a>Birden çok örnek

Web uygulamanız birden çok örnekte çalışıyorsa, tetikleyicileri ve arama işlevlerini dinleyerek her örnekte sürekli bir WebJob çalışır. Çeşitli tetikleyici bağlamaları, daha fazla örne ölçeklemenin daha fazla yük taşımanızı sağlayacak şekilde, çalışmaları örnekler arasında işbirliği içinde verimli bir şekilde paylaşmak üzere tasarlanmıştır.

Bazı tetikleyiciler çift işlemeyle sonuçlanabilirken, sıra ve blob depolama tetikleyicileri bir işlevin bir sıra iletisini işlemesini veya birden fazla kez blob çalışmasını otomatik olarak engeller. Daha fazla bilgi için Azure İşlevleri belgelerinde [aynı girdi için Tasarım](../azure-functions/functions-idempotent.md) bölümüne bakın.

Zamanlayıcı tetikleyicisi otomatik olarak zamanlayıcının yalnızca bir örneğinin çalışmasını sağlar, böylece belirli bir zamanlanan zamanda birden fazla işlev örneği çalışmazsınız.

Ana bilgisayar web uygulamasının birden çok örneği olsa bile bir işlevin yalnızca bir örneğinin [`Singleton`](#singleton-attribute) çalıştığından emin olmak istiyorsanız, bu özelliği kullanabilirsiniz.

## <a name="filters"></a>Filtreler

İşlev Filtreleri (önizleme), Web İşler yürütme ardışık hattını kendi mantığınızla özelleştirmenin bir yolunu sağlar. Filtreler [ASP.NET Core filtrelerine](https://docs.microsoft.com/aspnet/core/mvc/controllers/filters)benzer. Bunları işlevlerinize veya sınıflarınıza uygulanan bildirimsel öznitelikler olarak uygulayabilirsiniz. Daha fazla bilgi için [Bkz. İşlev Filtreleri.](https://github.com/Azure/azure-webjobs-sdk/wiki/Function-Filters)

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

ASP.NET için geliştirilen günlük çerçevesini öneririz. [Başlangıç](webjobs-sdk-get-started.md) makalesi nasıl kullanılacağını gösterir. 

### <a name="log-filtering"></a>Günlük filtreleme

Bir `ILogger` örnek tarafından oluşturulan her `Category` `Level`günlük ilişkili ve . [`LogLevel`](/dotnet/api/microsoft.extensions.logging.loglevel)numaralandırmadır ve sonsayı kodu göreli önemi gösterir:

|LogLevel    |Kod|
|------------|---|
|İzleme       | 0 |
|Hata ayıklama       | 1 |
|Bilgi | 2 |
|Uyarı     | 3 |
|Hata       | 4 |
|Kritik    | 5 |
|Hiçbiri        | 6 |

Her kategoriyi bağımsız olarak belirli [`LogLevel`](/dotnet/api/microsoft.extensions.logging.loglevel)bir kategoriye filtreleyebilirsiniz. Örneğin, blob tetikleme işleme için tüm günlükleri `Error` görmek isteyebilirsiniz ama sadece ve her şey için daha yüksek.

#### <a name="version-3x"></a>Sürüm 3. *x*

Sürüm 3. *x* sdk .NET Core yerleşik filtreleme dayanır. Sınıf, `LogCategories` belirli işlevler, tetikleyiciler veya kullanıcılar için kategoriler tanımlamanıza olanak tanır. Ayrıca, belirli ana bilgisayar durumları için `Startup` `Results`filtreler tanımlar, gibi ve. Bu, günlüğe kaydetme çıktısına ince ayar yapmanızı sağlar. Tanımlanan kategorilerde eşleşme bulunmazsa, iletiye filtre `Default` uygulayıp filtrelemeyeceğine karar verirken filtre değere geri döner.

`LogCategories`aşağıdaki leri kullanarak ifade gerektirir:

```cs
using Microsoft.Azure.WebJobs.Logging; 
```

Aşağıdaki örnek, varsayılan olarak tüm günlükleri `Warning` düzeyinde filtreleyen bir filtre oluşturuyor. Ve kategoriler (sürüm `Host.Results` 2'ye eşdeğerdir. `results` `Function` * x*) `Error` düzeyinde süzülür. Filtre, geçerli kategoriyi `LogCategories` örnekteki tüm kayıtlı düzeylerle karşılaştırır ve en uzun eşleşmeyi seçer. Bu, `Debug` eşleşmeler `Host.Triggers` `Host.Triggers.Queue` için kayıtlı `Host.Triggers.Blob`düzeyin veya . Bu, her birini eklemenize gerek kalmadan daha geniş kategorileri denetlemenize olanak tanır.

```cs
static async Task Main(string[] args)
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
    });
    builder.ConfigureLogging(logging =>
            {
                logging.SetMinimumLevel(LogLevel.Warning);
                logging.AddFilter("Function", LogLevel.Error);
                logging.AddFilter(LogCategories.CreateFunctionCategory("MySpecificFunctionName"),
                    LogLevel.Debug);
                logging.AddFilter(LogCategories.Results, LogLevel.Error);
                logging.AddFilter("Host.Triggers", LogLevel.Debug);
            });
    var host = builder.Build();
    using (host)
    {
        await host.RunAsync();
    }
}
```

#### <a name="version-2x"></a>Sürüm 2. *x*

Sürüm 2'de. *x* SDK, filtreleme `LogCategoryFilter` denetlemek için sınıf kullanın. Başlangıç `LogCategoryFilter` değeri `Default` olan bir özelliğe sahip olan `Information`, `Information`yani `Warning` `Error`, `Critical` , , , veya düzeylerdeki `Debug` iletilerin günlüğe kaydedildiği, ancak veya düzeylerdeki `Trace` iletilerin filtrelenmiş olması anlamına gelir.

Sürüm `LogCategories` 3'te olduğu gibi. *x*, `CategoryLevels` özellik, günlük çıktıince ayar böylece belirli kategoriler için günlük düzeyleri belirtmenize olanak sağlar. `CategoryLevels` Sözlükte eşleşme bulunmazsa, iletiye filtre uygulayıp filtrelemeyeceğine karar verirken filtre `Default` değere geri döner.

Aşağıdaki örnekte, varsayılan olarak tüm günlükleri düzeyinde `Warning` filtreleyen bir filtre yapılsın. `Function` Ve `Host.Results` kategoriler `Error` düzeyinde filtrelenir. Geçerli `LogCategoryFilter` kategoriyi tüm kayıtlılarla `CategoryLevels` karşılaştırır ve en uzun eşleşmeyi seçer. Yani `Debug` kayıtlı seviye `Host.Triggers` `Host.Triggers.Queue` eşleşecek `Host.Triggers.Blob`ya da . Bu, her birini eklemenize gerek kalmadan daha geniş kategorileri denetlemenize olanak tanır.

```csharp
var filter = new LogCategoryFilter();
filter.DefaultLevel = LogLevel.Warning;
filter.CategoryLevels[LogCategories.Function] = LogLevel.Error;
filter.CategoryLevels[LogCategories.Results] = LogLevel.Error;
filter.CategoryLevels["Host.Triggers"] = LogLevel.Debug;

config.LoggerFactory = new LoggerFactory()
    .AddApplicationInsights(instrumentationKey, filter.Filter)
    .AddConsole(filter.Filter);
```

### <a name="custom-telemetry-for-application-insights"></a>Uygulama Öngörüleri için özel telemetri

[Uygulama Öngörüleri](../azure-monitor/app/app-insights-overview.md) için özel telemetri uygulama işlemi SDK sürümüne bağlıdır. Uygulama Öngörülerini nasıl yapılandırılabildiğini öğrenmek için [bkz.](webjobs-sdk-get-started.md#add-application-insights-logging)

#### <a name="version-3x"></a>Sürüm 3. *x*

Çünkü sürüm 3. *X* WebJobs SDK .NET Core genel ana bilgisayara dayanır, özel bir telemetri fabrikası artık sağlanmaz. Ancak, bağımlılık enjeksiyonu kullanarak boru hattına özel telemetri ekleyebilirsiniz. Bu bölümdeki örnekler aşağıdaki `using` ifadeleri gerektirir:

```cs
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.ApplicationInsights.Channel;
```

Aşağıdaki özel uygulama [`ITelemetryInitializer`] varsayılan [`ITelemetry`](/dotnet/api/microsoft.applicationinsights.channel.itelemetry) [`TelemetryConfiguration`]kendi eklemenize olanak sağlar.

```cs
internal class CustomTelemetryInitializer : ITelemetryInitializer
{
    public void Initialize(ITelemetry telemetry)
    {
        // Do something with telemetry.
    }
}
```

Özel [`ConfigureServices`] inizi boru hattına [`ITelemetryInitializer`] eklemek için oluşturucuyu çağırın.

```cs
static async Task Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
    });
    builder.ConfigureLogging((context, b) =>
    {
        // Add logging providers.
        b.AddConsole();

        // If this key exists in any config, use it to enable Application Insights.
        string appInsightsKey = context.Configuration["APPINSIGHTS_INSTRUMENTATIONKEY"];
        if (!string.IsNullOrEmpty(appInsightsKey))
        {
            // This uses the options callback to explicitly set the instrumentation key.
            b.AddApplicationInsights(o => o.InstrumentationKey = appInsightsKey);
        }
    });
    builder.ConfigureServices(services =>
        {
            services.AddSingleton<ITelemetryInitializer, CustomTelemetryInitializer>();
        });
    var host = builder.Build();
    using (host)
    {
        await host.RunAsync();
    }
}
```

[`TelemetryConfiguration`] Oluşturulduğunda, tüm kayıtlı türleri [`ITelemetryInitializer`] dahildir. Daha fazla bilgi edinmek [için özel olaylar ve ölçümler için Uygulama Öngörüleri API'sine](../azure-monitor/app/api-custom-events-metrics.md)bakın.

Sürüm 3'te. *x*, artık ana bilgisayar [`TelemetryClient`] durduğunda sifonu çekmeniz gerekmez. .NET Core bağımlılık enjeksiyon sistemi otomatik olarak `ApplicationInsightsLoggerProvider`kayıtlı bertaraf , [`TelemetryClient`]hangi floşlar .

#### <a name="version-2x"></a>Sürüm 2. *x*

Sürüm 2'de. *x,* [`TelemetryClient`] WebJobs SDK için Application Insights sağlayıcısı tarafından [`ServerTelemetryChannel`](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/ServerTelemetryChannel/ServerTelemetryChannel.cs)dahili olarak oluşturulan kullanır. Application Insights bitiş noktası kullanılamadığında veya gelen istekleri azalttığında, bu kanal [istekleri web uygulamasının dosya sisteminde kaydeder ve daha sonra yeniden gönderir.](https://apmtips.com/blog/2015/09/03/more-telemetry-channels)

Uygular [`TelemetryClient`] bir sınıf tarafından `ITelemetryClientFactory`oluşturulur. Varsayılan olarak, [`DefaultTelemetryClientFactory`](https://github.com/Azure/azure-webjobs-sdk/blob/dev/src/Microsoft.Azure.WebJobs.Logging.ApplicationInsights/DefaultTelemetryClientFactory.cs)bu.

Application Insights ardışık hattının herhangi bir bölümünü değiştirmek istiyorsanız, kendi `ITelemetryClientFactory`'nizi sağlayabilirsiniz ve [`TelemetryClient`]ana bilgisayar sınıfınızı bir . Örneğin, bu kod `DefaultTelemetryClientFactory` bir özelliği değiştirmek `ServerTelemetryChannel`için geçersiz kılar:

```csharp
private class CustomTelemetryClientFactory : DefaultTelemetryClientFactory
{
    public CustomTelemetryClientFactory(string instrumentationKey, Func<string, LogLevel, bool> filter)
        : base(instrumentationKey, new SamplingPercentageEstimatorSettings(), filter)
    {
    }

    protected override ITelemetryChannel CreateTelemetryChannel()
    {
        ServerTelemetryChannel channel = new ServerTelemetryChannel();

        // Change the default from 30 seconds to 15 seconds.
        channel.MaxTelemetryBufferDelay = TimeSpan.FromSeconds(15);

        return channel;
    }
}
```

Nesne `SamplingPercentageEstimatorSettings` [uyarlanabilir örneklemeyi](https://docs.microsoft.com/azure/application-insights/app-insights-sampling)yapılandırır. Bu, bazı yüksek hacimli senaryolarda Applications Insights'ın seçili bir telemetri veri kümesini sunucuya gönderdiği anlamına gelir.

Telemetri fabrikasını oluşturduktan sonra, uygulama istatistiklerini Uygulama Öngörüleri günlüğe kaydetme sağlayıcısına aktarın:

```csharp
var clientFactory = new CustomTelemetryClientFactory(instrumentationKey, filter.Filter);

config.LoggerFactory = new LoggerFactory()
    .AddApplicationInsights(clientFactory);
```

## <a name="next-steps"></a><a id="nextsteps"></a>Sonraki adımlar

Bu makalede, Webİşler SDK ile çalışmak için ortak senaryoların nasıl işleyeceğini gösteren kod parçacıkları sağlanmıştır. Tam örnekler için [azure-webjobs-sdk-örneklerine](https://github.com/Azure/azure-webjobs-sdk/tree/dev/sample/SampleHost)bakın.

['Yürütme Bağlamı']: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions/Extensions/Core/ExecutionContext.cs
['TelemetriMüşteri']: /dotnet/api/microsoft.applicationinsights.telemetryclient
['Yapılandırılan Hizmetler']: /dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices
['ITelemetryInitializer']: /dotnet/api/microsoft.applicationinsights.extensibility.itelemetryinitializer
['TelemetriYapılandırma']: /dotnet/api/microsoft.applicationinsights.extensibility.telemetryconfiguration
['JobHostConfiguration']: https://github.com/Azure/azure-webjobs-sdk/blob/v2.x/src/Microsoft.Azure.WebJobs.Host/JobHostConfiguration.cs
