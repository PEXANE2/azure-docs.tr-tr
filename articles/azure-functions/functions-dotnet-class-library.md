---
title: Azure İşlevler C# geliştirici başvurusu
description: C# kullanarak Azure İşlevlerini nasıl geliştireceklerini öğrenin.
ms.topic: reference
ms.date: 09/12/2018
ms.openlocfilehash: cfa53fe2defca768196af595c1d088d41bc60f71
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277069"
---
# <a name="azure-functions-c-developer-reference"></a>Azure İşlevler C# geliştirici başvurusu

<!-- When updating this article, make corresponding changes to any duplicate content in functions-reference-csharp.md -->

Bu makale, .NET sınıf kitaplıklarında C# kullanarak Azure İşlevlerini geliştirmeye giriştir.

Azure İşlevleri C# ve C# komut dosyası programlama dillerini destekler. [Azure portalında C# kullanma](functions-create-function-app-portal.md)yla ilgili bir kılavuz arıyorsanız, [C# komut dosyası (.csx) geliştirici başvurusuna](functions-reference-csharp.md)bakın.

Bu makalede, aşağıdaki makaleleri zaten okuduğunuz varsayar:

* [Azure Fonksiyonları geliştiricileri kılavuzu](functions-reference.md)
* [Azure Fonksiyonları Visual Studio 2019 Araçları](functions-develop-vs.md)

## <a name="supported-versions"></a>Desteklenen sürümler

İşlevler çalışma zamanı sürümleri .NET'in belirli sürümleriyle çalışır. Aşağıdaki tablo, projenizdeki Işlevlerin belirli bir sürümüyle kullanılabilecek en yüksek .NET Core ve .NET Framework ve .NET Core düzeyini gösterir. 

| İşlevler çalışma zamanı sürümü | Max .NET sürümü |
| ---- | ---- |
| Fonksiyonlar 3.x | .NET Çekirdek 3.1 |
| İşlevler 2.x | .NET Core 2.2 |
| İşlevler 1.x | .NET Framework 4.6 |

Daha fazla bilgi için [Azure İşlevleri çalışma zamanı sürümlerine genel bakış](functions-versions.md)

## <a name="functions-class-library-project"></a>Fonksiyonlar sınıf kitaplık projesi

Visual Studio'da **Azure İşlevleri** proje şablonu, aşağıdaki dosyaları içeren bir C# sınıfı kitaplık projesi oluşturur:

