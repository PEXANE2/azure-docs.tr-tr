---
title: Azure Işlevleri 'nde .NET 5,0 için .NET yalıtılmış işlem kılavuzu
description: Azure 'da .NET 5,0 işlem dışı olarak C# işlevlerinizi çalıştırmak için .NET yalıtılmış işlemini nasıl kullanacağınızı öğrenin.
ms.service: azure-functions
ms.topic: conceptual
ms.date: 03/01/2021
ms.custom: template-concept
ms.openlocfilehash: 5ee38fa4b005cf053890c223dfec9244c637bd00
ms.sourcegitcommit: 18a91f7fe1432ee09efafd5bd29a181e038cee05
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2021
ms.locfileid: "103561830"
---
# <a name="guide-for-running-functions-on-net-50-in-azure"></a>Azure 'da .NET 5,0 üzerinde işlev çalıştırmaya yönelik kılavuz

Bu makale, Azure Işlevleri 'nde işlem dışı çalışan .NET yalıtılmış işlem işlevleri geliştirmek Için C# kullanmaya giriş niteliğindedir. İşlem dışı çalışmak, işlev kodunuzu Azure Işlevleri çalışma zamanından ayırarak kullanmanıza olanak sağlar. Ayrıca, geçerli .NET 5,0 sürümünü hedefleyen işlevleri oluşturmanız ve çalıştırmanız için bir yol sağlar. 

