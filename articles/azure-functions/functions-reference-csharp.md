---
title: Azure İşlevler C# komut dosyası geliştirici başvurusu
description: C# komut dosyasını kullanarak Azure İşlevlerini nasıl geliştireceklerini öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 12/12/2017
ms.author: cshoe
ms.openlocfilehash: 76af1f51c83e9554a51e6c17266fac739e6bd6b1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79276822"
---
# <a name="azure-functions-c-script-csx-developer-reference"></a>Azure İşlevler C# komut dosyası (.csx) geliştirici başvurusu

<!-- When updating this article, make corresponding changes to any duplicate content in functions-dotnet-class-library.md -->

Bu makale, C# komut dosyası (*.csx)* kullanarak Azure İşlevlerini geliştirmeye giriştir.

Azure İşlevleri C# ve C# komut dosyası programlama dillerini destekler. [Visual Studio sınıf kitaplık projesinde C# kullanma](functions-develop-vs.md)yla ilgili bir kılavuz arıyorsanız, [Bkz. C# geliştirici başvurusu.](functions-dotnet-class-library.md)

Bu makalede, [Azure İşlevleri geliştiricileri kılavuzunu](functions-reference.md)zaten okuduğunuz varsayar.

## <a name="how-csx-works"></a>.csx nasıl çalışır?

Azure İşlevleri için C# komut dosyası deneyimi [Azure Webİşler SDK'ya](https://github.com/Azure/azure-webjobs-sdk/wiki/Introduction)dayanır. Yöntem bağımsız değişkenleri aracılığıyla c# işlevinize veri akışı. Bağımsız değişken adları `function.json` bir dosyada belirtilir ve işlev kaydedici ve iptal belirteçleri gibi şeylere erişmek için önceden tanımlanmış adlar vardır.

*.csx* biçimi daha az "kazan plakası" yazmanızı ve yalnızca Bir C# işlevi yazmaya odaklanmanızı sağlar. Her şeyi bir ad alanına ve sınıfa `Run` sarmak yerine, yalnızca bir yöntem tanımlayın. Her zamanki gibi dosyanın başına herhangi bir derleme başvuruları ve ad alanları ekleyin.

Bir işlev uygulamasının *.csx* dosyaları, bir örnek baş harfe döndüğünde derlenir. Bu derleme adımı, c# sınıfı kitaplıklara kıyasla C# komut dosyası işlevleri için soğuk başlangıç gibi şeylerin daha uzun sürebileceği anlamına gelir. Bu derleme adımı, C# komut dosyası işlevlerinin Azure portalında da kullanılabilir olmasının nedenidir, C# sınıf kitaplıkları ise bu şekilde değildir.

## <a name="folder-structure"></a>Klasör yapısı

C# komut dosyası projesinin klasör yapısı aşağıdaki gibi görünür:

```
FunctionsProject
 | - MyFirstFunction
 | | - run.csx
 | | - function.json
 | | - function.proj
 | - MySecondFunction
 | | - run.csx
 | | - function.json
 | | - function.proj
 | - host.json
 | - extensions.csproj
 | - bin
```

İşlev uygulamasını yapılandırmak için kullanılabilecek paylaşılan bir [host.json](functions-host-json.md) dosyası vardır. Her işlevin kendi kod dosyası (.csx) ve bağlama yapılandırma dosyası (function.json) vardır.