* [host.json](functions-host-json.md) - yerel olarak veya Azure'da çalışırken projedeki tüm işlevleri etkileyen yapılandırma ayarlarını depolar.
* [local.settings.json](functions-run-local.md#local-settings-file) - yerel olarak çalışırken kullanılan uygulama ayarlarını ve bağlantı dizelerini saklar. Bu dosya sırlar içerir ve Azure'daki işlev uygulamanızda yayınlanmaz. Bunun yerine, [işlev uygulamanıza uygulama ayarları ekleyin.](functions-develop-vs.md#function-app-settings)

Projeyi oluşturduğunuzda, yapı çıktıdizinde aşağıdaki örneğe benzeyen bir klasör yapısı oluşturulur:

```
<framework.version>
 | - bin
 | - MyFirstFunction
 | | - function.json
 | - MySecondFunction
 | | - function.json
 | - host.json
```

Bu dizin, Azure'daki işlev uygulamanıza dağıtılan dizindir. İşlevler çalışma [süresinin 2.x sürümünde](functions-versions.md) gerekli bağlama uzantıları [projeye NuGet paketleri olarak eklenir.](./functions-bindings-register.md#vs)

> [!IMPORTANT]
> Yapı işlemi, her işlev için bir *işlev.json* dosyası oluşturur. Bu *function.json* dosyası doğrudan düzenlenmiyor. Bu dosyayı düzenleyerek bağlama yapılandırmasını değiştiremezsiniz veya işlevi devre dışı kalamazsınız. Bir işlevi nasıl devre dışı dışı kgöreceğizi öğrenmek [için işlevleri nasıl devre dışı dışı süreceğinizi](disable-function.md)öğrenin.


## <a name="methods-recognized-as-functions"></a>Fonksiyon olarak tanınan yöntemler

Sınıf kitaplığında, işlev aşağıdaki örnekte `FunctionName` gösterildiği gibi a ve tetikleyici özniteliği olan statik bir yöntemdir:

```csharp
public static class SimpleExample
{
    [FunctionName("QueueTrigger")]
    public static void Run(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        ILogger log)
    {
        log.LogInformation($"C# function processed: {myQueueItem}");
    }
} 
```

Öznitelik, `FunctionName` yöntemi işlev giriş noktası olarak işaretler. Ad bir proje içinde benzersiz olmalı, bir harfle başlamalı `_`ve `-`yalnızca harfler, sayılar ve 127 karaktere kadar uzunlukta olmalıdır. Proje şablonları genellikle adlı `Run`bir yöntem oluşturur, ancak yöntem adı geçerli bir C# yöntemi adı olabilir.

Tetikleyici özniteliği tetikleyici türünü belirtir ve giriş verilerini bir yöntem parametresine bağlar. Örnek işlev bir sıra iletisi tarafından tetiklenir ve sıra iletisi `myQueueItem` parametredeki yönteme aktarılır.

## <a name="method-signature-parameters"></a>Yöntem imza parametreleri

Yöntem imzası, tetikleyici özniteliği ile kullanılan parametre ler dışında parametreler içerebilir. Şunları ekleyebiliriz ek parametrelerden bazıları şunlardır:

* Öznitelikleri ile süsleyerek işaretlenmiş [giriş ve çıkış ciltleri.](functions-triggers-bindings.md)  
* Günlük `ILogger` `TraceWriter` için bir veya ([sürüm 1.x-only)](functions-versions.md#creating-1x-apps)parametresi [.](#logging)
* Zarif `CancellationToken` [kapatma](#cancellation-tokens)için bir parametre.
* Tetikleyici meta verileri almak için ifadeler parametrelerini [bağlama.](./functions-bindings-expressions-patterns.md)

İşlev imzasındaki parametrelerin sırası önemli değildir. Örneğin, tetikleyici parametrelerini diğer bağlamalardan önce veya sonra koyabilirsiniz ve logger parametresini tetikleyici veya bağlama parametrelerinden önce veya sonra koyabilirsiniz.

### <a name="output-binding-example"></a>Çıktı bağlama örneği

Aşağıdaki örnek, bir çıktı sırası bağlama ekleyerek öncekini değiştirir. İşlev, işlevi farklı bir sırada yeni bir sıra iletisine tetikleyen sıra iletisini yazar.

```csharp
public static class SimpleExampleWithOutput
{
    [FunctionName("CopyQueueMessage")]
    public static void Run(
        [QueueTrigger("myqueue-items-source")] string myQueueItem, 
        [Queue("myqueue-items-destination")] out string myQueueItemCopy,
        ILogger log)
    {
        log.LogInformation($"CopyQueueMessage function processed: {myQueueItem}");
        myQueueItemCopy = myQueueItem;
    }
}
```

Bağlayıcı başvuru[makaleleri (Depolama kuyrukları](functions-bindings-storage-queue.md), örneğin), tetikleyici, giriş veya çıktı bağlama öznitelikleriyle hangi parametre türlerini kullanabileceğinizi açıklar.

### <a name="binding-expressions-example"></a>İfadeleri bağlama örneği

Aşağıdaki kod, bir uygulama ayarından izlenecek sıranın adını alır ve `insertionTime` parametredeki sıra iletisi oluşturma süresini alır.

```csharp
public static class BindingExpressionsExample
{
    [FunctionName("LogQueueMessage")]
    public static void Run(
        [QueueTrigger("%queueappsetting%")] string myQueueItem,
        DateTimeOffset insertionTime,
        ILogger log)
    {
        log.LogInformation($"Message content: {myQueueItem}");
        log.LogInformation($"Created at: {insertionTime}");
    }
}
```

## <a name="autogenerated-functionjson"></a>Otomatik oluşturulan fonksiyon.json

Yapı işlemi, yapı klasöründeki bir işlev klasöründe bir *işlev.json* dosyası oluşturur. Daha önce belirtildiği gibi, bu dosyadoğrudan düzenlenmis değildir. Bu dosyayı düzenleyerek bağlama yapılandırmasını değiştiremezsiniz veya işlevi devre dışı kalamazsınız. 

Bu dosyanın amacı, Tüketim planı ndaki kararları [ölçeklendirmek](functions-scale.md#how-the-consumption-and-premium-plans-work)için kullanılacak ölçek denetleyicisine bilgi sağlamaktır. Bu nedenle, dosya yalnızca giriş veya çıkış bağlamaları değil, tetikleyici bilgileri vardır.

Oluşturulan *function.json* dosyası, `configurationSource` çalışma zamanını *function.json* yapılandırması yerine .NET özniteliklerini bağlamaiçin kullanmalarını söyleyen bir özellik içerir. Bir örneği aşağıda verilmiştir:

```json
{
  "generatedBy": "Microsoft.NET.Sdk.Functions-1.0.0.0",
  "configurationSource": "attributes",
  "bindings": [
    {
      "type": "queueTrigger",
      "queueName": "%input-queue-name%",
      "name": "myQueueItem"
    }
  ],
  "disabled": false,
  "scriptFile": "..\\bin\\FunctionApp1.dll",
  "entryPoint": "FunctionApp1.QueueTrigger.Run"
}
```

## <a name="microsoftnetsdkfunctions"></a>Microsoft.NET.Sdk.Fonksiyonlar

*function.json* dosya oluşturma NuGet paketi [Microsoft\.\.NET\.Sdk Fonksiyonları](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions)tarafından gerçekleştirilir. 

Aynı paket, Fonksiyonlar çalışma zamanının hem 1.x hem de 2.x sürümü için kullanılır. Hedef çerçeve, 1.x projeyi 2.x projeden ayıran şeydir. Farklı hedef çerçeveleri ve aynı `Sdk` paketi gösteren *.csproj* dosyalarının ilgili bölümleri şunlardır:

**İşlevler 1.x**

```xml
<PropertyGroup>
  <TargetFramework>net461</TargetFramework>
</PropertyGroup>
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Sdk.Functions" Version="1.0.8" />
</ItemGroup>
```

**İşlevler 2.x**

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.1</TargetFramework>
  <AzureFunctionsVersion>v2</AzureFunctionsVersion>
</PropertyGroup>
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Sdk.Functions" Version="1.0.8" />
</ItemGroup>
```

`Sdk` Paket bağımlılıkları arasında tetikleyiciler ve bağlamalar vardır. 1.x projesi 1.x tetikleyicileri ve bağlamaları ifade eder, çünkü bu tetikleyiciler ve bağlamalar .NET Framework'u hedef alırken, 2.x tetikler ve bağlamalar hedef .NET Core'u hedefeder.

Paket `Sdk` aynı zamanda [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json)bağlıdır , ve dolaylı [olarak WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage). Bu bağımlılıklar, projenizin, projenin hedeflenedığı Işlevler çalışma zamanı sürümüyle çalışan bu paketlerin sürümlerini kullandığından emin olun. Örneğin, `Newtonsoft.Json` .NET Framework 4.6.1 için sürüm 11 vardır, ancak .NET Framework 4.6.1'i hedefleyen Işlevler çalışma süresi yalnızca 9.0.1 ile `Newtonsoft.Json` uyumludur. Yani bu projedeki işlev kodunuz `Newtonsoft.Json` da 9.0.1 kullanmak zorundadır.

Kaynak kodu `Microsoft.NET.Sdk.Functions` GitHub repo [azure\-fonksiyonları\-vs\-\-yapı sdk](https://github.com/Azure/azure-functions-vs-build-sdk)mevcuttur.

## <a name="runtime-version"></a>Çalışma zamanı sürümü

Visual Studio, İşlev projelerini çalıştırmak için [Azure İşlevler Temel Araçlarını](functions-run-local.md#install-the-azure-functions-core-tools) kullanır. Çekirdek Araçları, İşlevler için çalışma zamanı için bir komut satırı arabirimidir.

Core Tools'u npm kullanarak yüklerseniz, bu Visual Studio tarafından kullanılan Core Tools sürümünü etkilemez. Visual Studio, Çalışma Zamanı sürüm 1.x için Core Tools sürümlerini *%USERPROFILE%\AppData\Local\Azure.Functions.Cli* adresinde saklar ve burada depolanan en son sürümü kullanır. Fonksiyonlar 2.x için Temel Araçlar **Azure İşve İş Araçları** uzantısına dahildir. Hem 1.x hem de 2.x için, bir Fonksiyonlar projesi çalıştırdığınızda konsol çıkışında hangi sürümün kullanıldığını görebilirsiniz:

```terminal
[3/1/2018 9:59:53 AM] Starting Host (HostId=contoso2-1518597420, Version=2.0.11353.0, ProcessId=22020, Debug=False, Attempt=0, FunctionsExtensionVersion=)
```

## <a name="supported-types-for-bindings"></a>Bağlamalar için desteklenen türler

Her bağlayıcının kendi desteklenen türleri vardır; örneğin, bir blob tetikleme özniteliği bir dize parametresi, bir `CloudBlockBlob` POCO parametresi, bir parametre veya diğer birkaç desteklenen türlerden herhangi biri uygulanabilir. [Blob bağlamaları için bağlayıcı başvuru makalesi](functions-bindings-storage-blob-trigger.md#usage) desteklenen tüm parametre türlerini listeler. Daha fazla bilgi için, [tetikleyiciler ve bağlamalar ve](functions-triggers-bindings.md) [her bağlayıcı türü için bağlayıcı başvuru dokümanları](functions-triggers-bindings.md#next-steps)bakın.

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="binding-to-method-return-value"></a>Yöntem döndürme değerine bağlama

Yöntem iade değeriözasyon uygulayarak, çıktı bağlama için bir yöntem iade değeri kullanabilirsiniz. Örnekler için, [bkz. Tetikleyiciler ve bağlamalar.](./functions-bindings-return-value.md) 

İade değerini yalnızca başarılı bir işlev yürütmesi her zaman çıktı bağlamasına geçmek üzere bir iade değeriyle sonuçlanırsa kullanın. Aksi takdirde, aşağıdaki bölümde gösterildiği gibi kullanın `ICollector` veya, `IAsyncCollector`

## <a name="writing-multiple-output-values"></a>Birden çok çıktı değeri yazma

Çıktı bağlamasına birden çok değer yazmak veya başarılı bir işlev çağırması çıktı bağlamaya geçmek [`ICollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) [`IAsyncCollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) için bir şey neden olmayabilir, kullanın veya türleri. Bu türler, yöntem tamamlandığında çıktı bağlamasına yalnızca yazma koleksiyonlarıdır.

Bu örnek, aynı kuyruğa `ICollector`birden çok sıra iletisi yazar:

```csharp
public static class ICollectorExample
{
    [FunctionName("CopyQueueMessageICollector")]
    public static void Run(
        [QueueTrigger("myqueue-items-source-3")] string myQueueItem,
        [Queue("myqueue-items-destination")] ICollector<string> myDestinationQueue,
        ILogger log)
    {
        log.LogInformation($"C# function processed: {myQueueItem}");
        myDestinationQueue.Add($"Copy 1: {myQueueItem}");
        myDestinationQueue.Add($"Copy 2: {myQueueItem}");
    }
}
```

## <a name="logging"></a>Günlüğe Kaydetme

C#'daki akış günlüklerinize çıktınızı günlüğe kaydetmek [için, ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger)türünden bir bağımsız değişken ekleyin. Aşağıdaki örnekte olduğu `log`gibi, bunu adlandırmanızı öneririz:  

```csharp
public static class SimpleExample
{
    [FunctionName("QueueTrigger")]
    public static void Run(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        ILogger log)
    {
        log.LogInformation($"C# function processed: {myQueueItem}");
    }
} 
```

Azure `Console.Write` İşlevler'de kullanmaktan kaçının. Daha fazla bilgi için [bkz.](functions-monitoring.md#write-logs-in-c-functions) **Monitor Azure Functions**

## <a name="async"></a>Zaman Uyumsuz

Bir işlevin [eşzamanlı](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/)olmasını sağlamak `async` için, anahtar `Task` kelimeyi kullanın ve bir nesneyi döndürün.

```csharp
public static class AsyncExample
{
    [FunctionName("BlobCopy")]
    public static async Task RunAsync(
        [BlobTrigger("sample-images/{blobName}")] Stream blobInput,
        [Blob("sample-images-copies/{blobName}", FileAccess.Write)] Stream blobOutput,
        CancellationToken token,
        ILogger log)
    {
        log.LogInformation($"BlobCopy function processed.");
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
}
```

Parametreleri async `out` işlevlerinde kullanamazsınız. Çıktı bağlamaları için [işlev döndürme değerini](#binding-to-method-return-value) veya [bir toplayıcı nesnesini](#writing-multiple-output-values) kullanın.

## <a name="cancellation-tokens"></a>İptal belirteçleri

Bir işlev, işlev sonlandırılmak üzereyken işletim sisteminin kodunuzu bildirmesini sağlayan [İptalToken](/dotnet/api/system.threading.cancellationtoken) parametresini kabul edebilir. Bu bildirimi, işlevin beklenmeyen bir şekilde sonlandırmadığından emin olmak için verileri tutarsız bir durumda bırakabilecek şekilde kullanabilirsiniz.

Aşağıdaki örnek, yaklaşan işlev sonlandırma için nasıl denetleyikarşılanınacak larını gösterir.

```csharp
public static class CancellationTokenExample
{
    public static void Run(
        [QueueTrigger("inputqueue")] string inputText,
        TextWriter logger,
        CancellationToken token)
    {
        for (int i = 0; i < 100; i++)
        {
            if (token.IsCancellationRequested)
            {
                logger.WriteLine("Function was cancelled at iteration {0}", i);
                break;
            }
            Thread.Sleep(5000);
            logger.WriteLine("Normal processing for queue message={0}", inputText);
        }
    }
}
```

## <a name="environment-variables"></a>Ortam değişkenleri

Bir ortam değişkeni veya uygulama ayar `System.Environment.GetEnvironmentVariable`değeri elde etmek için aşağıdaki kod örneğinde gösterildiği gibi kullanın:

```csharp
public static class EnvironmentVariablesExample
{
    [FunctionName("GetEnvironmentVariables")]
    public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, ILogger log)
    {
        log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
        log.LogInformation(GetEnvironmentVariable("AzureWebJobsStorage"));
        log.LogInformation(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
    }

    public static string GetEnvironmentVariable(string name)
    {
        return name + ": " +
            System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
    }
}
```

Uygulama ayarları, hem yerel olarak gelişirken hem de Azure'da çalışırken ortam değişkenlerinden okunabilir. Yerel olarak geliştirirken, uygulama `Values` ayarları *local.settings.json* dosyasındaki koleksiyondan gelir. Yerel ve Azure olmak `GetEnvironmentVariable("<app setting name>")` üzere her iki ortamda da adlandırılmış uygulama ayarının değerini alır. Örneğin, yerel olarak çalışırken, *local.settings.json* dosyanız içeriyorsa `{ "Values": { "WEBSITE_SITE_NAME": "My Site Name" } }`"Site Adım" döndürülür.

[System.Configuration.ConfigurationManager.AppSettings](https://docs.microsoft.com/dotnet/api/system.configuration.configurationmanager.appsettings) özelliği uygulama ayar değerlerini almak için alternatif bir API'dir, ancak burada gösterildiği gibi kullanmanızı `GetEnvironmentVariable` öneririz.

## <a name="binding-at-runtime"></a>Çalışma zamanında bağlama

C# ve diğer .NET dillerinde, özniteliklerdeki [*bildirimsel*](https://en.wikipedia.org/wiki/Declarative_programming) bağlamaların aksine [zorunlu](https://en.wikipedia.org/wiki/Imperative_programming) bir bağlama deseni kullanabilirsiniz. Zorunlu bağlama, bağlama parametrelerinin tasarım zamanı yerine çalışma zamanında hesaplanması gerektiğinde yararlıdır. Bu desenle, işlev kodunuzda desteklenen giriş ve çıkış bağlamalarını anında bağlayabilirsiniz.

Zorunlu bir bağlamayı aşağıdaki gibi tanımlayın:

- **Do not** İstediğiniz zorunlu bağlamalar için işlev imzasında bir öznitelik eklemeyin.
- Giriş parametresi [`Binder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) veya [`IBinder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).
- Veri bağlamagerçekleştirmek için aşağıdaki C# deseni kullanın.

  ```cs
  using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
  {
      ...
  }
  ```

  `BindingTypeAttribute`bağlamanızı tanımlayan .NET özniteliğidir ve `T` bu bağlama türü tarafından desteklenen bir giriş veya çıkış türüdür. `T`bir `out` parametre türü olamaz `out JObject`(gibi). Örneğin, Mobil Uygulamalar tablo çıktı bağlama [altı çıkış türünü](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22)destekler, ancak yalnızca zorunlu bağlama ile [ICollector\<T>](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) veya [IAsyncCollector\<T>](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) kullanabilirsiniz.

### <a name="single-attribute-example"></a>Tek öznitelik örneği

Aşağıdaki örnek kod, çalışma zamanında tanımlanan blob yolu ile bağlayıcı bir [Depolama blob çıkışı](functions-bindings-storage-blob-output.md) oluşturur, sonra blob bir dize yazar.

```cs
public static class IBinderExample
{
    [FunctionName("CreateBlobUsingBinder")]
    public static void Run(
        [QueueTrigger("myqueue-items-source-4")] string myQueueItem,
        IBinder binder,
        ILogger log)
    {
        log.LogInformation($"CreateBlobUsingBinder function processed: {myQueueItem}");
        using (var writer = binder.Bind<TextWriter>(new BlobAttribute(
                    $"samples-output/{myQueueItem}", FileAccess.Write)))
        {
            writer.Write("Hello World!");
        };
    }
}
```

[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs/BlobAttribute.cs) Depolama [blob](functions-bindings-storage-blob.md) giriş veya çıkış bağlama tanımlar ve [TextWriter](/dotnet/api/system.io.textwriter) desteklenen bir çıkış bağlama türüdür.

### <a name="multiple-attribute-example"></a>Birden çok öznitelik örneği

Önceki örnek, işlev uygulamasının ana Depolama hesabı bağlantı dizesi `AzureWebJobsStorage`için uygulama ayarını alır (ki bu da). [StorageAccountAttribute'ı](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) ekleyerek ve öznitelik dizisini ' ye `BindAsync<T>()`geçirerek Depolama hesabı için kullanılacak özel bir uygulama ayarını belirtebilirsiniz. Parametre `Binder` kullanın, `IBinder`değil.  Örnek:

```cs
public static class IBinderExampleMultipleAttributes
{
    [FunctionName("CreateBlobInDifferentStorageAccount")]
    public async static Task RunAsync(
            [QueueTrigger("myqueue-items-source-binder2")] string myQueueItem,
            Binder binder,
            ILogger log)
    {
        log.LogInformation($"CreateBlobInDifferentStorageAccount function processed: {myQueueItem}");
        var attributes = new Attribute[]
        {
        new BlobAttribute($"samples-output/{myQueueItem}", FileAccess.Write),
        new StorageAccountAttribute("MyStorageAccount")
        };
        using (var writer = await binder.BindAsync<TextWriter>(attributes))
        {
            await writer.WriteAsync("Hello World!!");
        }
    }
}
```

## <a name="triggers-and-bindings"></a>Tetikleyiciler ve bağlamalar 

[!INCLUDE [Supported triggers and bindings](../../includes/functions-bindings.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tetikleyiciler ve bağlamalar hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Azure İşlevleri için en iyi uygulamalar hakkında daha fazla bilgi edinin](functions-best-practices.md)
