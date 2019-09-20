---
title: .NET Azure Işlevlerinde bağımlılık ekleme 'yi kullanma
description: .NET işlevlerinde Hizmetleri kaydetmek ve kullanmak için bağımlılık ekleme eklemeyi nasıl kullanacağınızı öğrenin
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
keywords: Azure işlevleri, işlevler, sunucusuz mimari
ms.service: azure-functions
ms.devlang: dotnet
ms.topic: reference
ms.date: 09/05/2019
ms.author: cshoe
ms.reviewer: jehollan
ms.openlocfilehash: e1cf67abcc44a3ca134e5435137869d4fff1a7eb
ms.sourcegitcommit: a7a9d7f366adab2cfca13c8d9cbcf5b40d57e63a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71162357"
---
# <a name="use-dependency-injection-in-net-azure-functions"></a>.NET Azure Işlevlerinde bağımlılık ekleme 'yi kullanma

Azure Işlevleri, sınıflar ve bunların bağımlılıkları arasında [denetimin (IOC) bir Iç sürümünü](https://docs.microsoft.com/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) elde etmek için bir tekniktir olan bağımlılık ekleme (dı) yazılım tasarım modelini destekler.

- Azure Işlevlerine bağımlılık ekleme, .NET Core bağımlılığı ekleme özellikleri üzerine kurulmuştur. [.NET Core bağımlılığı ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) konusunda benzerlik yapmanız önerilir. Ancak, bağımlılıkları geçersiz kılma ve yapılandırma değerlerinin tüketim planında Azure Işlevleri ile nasıl okunduğu ile ilgili farklılıklar vardır.

- Bağımlılık ekleme desteği, Azure Işlevleri 2. x ile başlar.

## <a name="prerequisites"></a>Önkoşullar

Bağımlılık ekleme 'yi kullanabilmeniz için aşağıdaki NuGet paketlerini yüklemelisiniz:

- [Microsoft. Azure. Functions. uzantıları](https://www.nuget.org/packages/Microsoft.Azure.Functions.Extensions/)

- [Microsoft. net. SDK. Functions paket](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions/) sürümü 1.0.28 veya üzeri

## <a name="register-services"></a>Hizmetleri Kaydet

Hizmetleri kaydetmek için bir `IFunctionsHostBuilder` örneğe bileşen yapılandırmak ve eklemek üzere bir yöntem oluşturun.  Azure işlevleri ana makinesi bir örneği oluşturur `IFunctionsHostBuilder` ve doğrudan kendi yönteğinize geçirir.

Yöntemi kaydetmek için başlangıç sırasında kullanılan tür `FunctionsStartup` adını belirten derleme özniteliğini ekleyin.

```csharp
using System;
using Microsoft.Azure.Functions.Extensions.DependencyInjection;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Http;
using Microsoft.Extensions.Logging;

[assembly: FunctionsStartup(typeof(MyNamespace.Startup))]

namespace MyNamespace
{
    public class Startup : FunctionsStartup
    {
        public override void Configure(IFunctionsHostBuilder builder)
        {
            builder.Services.AddHttpClient();

            builder.Services.AddSingleton((s) => {
                return new MyService();
            });

            builder.Services.AddSingleton<ILoggerProvider, MyLoggerProvider>();
        }
    }
}
```

### <a name="caveats"></a>Uyarılar

Çalışma zamanı başlangıç sınıfını işleyerek önce ve sonra çalıştırılan bir dizi kayıt adımı. Bu nedenle, aşağıdaki öğeleri aklınızda bulundurun:

- *Başlangıç sınıfı yalnızca kurulum ve kayıt için tasarlanmıştır.* Başlangıç işlemi sırasında başlangıçta kayıtlı hizmetleri kullanmaktan kaçının. Örneğin, başlatma sırasında kaydedilen bir günlükçüde bir iletiyi günlüğe almaya çalışmayın. Kayıt sürecinin bu noktası, hizmetlerinizin kullanıma hazır olması için çok erken bir işlemdir. `Configure` Yöntem çalıştırıldıktan sonra işlevler çalışma zamanı, hizmetlerinizin nasıl çalıştığını etkileyebilecek ek bağımlılıklar kaydetmeye devam eder.

- *Bağımlılık ekleme kapsayıcısı yalnızca açık olarak kayıtlı türleri barındırır*. Yalnızca Injectable türleri olarak kullanılabilen hizmetler, `Configure` yönteminde kurulum olan şeydir. Sonuç olarak, gibi `BindingContext` `ExecutionContext` işlevlere özgü türler kurulum sırasında veya Injectable türleri olarak kullanılamaz.

## <a name="use-injected-dependencies"></a>Eklenen bağımlılıkları kullan

Oluşturucu Ekleme, bağımlılıklarınızı bir işlevde kullanılabilir hale getirmek için kullanılır. Oluşturucu Ekleme kullanımı statik sınıflar kullanmanıza gerek duyar.

Aşağıdaki örnek, `IMyService` ve `HttpClient` bağımlılıklarının http ile tetiklenen bir işleve nasıl eklendiğini gösterir. Bu örnek, bir `HttpClient` başlangıç kaydı için gereken [Microsoft. Extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) paketini kullanır.

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;

namespace MyNamespace
{
    public class HttpTrigger
    {
        private readonly IMyService _service;
        private readonly HttpClient _client;

        public HttpTrigger(IMyService service, IHttpClientFactory httpClientFactory)
        {
            _service = service;
            _client = httpClientFactory.CreateClient();
        }

        [FunctionName("GetPosts")]
        public async Task<IActionResult> Get(
            [HttpTrigger(AuthorizationLevel.Function, "get", Route = "posts")] HttpRequest req,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            var res = await _client.GetAsync("https://microsoft.com");
            await _service.AddResponse(res);

            return new OkResult();
        }
    }
}
```

## <a name="service-lifetimes"></a>Hizmet yaşam süreleri

Azure Işlevleri uygulamaları, [ASP.net bağımlılığı ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes)ile aynı hizmet yaşam sürelerini sağlar. Işlevler uygulaması için farklı hizmet yaşam süreleri aşağıdaki gibi davranır:

- **Geçici**: Geçici hizmetler hizmet istekleri üzerine oluşturulur.
- **Kapsam**: Kapsamlı hizmet ömrü bir işlev yürütme ömrüyle eşleşir. Kapsamlı hizmetler, her yürütme için bir kez oluşturulur. Yürütme sırasında o hizmetin sonraki istekleri, var olan hizmet örneğini yeniden kullanır.
- **Tek**: Tek hizmet ömrü ana bilgisayar ömrü ile eşleşir ve bu örnekteki işlev Yürütmelerinde yeniden kullanılır. Tek ömür Hizmetleri, bağlantılar ve istemciler için, örneğin `SqlConnection` veya `HttpClient` örnekler için önerilir.

GitHub üzerinde [farklı hizmet yaşam sürelerinin bir örneğini](https://aka.ms/functions/di-sample) görüntüleyin veya indirin.

## <a name="logging-services"></a>Günlüğe kaydetme hizmetleri

Kendi günlük sağlayıcınıza ihtiyacınız varsa, özel bir türü `ILoggerProvider` örnek olarak kaydedin. Application Insights, Azure Işlevleri tarafından otomatik olarak eklenir.

> [!WARNING]
> - Ortam tarafından sunulan `AddApplicationInsightsTelemetry()` hizmetlerle çakışan Hizmetleri kaydederken hizmetler koleksiyonuna eklemeyin.
> - Kendi kendinize `TelemetryConfiguration` kaydolmayın veya `TelemetryClient` yerleşik Application Insights işlevlerini kullanıyorsanız.

## <a name="function-app-provided-services"></a>İşlev uygulaması tarafından sunulan hizmetler

İşlev Konağı birçok hizmeti kaydeder. Aşağıdaki hizmetler uygulamanızda bir bağımlılık olarak ele alınır:

|Hizmet türü|Süre|Açıklama|
|--|--|--|
|`Microsoft.Extensions.Configuration.IConfiguration`|Adet|Çalışma zamanı yapılandırması|
|`Microsoft.Azure.WebJobs.Host.Executors.IHostIdProvider`|Adet|Konak örneğinin KIMLIĞINI sağlamaktan sorumlu|

Bağımlılığı almak istediğiniz başka hizmetler varsa, [bir sorun oluşturun ve bunları GitHub 'da önerin](https://github.com/azure/azure-functions-host).

### <a name="overriding-host-services"></a>Konak hizmetlerini geçersiz kılma

Konak tarafından belirtilen geçersiz kılma Hizmetleri şu anda desteklenmiyor.  Geçersiz kılmak istediğiniz hizmetler varsa, [bir sorun oluşturun ve bunları GitHub 'da önerin](https://github.com/azure/azure-functions-host).

## <a name="working-with-options-and-settings"></a>Seçeneklerle ve ayarlarla çalışma

[Uygulama ayarlarında](./functions-how-to-use-azure-function-app-settings.md#settings) tanımlanan değerler, başlangıç sınıfındaki uygulama ayarları `IConfiguration` değerlerini okumanızı sağlayan bir örnekte kullanılabilir.

`IConfiguration` Örnekten değerleri özel bir türe ayıklayabilirsiniz. Uygulama ayarları değerlerini özel bir türe kopyalamak, bu değerleri tablo haline getirerek, hizmetlerinizi test etmelerini kolaylaştırır. Uygulama ayarıyla tutarlı adlı bir özellik içeren aşağıdaki sınıfı göz önünde bulundurun.

```csharp
public class MyOptions
{
    public string MyCustomSetting { get; set; }
}
```

`Startup.Configure` Yöntemi içinde, aşağıdaki kodu kullanarak `IConfiguration` örnekten değerleri özel türüne ayıklayabilirsiniz:

```csharp
builder.Services.AddOptions<MyOptions>()
                .Configure<IConfiguration>((settings, configuration) =>
                                           {
                                                configuration.Bind(settings);
                                           });
```

Çağırma `Bind` özelliği, eşleşen özellik adlarına sahip değerleri özel örneğe kopyalar. Options örneği artık bir işleve eklemek için IoC kapsayıcısında kullanılabilir.

Options nesnesi, genel `IOptions` arabirimin bir örneği olarak işlevine eklenir. Yapılandırmanızda bulunan değerlere erişmek için özelliğinikullanın.`Value`

```csharp
using System;
using Microsoft.Extensions.Options;

public class HttpTrigger
{
    private readonly MyOptions _settings;

    public HttpTrigger(IOptions<MyOptions> options)
    {
        _service = service;
        _settings = options.Value;
    }
}
```

Seçeneklerle çalışma hakkında daha fazla ayrıntı için [ASP.NET Core Içindeki seçenekler düzenine](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/options) bakın.

> [!WARNING]
> *Yerel. Settings. JSON* veya appSettings gibi dosyalardaki değerleri okumaya çalışmadan kaçının *. { Tüketim planında Environment}. JSON* . Barındırma altyapısının yapılandırma bilgilerine erişimi olmadığından, tetikleyici bağlantılarıyla ilgili bu dosyalardan okunan değerler uygulama ölçeklenirken kullanılamaz.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [İşlev uygulamanızı izleme](functions-monitoring.md)
- [İşlevler için en iyi uygulamalar](functions-best-practices.md)