Sürüm [2.x'te](functions-versions.md) ve İşlevler çalışma zamanının sonraki sürümlerinde gerekli olan bağlama `extensions.csproj` uzantıları, `bin` dosyada klasördeki gerçek kitaplık dosyalarıyla birlikte tanımlanır. Yerel olarak geliştirirken, [bağlama uzantılarını kaydetmeniz](./functions-bindings-register.md#extension-bundles)gerekir. Azure portalında işlevler geliştirirken, bu kayıt sizin için yapılır.

## <a name="binding-to-arguments"></a>Bağımsız değişkenlere bağlanma

Giriş veya çıktı `name` *verileri, function.json* yapılandırma dosyasındaki özellik üzerinden C# komut dosyası işlev parametresine bağlanır. Aşağıdaki örnek, sıra tetiklenen bir işlev için bir *function.json* dosyasını ve *run.csx* dosyasını gösterir. Sıra iletisinden veri alan parametre, `myQueueItem` `name` özelliğin değeri olduğu için adlandırılır.

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

```csharp
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.Extensions.Logging;
using Microsoft.WindowsAzure.Storage.Queue;
using System;

public static void Run(CloudQueueMessage myQueueItem, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem.AsString}");
}
```

İfade `#r` daha [sonra bu makalede](#referencing-external-assemblies)açıklanmıştır.

## <a name="supported-types-for-bindings"></a>Bağlamalar için desteklenen türler

Her bağlayıcının kendi desteklenen türleri vardır; örneğin, bir blob tetikleyici bir dize parametresi, bir POCO parametresi, bir `CloudBlockBlob` parametre veya diğer birkaç desteklenen türlerden herhangi biri ile kullanılabilir. [Blob bağlamaları için bağlayıcı başvuru makalesi,](functions-bindings-storage-blob-trigger.md#usage) blob tetikleyicileri için desteklenen tüm parametre türlerini listeler. Daha fazla bilgi için, [tetikleyiciler ve bağlamalar ve](functions-triggers-bindings.md) [her bağlayıcı türü için bağlayıcı başvuru dokümanları](functions-triggers-bindings.md#next-steps)bakın.

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="referencing-custom-classes"></a>Özel sınıflara başvurma

Özel bir Düz Eski CLR Nesnesi (POCO) sınıfı kullanmanız gerekiyorsa, sınıf tanımını aynı dosyaya ekleyebilir veya ayrı bir dosyaya koyabilirsiniz.

Aşağıdaki örnek, POCO sınıf tanımını içeren bir *run.csx* örneği gösterir.

```csharp
public static void Run(string myBlob, out MyClass myQueueItem)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
    myQueueItem = new MyClass() { Id = "myid" };
}

public class MyClass
{
    public string Id { get; set; }
}
```

Bir POCO sınıfı, her özellik için bir getter ve ayarlayıcı tanımlı olmalıdır.

## <a name="reusing-csx-code"></a>.csx kodunu yeniden kullanma

*Run.csx* dosyanızda diğer *.csx* dosyalarında tanımlanan sınıfları ve yöntemleri kullanabilirsiniz. Bunu yapmak için `#load` *run.csx* dosyanızdaki yönergeleri kullanın. Aşağıdaki örnekte, `MyLogger` *myLogger.csx'te* adlandırılmış bir günlük yordamı paylaşılır `#load` ve yönerge kullanılarak *run.csx'e* yüklenir:

Örnek *run.csx*:

```csharp
#load "mylogger.csx"

using Microsoft.Extensions.Logging;

public static void Run(TimerInfo myTimer, ILogger log)
{
    log.LogInformation($"Log by run.csx: {DateTime.Now}");
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

Örnek *mylogger.csx*:

```csharp
public static void MyLogger(ILogger log, string logtext)
{
    log.LogInformation(logtext);
}
```

Paylaşılan *bir .csx* dosyasını kullanmak, bir POCO nesnesi kullanarak işlevler arasında geçirilen verileri güçlü bir şekilde yazmak istediğinizde yaygın bir desendir. Aşağıdaki basitleştirilmiş örnekte, bir HTTP tetikleyicisi ve `Order` kuyruk tetikleyicisi, sipariş verilerini güçlü bir şekilde yazmak üzere adında bir POCO nesnesini paylaşır:

HTTP tetikleyicisi için örnek *run.csx:*

```cs
#load "..\shared\order.csx"

using System.Net;
using Microsoft.Extensions.Logging;

public static async Task<HttpResponseMessage> Run(Order req, IAsyncCollector<Order> outputQueueItem, ILogger log)
{
    log.LogInformation("C# HTTP trigger function received an order.");
    log.LogInformation(req.ToString());
    log.LogInformation("Submitting to processing queue.");

    if (req.orderId == null)
    {
        return new HttpResponseMessage(HttpStatusCode.BadRequest);
    }
    else
    {
        await outputQueueItem.AddAsync(req);
        return new HttpResponseMessage(HttpStatusCode.OK);
    }
}
```

Sıra tetikleyicisi için örnek *run.csx:*

```cs
#load "..\shared\order.csx"

using System;
using Microsoft.Extensions.Logging;

public static void Run(Order myQueueItem, out Order outputQueueItem, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed order...");
    log.LogInformation(myQueueItem.ToString());

    outputQueueItem = myQueueItem;
}
```

Örnek *order.csx*:

```cs
public class Order
{
    public string orderId {get; set; }
    public string custName {get; set;}
    public string custAddress {get; set;}
    public string custEmail {get; set;}
    public string cartId {get; set; }

    public override String ToString()
    {
        return "\n{\n\torderId : " + orderId +
                  "\n\tcustName : " + custName +
                  "\n\tcustAddress : " + custAddress +
                  "\n\tcustEmail : " + custEmail +
                  "\n\tcartId : " + cartId + "\n}";
    }
}
```

`#load` Yönergeyle göreli bir yol kullanabilirsiniz:

* `#load "mylogger.csx"`işlev klasöründe bulunan bir dosyayı yükler.
* `#load "loadedfiles\mylogger.csx"`işlev klasöründeki bir klasörde bulunan bir dosyayı yükler.
* `#load "..\shared\mylogger.csx"`işlev klasörüyle aynı düzeyde bir klasörde bulunan bir dosyayı yükler, yani doğrudan *wwwroot'un*altında.

`#load` Yönerge yalnızca *.csx* dosyalarıyla çalışır, *.cs* dosyalarıyla değil.

## <a name="binding-to-method-return-value"></a>Yöntem döndürme değerine bağlama

Bir çıktı bağlama için bir yöntem iade değeri, `$return` *function.json'daki*adı kullanarak kullanabilirsiniz. Örnekler için, [bkz. Tetikleyiciler ve bağlamalar.](./functions-bindings-return-value.md)

İade değerini yalnızca başarılı bir işlev yürütmesi her zaman çıktı bağlamasına geçmek üzere bir iade değeriyle sonuçlanırsa kullanın. Aksi takdirde, aşağıdaki bölümde gösterildiği gibi kullanın `ICollector` veya, `IAsyncCollector`

## <a name="writing-multiple-output-values"></a>Birden çok çıktı değeri yazma

Çıktı bağlamasına birden çok değer yazmak veya başarılı bir işlev çağırması çıktı bağlamaya geçmek [`ICollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) [`IAsyncCollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) için bir şey neden olmayabilir, kullanın veya türleri. Bu türler, yöntem tamamlandığında çıktı bağlamasına yalnızca yazma koleksiyonlarıdır.

Bu örnek, aynı kuyruğa `ICollector`birden çok sıra iletisi yazar:

```csharp
public static void Run(ICollector<string> myQueue, ILogger log)
{
    myQueue.Add("Hello");
    myQueue.Add("World!");
}
```

## <a name="logging"></a>Günlüğe Kaydetme

C#'daki akış günlüklerinize çıktınızı günlüğe kaydetmek [için, ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger)türünden bir bağımsız değişken ekleyin. Adını adlandırmanızı `log`öneririz. Azure `Console.Write` İşlevler'de kullanmaktan kaçının.

```csharp
public static void Run(string myBlob, ILogger log)
{
    log.LogInformation($"C# Blob trigger function processed: {myBlob}");
}
```

> [!NOTE]
> Yerine kullanabileceğiniz `TraceWriter`daha yeni bir günlük çerçevesi hakkında bilgi **için, Azure İşlerini İzle** makalesinde [C# işlevlerinde günlük yaz'a](functions-monitoring.md#write-logs-in-c-functions) bakın.

## <a name="async"></a>Zaman Uyumsuz

Bir işlevin [eşzamanlı](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/)olmasını sağlamak `async` için, anahtar `Task` kelimeyi kullanın ve bir nesneyi döndürün.

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName,
        Stream blobInput,
        Stream blobOutput)
{
    await blobInput.CopyToAsync(blobOutput, 4096);
}
```

Parametreleri async `out` işlevlerinde kullanamazsınız. Çıktı bağlamaları için [işlev döndürme değerini](#binding-to-method-return-value) veya [bir toplayıcı nesnesini](#writing-multiple-output-values) kullanın.

## <a name="cancellation-tokens"></a>İptal belirteçleri

Bir işlev, işlev sonlandırılmak üzereyken işletim sisteminin kodunuzu bildirmesini sağlayan [İptalToken](/dotnet/api/system.threading.cancellationtoken) parametresini kabul edebilir. Bu bildirimi, işlevin beklenmeyen bir şekilde sonlandırmadığından emin olmak için verileri tutarsız bir durumda bırakabilecek şekilde kullanabilirsiniz.

Aşağıdaki örnek, yaklaşan işlev sonlandırma için nasıl denetleyikarşılanınacak larını gösterir.

```csharp
using System;
using System.IO;
using System.Threading;

public static void Run(
    string inputText,
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
```

## <a name="importing-namespaces"></a>Ad alanlarını alma

Ad alanlarını almanız gerekiyorsa, bunu her zamanki `using` gibi, yan tümceyle yapabilirsiniz.

```csharp
using System.Net;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger log)
```

Aşağıdaki ad alanları otomatik olarak alınır ve bu nedenle isteğe bağlıdır:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`

## <a name="referencing-external-assemblies"></a>Dış derlemelere başvurma

Çerçeve derlemeleri için yönergeyi `#r "AssemblyName"` kullanarak referanslar ekleyin.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger log)
```

Aşağıdaki derlemeler Azure İşlevleri barındırma ortamı tarafından otomatik olarak eklenir:

* `mscorlib`
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`

Aşağıdaki derlemeler basit adla başvurulabilir (örneğin,): `#r "AssemblyName"`

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNet.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

## <a name="referencing-custom-assemblies"></a>Özel derlemelere başvurma

Özel bir derlemeye başvurmak için *paylaşılan* derleme yi veya *özel* derlemeyi kullanabilirsiniz:

* Paylaşılan derlemeler, bir işlev uygulaması içindeki tüm işlevler arasında paylaşılır. Özel bir derlemeye başvurmak için, derlemeyi `bin` [işlev uygulama kök](functions-reference.md#folder-structure) klasörünüzde (wwwroot) adlı bir klasöre yükleyin.

* Özel derlemeler belirli bir işlevin bağlamının bir parçasıdır ve farklı sürümlerin yan yüklenmesi desteklenir. Özel derlemeler işlev dizinindeki `bin` bir klasöre yüklenmelidir. Dosya adını kullanarak derlemelere `#r "MyAssembly.dll"`başvurun.

İşlev klasörünüze dosya yükleme hakkında bilgi için [paket yönetimi](#using-nuget-packages)bölümüne bakın.

### <a name="watched-directories"></a>İzlenen dizinler

İşlev komut dosyası dosyasını içeren dizin, derlemelerde yapılan değişiklikler için otomatik olarak izlenir. Diğer dizinlerde montaj değişiklikleri izlemek için, `watchDirectories` [host.json](functions-host-json.md)listeye ekleyin.

## <a name="using-nuget-packages"></a>NuGet paketlerini kullanma
NuGet paketlerini 2.x ve daha sonra C# işlevinde kullanmak için, function uygulamasının dosya sisteminde işlevin klasörüne bir *function.proj* dosyası yükleyin. *Microsoft.ProjectOxford.Face* sürüm *1.1.0'a*başvuru ekleyen örnek bir *function.proj* dosyası aşağıda verilmiştir:

```xml
<Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
    </PropertyGroup>

    <ItemGroup>
        <PackageReference Include="Microsoft.ProjectOxford.Face" Version="1.1.0" />
    </ItemGroup>
</Project>
```

Özel bir NuGet akışı kullanmak için, İşlev Uygulaması kökündeki *Nuget.Config* dosyasındaki akışı belirtin. Daha fazla bilgi için Bkz. [NuGet davranışını Yapılandırma.](/nuget/consume-packages/configuring-nuget-behavior)

> [!NOTE]
> 1.x C# işlevlerinde, NuGet paketleri *bir function.proj* dosyası yerine *project.json* dosyası ile başvurulur.

1.x işlevleri için *project.json* dosyası kullanın. Burada örnek bir *project.json* dosyası:

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

### <a name="using-a-functionproj-file"></a>function.proj dosyakullanma

1. Azure portalındaki işlevi açın. Günlükler sekmesi paket yükleme çıktısını görüntüler.
2. *bir function.proj* dosyasını yüklemek için, Azure İşlevleri geliştirici başvuru başlığında [işlev uygulama dosyalarının nasıl güncelleştirilir'de](functions-reference.md#fileupdate) açıklanan yöntemlerden birini kullanın.
3. *function.proj* dosyası yüklendikten sonra, işlevinizin akış günlüğünde aşağıdaki örnekteki çıktıyı görürsünüz:

```
2018-12-14T22:00:48.658 [Information] Restoring packages.
2018-12-14T22:00:48.681 [Information] Starting packages restore
2018-12-14T22:00:57.064 [Information] Restoring packages for D:\local\Temp\9e814101-fe35-42aa-ada5-f8435253eb83\function.proj...
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\function.proj...
2018-12-14T22:01:00.844 [Information] Installing Newtonsoft.Json 10.0.2.
2018-12-14T22:01:01.041 [Information] Installing Microsoft.ProjectOxford.Common.DotNetStandard 1.0.0.
2018-12-14T22:01:01.140 [Information] Installing Microsoft.ProjectOxford.Face.DotNetStandard 1.0.0.
2018-12-14T22:01:09.799 [Information] Restore completed in 5.79 sec for D:\local\Temp\9e814101-fe35-42aa-ada5-f8435253eb83\function.proj.
2018-12-14T22:01:10.905 [Information] Packages restored.
```

## <a name="environment-variables"></a>Ortam değişkenleri

Bir ortam değişkeni veya uygulama ayar `System.Environment.GetEnvironmentVariable`değeri elde etmek için aşağıdaki kod örneğinde gösterildiği gibi kullanın:

```csharp
public static void Run(TimerInfo myTimer, ILogger log)
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
```

<a name="imperative-bindings"></a>

## <a name="binding-at-runtime"></a>Çalışma zamanında bağlama

C# ve diğer .NET dillerinde, *function.json'daki* [*bildirimsel*](https://en.wikipedia.org/wiki/Declarative_programming) bağlamaların aksine [zorunlu](https://en.wikipedia.org/wiki/Imperative_programming) bağlama deseni kullanabilirsiniz. Zorunlu bağlama, bağlama parametrelerinin tasarım zamanı yerine çalışma zamanında hesaplanması gerektiğinde yararlıdır. Bu desenle, işlev kodunuzda desteklenen giriş ve çıkış bağlamalarını anında bağlayabilirsiniz.

Zorunlu bir bağlamayı aşağıdaki gibi tanımlayın:

- **Do not** İstediğiniz zorunlu bağlamalar için *function.json'a* bir giriş eklemeyin.
- Giriş parametresi [`Binder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) veya [`IBinder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).
- Veri bağlamagerçekleştirmek için aşağıdaki C# deseni kullanın.

```cs
using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
{
    ...
}
```

`BindingTypeAttribute`bağlamanızı tanımlayan ve `T` bu bağlama türü tarafından desteklenen bir giriş veya çıkış türüdür. `T`bir `out` parametre türü olamaz `out JObject`(gibi). Örneğin, Mobil Uygulamalar tablo çıktısı bağlama [altı çıkış türünü](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22)destekler, ancak yalnızca [ICollector\<T>](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) veya [`IAsyncCollector<T>`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) `T`.

### <a name="single-attribute-example"></a>Tek öznitelik örneği

Aşağıdaki örnek kod, çalışma zamanında tanımlanan blob yolu ile bağlayıcı bir [Depolama blob çıkışı](functions-bindings-storage-blob-output.md) oluşturur, sonra blob bir dize yazar.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host.Bindings.Runtime;

public static async Task Run(string input, Binder binder)
{
    using (var writer = await binder.BindAsync<TextWriter>(new BlobAttribute("samples-output/path")))
    {
        writer.Write("Hello World!!");
    }
}
```

[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs/BlobAttribute.cs) Depolama [blob](functions-bindings-storage-blob.md) giriş veya çıkış bağlama tanımlar ve [TextWriter](/dotnet/api/system.io.textwriter) desteklenen bir çıkış bağlama türüdür.

### <a name="multiple-attribute-example"></a>Birden çok öznitelik örneği

Önceki örnek, işlev uygulamasının ana Depolama hesabı bağlantı dizesi `AzureWebJobsStorage`için uygulama ayarını alır (ki bu da). [StorageAccountAttribute'ı](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) ekleyerek ve öznitelik dizisini ' ye `BindAsync<T>()`geçirerek Depolama hesabı için kullanılacak özel bir uygulama ayarını belirtebilirsiniz. Parametre `Binder` kullanın, `IBinder`değil.  Örnek:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host.Bindings.Runtime;

public static async Task Run(string input, Binder binder)
{
    var attributes = new Attribute[]
    {
        new BlobAttribute("samples-output/path"),
        new StorageAccountAttribute("MyStorageAccount")
    };

    using (var writer = await binder.BindAsync<TextWriter>(attributes))
    {
        writer.Write("Hello World!");
    }
}
```

Aşağıdaki tabloda, her bağlama türü için .NET öznitelikleri ve tanımlandıkları paketler listelenir.

> [!div class="mx-codeBreakAll"]
> | Bağlama | Öznitelik | Başvuru ekleme |
> |------|------|------|
> | Cosmos DB | [`Microsoft.Azure.WebJobs.DocumentDBAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.CosmosDB"` |
> | Event Hubs | [`Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/v2.x/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs) | `#r "Microsoft.Azure.Jobs.ServiceBus"` |
> | Mobile Apps | [`Microsoft.Azure.WebJobs.MobileTableAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.MobileApps"` |
> | Notification Hubs | [`Microsoft.Azure.WebJobs.NotificationHubAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.NotificationHubs"` |
> | Service Bus | [`Microsoft.Azure.WebJobs.ServiceBusAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs) | `#r "Microsoft.Azure.WebJobs.ServiceBus"` |
> | Depolama kuyruğu | [`Microsoft.Azure.WebJobs.QueueAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
> | Depolama blobu | [`Microsoft.Azure.WebJobs.BlobAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs/BlobAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
> | Depolama tablosu | [`Microsoft.Azure.WebJobs.TableAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
> | Twilio | [`Microsoft.Azure.WebJobs.TwilioSmsAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.Twilio"` |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tetikleyiciler ve bağlamalar hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Azure İşlevleri için en iyi uygulamalar hakkında daha fazla bilgi edinin](functions-best-practices.md)
