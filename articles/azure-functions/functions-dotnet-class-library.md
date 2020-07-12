---
title: Azure Işlevleri C# Geliştirici Başvurusu
description: C# kullanarak Azure Işlevleri geliştirmeyi anlayın.
ms.topic: conceptual
ms.date: 09/12/2018
ms.openlocfilehash: 038c1db2d4bb4d8bd80801d36cf5feec1905bbc1
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86254376"
---
# <a name="azure-functions-c-developer-reference"></a>Azure Işlevleri C# Geliştirici Başvurusu

<!-- When updating this article, make corresponding changes to any duplicate content in functions-reference-csharp.md -->

Bu makale, .NET sınıf kitaplıklarında C# kullanarak Azure Işlevleri geliştirmeye giriş niteliğindedir.

Azure Işlevleri C# ve C# betiği programlama dillerini destekler. [Azure Portal c# kullanma](functions-create-function-app-portal.md)hakkında rehberlik arıyorsanız bkz. [c# betiği (. CSX) geliştirici başvurusu](functions-reference-csharp.md).

Bu makalede, aşağıdaki makaleleri zaten okuduğunuzu varsaymış olursunuz:

* [Azure Işlevleri Geliştirici Kılavuzu](functions-reference.md)
* [Azure Işlevleri Visual Studio 2019 araçları](functions-develop-vs.md)

## <a name="supported-versions"></a>Desteklenen sürümler

Işlevler çalışma zamanının sürümleri .NET 'in belirli sürümleriyle çalışır. Aşağıdaki tabloda, projenizdeki belirli bir Işlev sürümü ile kullanılabilecek en yüksek .NET Core ve .NET Framework ve .NET Core düzeyi gösterilmektedir. 

| İşlevler çalışma zamanı sürümü | En yüksek .NET sürümü |
| ---- | ---- |
| İşlevler 3. x | .NET Core 3,1 |
| İşlevler 2.x | .NET Core 2.2 |
| İşlevler 1.x | .NET Framework 4.6 |

Daha fazla bilgi için bkz. [Azure işlevleri çalışma zamanı sürümlerine genel bakış](functions-versions.md)

## <a name="functions-class-library-project"></a>İşlevler sınıf kitaplığı projesi

Visual Studio 'da **Azure işlevleri** proje şablonu, aşağıdaki dosyaları Içeren bir C# sınıf kitaplığı projesi oluşturur:

* [host.js](functions-host-json.md) , yerel olarak veya Azure 'da çalışırken projedeki tüm işlevleri etkileyen yapılandırma ayarlarını depolar.
* [local.settings.js](functions-run-local.md#local-settings-file) , yerel olarak çalışırken kullanılan uygulama ayarlarını ve bağlantı dizelerini depolar. Bu dosya gizli dizileri içerir ve Azure 'da işlev uygulamanıza yayımlanmaz. Bunun yerine, [uygulama ayarlarını işlev uygulamanıza ekleyin](functions-develop-vs.md#function-app-settings).

Projeyi derlediğinizde, derleme çıkış dizininde aşağıdaki örnek gibi görünen bir klasör yapısı oluşturulur:

```
<framework.version>
 | - bin
 | - MyFirstFunction
 | | - function.json
 | - MySecondFunction
 | | - function.json
 | - host.json
```

Bu dizin, Azure 'daki işlev uygulamanıza dağıtılır. Işlevler çalışma zamanının [2. x sürümünde](functions-versions.md) gerekli olan bağlama uzantıları [projeye NuGet paketleri olarak eklenir](./functions-bindings-register.md#vs).

> [!IMPORTANT]
> Yapı işlemi her bir işlev için dosyada bir *function.js* oluşturur. Dosyadaki bu *function.js* doğrudan düzenlenmeyecek. Bu dosyayı düzenleyerek bağlama yapılandırmasını değiştiremez veya işlevi devre dışı bırakabilirsiniz. Bir işlevi nasıl devre dışı bırakacağınızı öğrenmek için bkz. [işlevleri devre dışı bırakma](disable-function.md).


## <a name="methods-recognized-as-functions"></a>İşlev olarak tanınan Yöntemler

Bir sınıf kitaplığında, bir işlev, `FunctionName` Aşağıdaki örnekte gösterildiği gibi, ve bir tetikleyici özniteliği olan statik bir yöntemdir:

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

`FunctionName`Özniteliği yöntemi bir işlev giriş noktası olarak işaretler. Ad bir proje içinde benzersiz olmalıdır, bir harfle başlamalı ve yalnızca harf, sayı, `_` ve `-` , en fazla 127 karakter içermelidir. Proje şablonları genellikle adlı bir yöntem oluşturur `Run` , ancak yöntem adı geçerli bir C# Yöntem adı olabilir.

Tetikleyici özniteliği Tetikleyici türünü belirtir ve giriş verilerini bir yöntem parametresine bağlar. Örnek işlev bir kuyruk iletisi tarafından tetiklenir ve kuyruk iletisi, parametresindeki yöntemine geçirilir `myQueueItem` .

## <a name="method-signature-parameters"></a>Yöntem imza parametreleri

Yöntem imzası, tetikleyici özniteliğiyle kullanılandan farklı parametreler içeriyor olabilir. Şunları dahil edebilirsiniz:

* [Giriş ve çıkış bağlamaları](functions-triggers-bindings.md) öznitelikleri ile süsleyerek bu şekilde işaretlenir.  
* `ILogger` `TraceWriter` [Günlüğe kaydetme](#logging)için bir veya ([Sürüm 1. x-yalnızca sürüm](functions-versions.md#creating-1x-apps)) parametresi.
* `CancellationToken` [Düzgün kapanma](#cancellation-tokens)için bir parametre.
* Tetikleyici meta verilerini almak için [ifade parametreleri bağlama](./functions-bindings-expressions-patterns.md) .

İşlev imzasında parametrelerin sırası önemi yoktur. Örneğin, tetikleme parametrelerini diğer bağlamalardan önce veya sonra koyabilirsiniz ve tetikleyici veya bağlama parametrelerinden önce veya sonra günlükçü parametresini yerleştirebilirsiniz.

### <a name="output-binding-example"></a>Çıkış bağlama örneği

Aşağıdaki örnek, bir çıkış sırası bağlaması ekleyerek önceki birini değiştirir. İşlevi, işlevi tetikleyen sıra iletisini farklı bir kuyruktaki yeni bir kuyruk iletisine yazar.

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

Bağlama başvuru makaleleri (örneğin,[depolama kuyrukları](functions-bindings-storage-queue.md)) tetikleyici, giriş veya çıkış bağlama öznitelikleri ile hangi parametre türlerini kullanabileceğinizi açıklar.

### <a name="binding-expressions-example"></a>Bağlama ifadeleri örneği

Aşağıdaki kod, bir uygulama ayarından izlenecek sıranın adını alır ve parametresindeki sıra iletisi oluşturma zamanını alır `insertionTime` .

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

## <a name="autogenerated-functionjson"></a>function.jsotomatik olarak oluşturulur

Yapı işlemi, derleme klasöründeki bir işlev klasöründe bir *function.js* dosya oluşturur. Daha önce belirtildiği gibi, bu dosyanın doğrudan düzenlenmesi amaçlıyordu. Bu dosyayı düzenleyerek bağlama yapılandırmasını değiştiremez veya işlevi devre dışı bırakabilirsiniz. 

Bu dosyanın amacı, [Tüketim planındaki kararları ölçeklendirirken](functions-scale.md#how-the-consumption-and-premium-plans-work)kullanılacak ölçek denetleyicisine bilgi sağlamaktır. Bu nedenle, dosya yalnızca tetikleyici bilgisine sahiptir, giriş veya çıkış bağlamaları değildir.

Dosyada oluşturulan *function.js* , `configurationSource` çalışma zamanına yapılandırma *function.js* yerine bağlama için .net öznitelikleri kullanmasını söyleyen bir özelliği içerir. Aşağıda bir örnek verilmiştir:

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

## <a name="microsoftnetsdkfunctions"></a>Microsoft. NET. SDK. Functions

Dosya oluşturma *function.js* , NuGet paketi [Microsoft \. net \. SDK \. işlevleri](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions)tarafından gerçekleştirilir. 

Aynı paket, Işlevler çalışma zamanının hem sürüm 1. x hem de 2. x için kullanılır. Hedef Framework, bir 2. x projesinden 1. x projesini farklılaştırır. Farklı hedef çerçeveleri ve aynı paketi gösteren *. csproj* dosyalarının ilgili bölümleri aşağıda verilmiştir `Sdk` :

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

`Sdk`Paket bağımlılıkları arasında Tetikleyiciler ve bağlamalar vardır. 1. x Projesi 1. x tetikleyicilerine ve bağlamalara başvurur çünkü bu Tetikleyiciler ve bağlamalar .NET Framework, ancak 2. x Tetikleyicileri ve bağlamaları hedef .NET Core ' u hedefler.

`Sdk`Paket Ayrıca, [windowsazure. Storage](https://www.nuget.org/packages/WindowsAzure.Storage)üzerinde [Newtonsoft.Js](https://www.nuget.org/packages/Newtonsoft.Json)ve dolaylı olarak değişir. Bu bağımlılıklar, projenizin, projenin hedeflediği Işlevler çalışma zamanı sürümüyle çalışan paketlerin sürümlerini kullandığından emin olun. Örneğin, `Newtonsoft.Json` .NET Framework 4.6.1 için sürüm 11 ' e sahiptir, ancak .NET Framework 4.6.1 hedefleyen işlevler çalışma zamanı yalnızca 9.0.1 ile uyumludur `Newtonsoft.Json` . Bu nedenle, bu projedeki işlev kodunuzun de 9.0.1 kullanması gerekir `Newtonsoft.Json` .

İçin kaynak kodu, `Microsoft.NET.Sdk.Functions` GitHub deposu [Azure \- işlevleri \- vs \- derlemesi \- SDK 'sında](https://github.com/Azure/azure-functions-vs-build-sdk)bulunabilir.

## <a name="runtime-version"></a>Çalışma zamanı sürümü

Visual Studio, Işlevler projelerini çalıştırmak için [Azure Functions Core Tools](functions-run-local.md#install-the-azure-functions-core-tools) kullanır. Temel araçlar, Işlevler çalışma zamanı için bir komut satırı arabirimidir.

Ana araçları NPM kullanarak yüklerseniz, Visual Studio tarafından kullanılan temel araçlar sürümünü etkilemez. Işlevler çalışma zamanı sürüm 1. x, Visual Studio *%USERPROFILE%\AppData\Local\Azure.Functions.cli* ' deki temel araçlar sürümlerini depolar ve burada depolanan en son sürümü kullanır. 2. x Işlevleri için, temel araçlar **Azure işlevleri ve Web Işleri araçları** uzantısına dahildir. Hem 1. x hem de 2. x için, bir Işlevler projesi çalıştırdığınızda konsol çıkışında hangi sürümün kullanıldığını görebilirsiniz:

```terminal
[3/1/2018 9:59:53 AM] Starting Host (HostId=contoso2-1518597420, Version=2.0.11353.0, ProcessId=22020, Debug=False, Attempt=0, FunctionsExtensionVersion=)
```

## <a name="supported-types-for-bindings"></a>Bağlamalar için desteklenen türler

Her bağlamanın kendi desteklenen türleri vardır; Örneğin, bir blob tetikleyici özniteliği bir dize parametresine, bir POCO parametresine, `CloudBlockBlob` parametreye veya desteklenen başka birkaç türden birine uygulanabilir. [BLOB bağlamaları için bağlama başvuru makalesi](functions-bindings-storage-blob-trigger.md#usage) , desteklenen tüm parametre türlerini listeler. Daha fazla bilgi için, bkz. [Tetikleyiciler ve bağlamalar](functions-triggers-bindings.md) ve [her bağlama türü için bağlama başvurusu belgeleri](functions-triggers-bindings.md#next-steps).

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="binding-to-method-return-value"></a>Metot dönüş değerine bağlama

Özniteliği yöntem dönüş değerine uygulayarak bir çıkış bağlaması için bir yöntem dönüş değeri kullanabilirsiniz. Örnekler için bkz. [Tetikleyiciler ve bağlamalar](./functions-bindings-return-value.md). 

Dönüş değerini yalnızca başarılı bir işlev yürütmesi her zaman çıkış bağlamasına geçirilecek bir dönüş değeri ile sonuçlanırsa kullanın. Aksi takdirde, `ICollector` `IAsyncCollector` aşağıdaki bölümde gösterildiği gibi, veya kullanın.

## <a name="writing-multiple-output-values"></a>Birden çok çıkış değeri yazma

Bir çıkış bağlamasına birden çok değer yazmak için veya başarılı bir işlev çağrılması çıkış bağlamasına geçirilecek herhangi bir şeye neden olmazsa, [`ICollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) veya [`IAsyncCollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) türlerini kullanın. Bu türler, yöntem tamamlandığında çıkış bağlamaya yazılan salt yazılır koleksiyonlardır.

Bu örnek, kullanarak birden çok kuyruk iletisini aynı kuyruğa yazar `ICollector` :

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

## <a name="logging"></a>Günlüğe kaydetme

Çıktıyı C# ' deki akış günlüklerinizi günlüğe kaydetmek için, [ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger)türünde bir bağımsız değişken ekleyin. `log`Aşağıdaki örnekte olduğu gibi, adını yazmanız önerilir:  

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

`Console.Write`Azure işlevleri 'nde kullanmaktan kaçının. Daha fazla bilgi için bkz. **Azure Işlevlerini izleme** makalesindeki [C# işlevlerinde yazma günlükleri](functions-monitoring.md#write-logs-in-c-functions) .

## <a name="async"></a>Zaman Uyumsuz

Bir işlevi [zaman uyumsuz](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/)yapmak için `async` anahtar sözcüğünü kullanın ve bir `Task` nesne döndürün.

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

`out`Zaman uyumsuz işlevlerde parametreleri kullanamazsınız. Çıktı bağlamaları için, bunun yerine [işlev dönüş değeri](#binding-to-method-return-value) veya [toplayıcı nesnesi](#writing-multiple-output-values) kullanın.

## <a name="cancellation-tokens"></a>İptal belirteçleri

Bir işlev bir [CancellationToken](/dotnet/api/system.threading.cancellationtoken) parametresini kabul edebilir ve bu, işlev sonlandırıldıktan sonra işletim sisteminin kodunuza bildirmesini sağlar. Bu bildirimi, işlevin verileri tutarsız bir durumda bırakmak için beklenmedik bir şekilde sonlandığından emin olmak için kullanabilirsiniz.

Aşağıdaki örnek, yaklaşan işlev sonlandırmasının nasıl kontrol alınacağını gösterir.

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

Bir ortam değişkeni veya bir uygulama ayarı değeri almak için, `System.Environment.GetEnvironmentVariable` Aşağıdaki kod örneğinde gösterildiği gibi kullanın:

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

Uygulama ayarları, hem yerel olarak hem de Azure 'da çalışırken ortam değişkenlerinden okunabilir. Yerel olarak geliştirme yaparken, uygulama ayarları `Values` dosyadaki *local.settings.js* koleksiyonundan gelir. Yerel ve Azure her iki ortamda da `GetEnvironmentVariable("<app setting name>")` adlandırılmış uygulama ayarının değerini alır. Örneğin, yerel olarak çalışırken, dosyasında *local.settings.js* içeriyorsa "Sitem adı" döndürülür `{ "Values": { "WEBSITE_SITE_NAME": "My Site Name" } }` .

[System.Configuration.ConfigurationManager. AppSettings](https://docs.microsoft.com/dotnet/api/system.configuration.configurationmanager.appsettings) özelliği, uygulama ayarı değerlerini almaya yönelik ALTERNATIF bir API 'dir, ancak `GetEnvironmentVariable` burada gösterildiği gibi kullanmanızı öneririz.

## <a name="binding-at-runtime"></a>Çalışma zamanında bağlama

C# ve diğer .NET dillerinde, öznitelikler içindeki [*bildirime dayalı*](https://en.wikipedia.org/wiki/Declarative_programming) bağlamalardan farklı olarak, [zorunlu bir bağlama](https://en.wikipedia.org/wiki/Imperative_programming) deseninin kullanılmasını sağlayabilirsiniz. Bağlama parametrelerinin tasarım zamanı yerine çalışma zamanında hesaplanması gerektiğinde, kesinlik temelli bağlama kullanışlıdır. Bu düzende, işlev kodunuzda, desteklenen giriş ve çıkış bağlamalarına bağlanabilirsiniz.

Zorunlu bir bağlamayı aşağıdaki gibi tanımlayın:

- İstediğiniz kesinlik bağlamalarınız için işlev imzasına bir **öznitelik eklemeyin.**
- Bir giriş parametresi veya olarak [`Binder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) geçirin [`IBinder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs) .
- Veri bağlamayı gerçekleştirmek için aşağıdaki C# stilini kullanın.

  ```cs
  using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
  {
      ...
  }
  ```

  `BindingTypeAttribute`, bağlamalarınızı tanımlayan .NET özniteliğidir ve `T` Bu bağlama türü tarafından desteklenen bir giriş veya çıkış türüdür. `T`bir `out` parametre türü (gibi `out JObject` ) olamaz. Örneğin, Mobile Apps tablo çıkış bağlaması [altı çıkış türünü](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22)destekler, ancak yalnızca [ICollector \<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) veya [ıasynccollector \<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) öğesini kesinlik ile kullanabilirsiniz.

### <a name="single-attribute-example"></a>Tek öznitelik örneği

Aşağıdaki örnek kod, çalışma zamanında tanımlanmış blob yolu ile bir [Depolama Blobu çıkış bağlaması](functions-bindings-storage-blob-output.md) oluşturur ve sonra blob 'a bir dize yazar.

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

[Blobattribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs/BlobAttribute.cs) , [Depolama Blobu](functions-bindings-storage-blob.md) girişini veya çıkış bağlamasını tanımlar ve [TextWriter](/dotnet/api/system.io.textwriter) desteklenen bir çıkış bağlama türüdür.

### <a name="multiple-attribute-example"></a>Birden çok öznitelik örneği

Yukarıdaki örnek, işlev uygulamasının ana depolama hesabı bağlantı dizesi (yani) için uygulama ayarını alır `AzureWebJobsStorage` . [Storageaccountattribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) ' i ekleyip öznitelik dizisini Içine geçirerek depolama hesabı için kullanılacak özel bir uygulama ayarı belirtebilirsiniz `BindAsync<T>()` . `Binder`Değil parametresini kullanın `IBinder` .  Örnek:

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
> [Tetikleyiciler ve bağlamalar hakkında daha fazla bilgi](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Azure Işlevleri için en iyi uygulamalar hakkında daha fazla bilgi edinin](functions-best-practices.md)