| Başlarken | Kavramlar| Örnekler |
|--|--|--| 
| <ul><li>[Visual Studio Code’u kullanma](dotnet-isolated-process-developer-howtos.md?pivots=development-environment-vscode)</li><li>[Komut satırı araçlarını kullanma](dotnet-isolated-process-developer-howtos.md?pivots=development-environment-cli)</li><li>[Visual Studio’yu kullanma](dotnet-isolated-process-developer-howtos.md?pivots=development-environment-vs)</li></ul> | <ul><li>[Barındırma seçenekleri](functions-scale.md)</li><li>[İzleme](functions-monitoring.md)</li> | <ul><li>[Başvuru örnekleri](https://github.com/Azure/azure-functions-dotnet-worker/tree/main/samples)</li></ul> |

.NET 5,0 ' i desteklemeniz veya işlevlerinizi işlem dışı olarak çalıştırmanız gerekmiyorsa, bunun yerine [C# sınıf kitaplığı işlevleri geliştirmek](functions-dotnet-class-library.md)isteyebilirsiniz.

## <a name="why-net-isolated-process"></a>.NET yalıtılmış işlemi neden?

Daha önce Azure Işlevleri, ana bilgisayarla aynı işlemde [bir sınıf kitaplığı olarak](functions-dotnet-class-library.md) çalışan .net işlevleri için yalnızca sıkı tümleşik bir mod destekliyordu. Bu mod, ana bilgisayar işlemiyle işlevleri arasında derin tümleştirme sağlar. Örneğin, .NET sınıf kitaplığı işlevleri bağlama API 'Lerini ve türlerini paylaşabilir. Ancak bu tümleştirme, ana bilgisayar işlemiyle .NET işlevi arasında daha sıkı bir şekilde bir bağlayıcısı gerektirir. Örneğin, işlem içinde çalışan .NET işlevlerinin, Işlevler çalışma zamanı ile aynı .NET sürümünde çalışması gerekir. Bu kısıtlamaların dışında çalıştırmanızı sağlamak için artık yalıtılmış bir işlemde çalıştırmayı tercih edebilirsiniz. Bu işlem yalıtımı, Işlevler çalışma zamanı tarafından yerel olarak desteklenmeyen geçerli .NET sürümlerini (.NET 5,0 gibi) kullanan işlevler geliştirmenize da imkan tanır.

Bu işlevler ayrı bir işlemde çalıştığından, .NET yalıtılmış işlev uygulamaları ve .NET sınıf kitaplığı işlev uygulamaları arasında bazı [özellik ve işlevsellik farklılıkları](#differences-with-net-class-library-functions) vardır.

### <a name="benefits-of-running-out-of-process"></a>İşlem dışı çalıştırmanın avantajları

İşlem dışı çalışırken .NET işlevleriniz aşağıdaki avantajlardan yararlanabilir: 

+ Daha az çakışma: işlevler ayrı bir işlemde çalıştığı için, uygulamanızda kullanılan derlemeler, ana bilgisayar işlemi tarafından kullanılan aynı derlemelerin farklı sürümüyle çakışmaz.  
+ İşlemin tam denetimi: uygulamanın başlangıcını denetler ve kullanılan yapılandırmayı denetleyebilir ve ara yazılım başlatılır.
+ Bağımlılık ekleme: işlem üzerinde tam denetim sahibi olduğunuz için, bağımlılık ekleme ve ara yazılımı işlev uygulamanıza ekleme için geçerli .NET davranışları kullanabilirsiniz. 

## <a name="supported-versions"></a>Desteklenen sürümler

Şu anda işlem dışı çalışmak için desteklenen .NET sürümü .NET 5,0 ' dir.

## <a name="net-isolated-project"></a>.NET yalıtılmış projesi

.NET yalıtılmış işlev projesi temelde .NET 5,0 ' i hedefleyen bir .NET konsol uygulaması projem. Aşağıda, .NET yalıtılmış projesinde gereken temel dosyalar verilmiştir:

+ Dosyada [host.js](functions-host-json.md) .
+ Dosyada [local.settings.js](functions-run-local.md#local-settings-file) .
+ Projeyi ve bağımlılıkları tanımlayan C# proje dosyası (. csproj).
+ Uygulamanın giriş noktası olan Program.cs dosyası.

## <a name="package-references"></a>Paket başvuruları

.NET projeniz, işlem dışı çalışırken hem çekirdek işlevleri hem de bağlama uzantıları uygulayan benzersiz bir paket kümesi kullanır. 

### <a name="core-packages"></a>Çekirdek paketler 

.NET işlevlerinizi yalıtılmış bir işlemde çalıştırmak için aşağıdaki paketler gereklidir:

+ [Microsoft. Azure. Functions. Worker](https://www.nuget.org/packages/Microsoft.Azure.Functions.Worker/)
+ [Microsoft. Azure. Functions. Worker. SDK](https://www.nuget.org/packages/Microsoft.Azure.Functions.Worker.Sdk/)

### <a name="extension-packages"></a>Uzantı paketleri

Bir .NET yalıtılmış işleminde çalışan işlevler farklı bağlama türleri kullandığından, benzersiz bir bağlama uzantısı paketleri kümesi gerektirir. 

Bu uzantı paketlerini [Microsoft. Azure. Functions. Worker. Extensions](https://www.nuget.org/packages?q=Microsoft.Azure.Functions.Worker.Extensions)altında bulacaksınız.

## <a name="start-up-and-configuration"></a>Başlatma ve yapılandırma 

.NET yalıtılmış işlevleri kullanılırken, genellikle Program.cs içinde olan işlev uygulamanızın başlangıcına erişebilirsiniz. Kendi konak örneğinizi oluşturup başlatmaktan siz sorumlusunuz. Bu nedenle, uygulamanız için yapılandırma ardışık düzenine doğrudan erişiminiz de vardır. Bağımlılıkları çok daha kolay bir şekilde ekleyebilir ve işlem dışı çalıştırırken ara yazılım çalıştırabilirsiniz. 

Aşağıdaki kodda bir işlem hattının örneği gösterilmektedir `HostBuilder` :

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/FunctionApp/Program.cs" id="docsnippet_startup":::

`HostBuilder` `IHost` , İşlev uygulamanızı başlatmak için zaman uyumsuz olarak çalıştırdığınız, tam olarak başlatılmış bir örnek derlemek ve döndürmek için kullanılır. 

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/FunctionApp/Program.cs" id="docsnippet_host_run":::

### <a name="configuration"></a>Yapılandırma

Konak Oluşturucu işlem hattına erişimi olması, başlatma sırasında uygulamaya özgü herhangi bir yapılandırmayı ayarlayabileceğiniz anlamına gelir. Bu yapılandırma işlevleri ayrı bir işlemde çalışan işlev uygulamanız için geçerlidir. İşlev konağında veya tetikleyici ve bağlama yapılandırmasında değişiklik yapmak için, [host.jsdosya üzerinde](functions-host-json.md)kullanmanız gerekecektir.      

<!--The following example shows how to add configuration `args`, which are read as command-line arguments: 
 
:::code language="csharp" 
                .ConfigureAppConfiguration(c =>
                {
                    c.AddCommandLine(args);
                })
                :::

The `ConfigureAppConfiguration` method is used to configure the rest of the build process and application. This example also uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder?view=dotnet-plat-ext-5.0&preserve-view=true), which makes it easier to add multiple configuration items. Because `ConfigureAppConfiguration` returns the same instance of [`IConfiguration`](/dotnet/api/microsoft.extensions.configuration.iconfiguration?view=dotnet-plat-ext-5.0&preserve-view=true), you can also just call it multiple times to add multiple configuration items.-->  
Tüm yapılandırma kümesine hem hem de erişebilirsiniz [`HostBuilderContext.Configuration`](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration?view=dotnet-plat-ext-5.0&preserve-view=true) [`IHost.Services`](/dotnet/api/microsoft.extensions.hosting.ihost.services?view=dotnet-plat-ext-5.0&preserve-view=true) .

Yapılandırma hakkında daha fazla bilgi edinmek için [ASP.NET Core yapılandırma](/aspnet/core/fundamentals/configuration/?view=aspnetcore-5.0&preserve-view=true)konusuna bakın. 

### <a name="dependency-injection"></a>Bağımlılık ekleme

Bağımlılık ekleme, .NET sınıf kitaplıklarıyla karşılaştırıldığında basitleştirilmiştir. Hizmetleri kaydettirmek için bir başlangıç sınıfı oluşturmak zorunda kalmak yerine, yalnızca `ConfigureServices` konak Oluşturucu 'yu çağırmanız ve [`IServiceCollection`](/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection?view=dotnet-plat-ext-5.0&preserve-view=true) belirli hizmetleri eklemek için üzerinde uzantı yöntemlerini kullanmanız gerekir. 

Aşağıdaki örnek bir tek hizmet bağımlılığını çıkarır:  
 
:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/FunctionApp/Program.cs" id="docsnippet_dependency_injection" :::

Daha fazla bilgi için bkz. [ASP.NET Core bağımlılık ekleme](/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-5.0&preserve-view=true).

<!--### Middleware

.NET isolated also supports middleware registration, again by using a model similar to what exists in ASP.NET. This model gives you the ability to inject logic into the invocation pipeline, and before and after functions execute.

While the full middleware registration set of APIs is not yet exposed, we do support middleware registration and have added an example to the sample application under the Middleware folder.

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/FunctionApp/Program.cs" id="docsnippet_middleware" :::-->

## <a name="execution-context"></a>Yürütme bağlamı

.NET yalıtılmış bir `FunctionContext` nesneyi işlev yöntemlerinize geçirir. Bu nesne [`ILogger`](/dotnet/api/microsoft.extensions.logging.ilogger?view=dotnet-plat-ext-5.0&preserve-view=true) , `GetLogger` yöntemini çağırarak ve bir dize sağlayarak günlüklere yazılacak bir örnek almanızı sağlar `categoryName` . Daha fazla bilgi için bkz. [günlüğe kaydetme](#logging). 

## <a name="bindings"></a>Bağlamalar 

Bağlamalar yöntemleri, parametreleri ve dönüş türleri üzerinde öznitelikler kullanılarak tanımlanır. Bir işlev yöntemi, `Function` Aşağıdaki örnekte gösterildiği gibi bir giriş parametresine uygulanan ve bir tetikleyici özniteliği olan bir yöntemdir:

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/Extensions/Queue/QueueFunction.cs" id="docsnippet_queue_trigger" :::

Tetikleyici özniteliği Tetikleyici türünü belirtir ve giriş verilerini bir yöntem parametresine bağlar. Önceki örnek işlev bir kuyruk iletisi tarafından tetiklenir ve kuyruk iletisi parametresindeki yöntemine geçirilir `myQueueItem` .

`Function`Özniteliği yöntemi bir işlev giriş noktası olarak işaretler. Ad bir proje içinde benzersiz olmalıdır, bir harfle başlamalı ve yalnızca harf, sayı, `_` ve `-` , en fazla 127 karakter içermelidir. Proje şablonları genellikle adlı bir yöntem oluşturur `Run` , ancak yöntem adı geçerli bir C# Yöntem adı olabilir.

.NET yalıtılmış projeleri ayrı bir çalışan işleminde çalıştığı için bağlamalar, ve gibi zengin bağlama sınıflarından yararlanamaz `ICollector<T>` `IAsyncCollector<T>` `CloudBlockBlob` . Ayrıca, [Documentclient](/dotnet/api/microsoft.azure.documents.client.documentclient) ve [brokeredmessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)gibi temel hizmet SDK 'larından devralınan türler için doğrudan destek de yoktur. Bunun yerine, bağlamalar dizeler, diziler ve düz eski sınıf nesneleri (POCOs) gibi seri hale getirilebilir türlere bağımlıdır. 

HTTP Tetikleyicileri için, `HttpRequestData` `HttpResponseData` istek ve yanıt verilerine erişmek üzere ve kullanmanız gerekir. Bunun nedeni, işlem dışı çalışırken özgün HTTP isteğine ve yanıt nesnelerine erişiminizin olmadığı bir işlemdir. 

### <a name="input-bindings"></a>Giriş bağlamaları

Bir işlev, bir işleve veri geçiresağlayan sıfır veya daha fazla giriş bağlaması olabilir. Tetikleyiciler gibi, giriş bağlamaları bir giriş parametresine bir bağlama özniteliği uygulanarak tanımlanır. İşlev çalıştırıldığında, çalışma zamanı bağlamada belirtilen verileri almaya çalışır. İstenen veriler genellikle bağlama parametreleri kullanılarak tetikleyici tarafından belirtilen bilgilere bağımlıdır.  

### <a name="output-bindings"></a>Çıkış bağlamaları

Bir çıkış bağlamasına yazmak için, işlev yöntemine bir çıkış bağlama özniteliği uygulamanız gerekir ve bu, bağlama hizmetine nasıl yazılacağını tanımlamış. Yöntemi tarafından döndürülen değer çıkış bağlamasına yazılır. Örneğin, aşağıdaki örnek, bir çıkış bağlaması kullanarak adlı bir ileti kuyruğuna bir String değeri yazar `functiontesting2` :

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/Extensions/Queue/QueueFunction.cs" id="docsnippet_queue_output_binding" :::

### <a name="multiple-output-bindings"></a>Birden çok çıkış bağlaması

Bir çıkış bağlamasına yazılan veriler her zaman işlevin dönüş değeridir. Birden fazla çıkış bağlamaya yazmanız gerekiyorsa, özel bir dönüş türü oluşturmanız gerekir. Bu dönüş türü, sınıfın bir veya daha fazla özelliklerine uygulanmış çıkış bağlama özniteliğine sahip olmalıdır. Aşağıdaki örnek hem HTTP yanıtına hem de bir sıra çıkış bağlamaya Yazar:

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/FunctionApp/Function1/Function1.cs" id="docsnippet_multiple_outputs":::

### <a name="http-trigger"></a>HTTP tetikleyicisi

HTTP Tetikleyicileri, gelen HTTP istek iletisini, `HttpRequestData` işleve geçirilen bir nesneye çevirir. Bu nesne,,,, `Headers` `Cookies` `Identities` `URL` ve isteğe bağlı bir ileti dahil olmak üzere istekten veri sağlar `Body` . Bu nesne, isteğin kendisi değil HTTP istek nesnesinin bir gösterimidir. 

Benzer şekilde, işlev, `HttpReponseData` ileti `StatusCode` , `Headers` ve isteğe bağlı olarak BIR ileti gibi http yanıtı oluşturmak için kullanılan verileri sağlayan bir nesnesi döndürür `Body` .  

Aşağıdaki kod bir HTTP tetikleyicisine sahiptir 

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/Extensions/Http/HttpFunction.cs" id="docsnippet_http_trigger" :::

## <a name="logging"></a>Günlüğe Kaydetme

.NET yalıtılmış ' te, [`ILogger`](/dotnet/api/microsoft.extensions.logging.ilogger?view=dotnet-plat-ext-5.0&preserve-view=true) işlevinizin geçirildiği bir nesneden elde edilen bir örneği kullanarak günlüklere yazabilirsiniz `FunctionContext` . `GetLogger`İçindeki günlüklerin yazıldığı kategorinin adı olan bir dize değeri geçirerek yöntemini çağırın. Kategori genellikle günlüklerin yazıldığı belirli işlevin adıdır. Kategoriler hakkında daha fazla bilgi için [İzleme makalesine](functions-monitoring.md#log-levels-and-categories)bakın. 

Aşağıdaki örnek, `ILogger` bir işlev içindeki bir ve yazma günlüklerinin nasıl alınacağını gösterir:

:::code language="csharp" source="~/azure-functions-dotnet-worker/samples/Extensions/Http/HttpFunction.cs" id="docsnippet_logging" ::: 

`ILogger`Veya gibi çeşitli günlük düzeylerini yazmak için çeşitli yöntemler kullanın `LogWarning` `LogError` . Günlük düzeyleri hakkında daha fazla bilgi için [İzleme makalesine](functions-monitoring.md#log-levels-and-categories)bakın.

[`ILogger`](/dotnet/api/microsoft.extensions.logging.ilogger?view=dotnet-plat-ext-5.0&preserve-view=true) [Bağımlılık ekleme](#dependency-injection)kullanılırken de sağlanır.

## <a name="differences-with-net-class-library-functions"></a>.NET sınıf kitaplığı işlevleriyle ilgili farklılıklar

Bu bölümde, .NET 5,0 ' de çalışan, işlem içi .NET sınıf kitaplığı işlevleri ile karşılaştırıldığında işlevsel ve davranış farklarının geçerli durumu açıklanmaktadır:

| Özellik/davranış |  İşlem içi (.NET Core 3,1) | İşlem dışı (.NET 5,0) |
| ---- | ---- | ---- |
| .NET sürümleri | LTS (.NET Core 3,1) | Geçerli (.NET 5,0) |
| Çekirdek paketler | [Microsoft. NET. SDK. Functions](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions/) | [Microsoft. Azure. Functions. Worker](https://www.nuget.org/packages/Microsoft.Azure.Functions.Worker/)<br/>[Microsoft. Azure. Functions. Worker. SDK](https://www.nuget.org/packages/Microsoft.Azure.Functions.Worker.Sdk) | 
| Uzantı paketlerini bağlama | [`Microsoft.Azure.WebJobs.Extensions.*`](https://www.nuget.org/packages?q=Microsoft.Azure.WebJobs.Extensions)  | Altındaki [`Microsoft.Azure.Functions.Worker.Extensions.*`](https://www.nuget.org/packages?q=Microsoft.Azure.Functions.Worker.Extensions) | 
| Günlüğe Kaydetme | [`ILogger`](/dotnet/api/microsoft.extensions.logging.ilogger?view=dotnet-plat-ext-5.0&preserve-view=true) işleve geçirildi | [`ILogger`](/dotnet/api/microsoft.extensions.logging.ilogger?view=dotnet-plat-ext-5.0&preserve-view=true) alındığı yer `FunctionContext` |
| İptal belirteçleri | [Desteklenir](functions-dotnet-class-library.md#cancellation-tokens) | Desteklenmez |
| Çıkış bağlamaları | Out parametreleri | Dönüş değerleri |
| Çıkış bağlaması türleri |  `IAsyncCollector`, [Documentclient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet&preserve-view=true), [brokeredmessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azure-dotnet&preserve-view=true)ve istemciye özgü diğer türler | Basit türler, JSON serileştirilebilir türler ve diziler. |
| Birden çok çıkış bağlaması | Desteklenir | [Desteklenir](#multiple-output-bindings) |
| HTTP tetikleyicisi | [`HttpRequest`](/dotnet/api/microsoft.aspnetcore.http.httprequest?view=aspnetcore-5.0&preserve-view=true)/[`ObjectResult`](/dotnet/api/microsoft.aspnetcore.mvc.objectresult?view=aspnetcore-5.0&preserve-view=true) | `HttpRequestData`/`HttpResponseData` |
| Dayanıklı İşlevler | [Desteklenir](durable/durable-functions-overview.md) | Desteklenmez | 
| Kesinlik temelli bağlamalar | [Desteklenir](functions-dotnet-class-library.md#binding-at-runtime) | Desteklenmez |
| Yapıt üzerinde function.js | Üret | Üretilmedi |
| Yapılandırma | [ Üzerindehost.js](functions-host-json.md) | [host.js](functions-host-json.md) ve özel başlatma |
| Bağımlılık ekleme | [Desteklenir](functions-dotnet-dependency-injection.md)  | [Desteklenir](#dependency-injection) |
| Ara yazılım | Desteklenmez | Desteklenir |
| Soğuk başlangıç süreleri | Genelde | Daha uzun, tam zamanında başlangıç. Olası gecikmeleri azaltmak için Windows yerine Linux üzerinde çalıştırın. |
| ReadyToRun | [Desteklenir](functions-dotnet-class-library.md#readytorun) | _TBD_ |

## <a name="known-issues"></a>Bilinen sorunlar

.NET yalıtılmış işlem işlevlerini çalıştırma sorunları hakkında daha fazla bilgi edinmek için [Bu bilinen sorunlar sayfasına](https://aka.ms/AAbh18e)bakın. Sorunları bildirmek için [Bu GitHub deposunda bir sorun oluşturun](https://github.com/Azure/azure-functions-dotnet-worker/issues/new/choose).  

## <a name="next-steps"></a>Sonraki adımlar

+ [Tetikleyiciler ve bağlamalar hakkında daha fazla bilgi](functions-triggers-bindings.md)
+ [Azure Işlevleri için en iyi uygulamalar hakkında daha fazla bilgi edinin](functions-best-practices.md)
