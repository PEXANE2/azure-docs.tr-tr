---
title: .NET Azure İşlevleri'nde bağımlılık eklemeyi kullanma
description: .NET işlevlerinde Hizmetleri kaydetmek ve kullanmak için bağımlılık ekleme eklemeyi nasıl kullanacağınızı öğrenin
author: craigshoemaker
ms.topic: reference
ms.date: 09/05/2019
ms.author: cshoe
ms.reviewer: jehollan
ms.openlocfilehash: a1ff8e0aedce5d3a6acc9a39084cf0839efdd88e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81678450"
---
# <a name="use-dependency-injection-in-net-azure-functions"></a>.NET Azure İşlevleri'nde bağımlılık eklemeyi kullanma

Azure Işlevleri, sınıflar ve bunların bağımlılıkları arasında [denetimin (IOC) bir Iç sürümünü](https://docs.microsoft.com/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) elde etmek için bir tekniktir olan bağımlılık ekleme (dı) yazılım tasarım modelini destekler.

- Azure Işlevlerine bağımlılık ekleme, .NET Core bağımlılığı ekleme özellikleri üzerine kurulmuştur. [.NET Core bağımlılığı ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) konusunda benzerlik yapmanız önerilir. Bağımlılıkları geçersiz kılma ve yapılandırma değerlerinin tüketim planında Azure Işlevleri ile nasıl okunduğu konusunda farklılıklar vardır.

- Bağımlılık ekleme desteği, Azure Işlevleri 2. x ile başlar.

## <a name="prerequisites"></a>Ön koşullar

Bağımlılık ekleme 'yi kullanabilmeniz için aşağıdaki NuGet paketlerini yüklemelisiniz:

- [Microsoft. Azure. Functions. uzantıları](https://www.nuget.org/packages/Microsoft.Azure.Functions.Extensions/)

- [Microsoft. net. SDK. Functions paket](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions/) sürümü 1.0.28 veya üzeri

## <a name="register-services"></a>Hizmetleri Kaydet

Hizmetleri kaydetmek için bir `IFunctionsHostBuilder` örneğe bileşen yapılandırmak ve eklemek üzere bir yöntem oluşturun.  Azure Işlevleri ana makinesi bir örneği oluşturur `IFunctionsHostBuilder` ve doğrudan kendi yönteğinize geçirir.

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

Aşağıdaki örnek, `IMyService` ve `HttpClient` bağımlılıklarının http ile tetiklenen bir işleve nasıl eklendiğini gösterir. Bu örnek, bir `HttpClient` başlangıç kaydı Için gereken [Microsoft. Extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) paketini kullanır.

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;
using System.Net.Http;

namespace MyNamespace
{
    public class HttpTrigger
    {
        private readonly IMyService _service;
        private readonly HttpClient _client;

        public HttpTrigger(IMyService service, HttpClient httpClient)
        {
            _service = service;
            _client = httpClient;
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

- **Geçici**: hizmet istekleri üzerine geçici hizmetler oluşturulur.
- **Kapsam**: kapsamlı hizmet ömrü bir işlev yürütme ömrü ile eşleşir. Kapsamlı hizmetler, her yürütme için bir kez oluşturulur. Yürütme sırasında o hizmetin sonraki istekleri, var olan hizmet örneğini yeniden kullanır.
- Tekil **: tek**hizmet ömrü, ana bilgisayar ömrü ile eşleşir ve bu örnekteki işlev Yürütmelerinde yeniden kullanılır. Tek ömür Hizmetleri, bağlantılar ve istemciler için, örneğin `SqlConnection` veya `HttpClient` örnekler için önerilir.

GitHub üzerinde [farklı hizmet yaşam sürelerinin bir örneğini](https://aka.ms/functions/di-sample) görüntüleyin veya indirin.

## <a name="logging-services"></a>Günlüğe kaydetme hizmetleri

Kendi günlük sağlayıcınıza ihtiyacınız varsa, özel bir türü `ILoggerProvider` örnek olarak kaydedin. Application Insights, Azure Işlevleri tarafından otomatik olarak eklenir.

> [!WARNING]
> - Ortam tarafından sunulan `AddApplicationInsightsTelemetry()` hizmetlerle çakışan Hizmetleri kaydederken hizmetler koleksiyonuna eklemeyin.
> - Kendi kendinize `TelemetryConfiguration` kaydolmayın veya `TelemetryClient` yerleşik Application Insights işlevlerini kullanıyorsanız. Kendi `TelemetryClient` örneğinizi yapılandırmanız gerekirse, `TelemetryConfiguration` [Azure işlevlerini izle](./functions-monitoring.md#version-2x-and-later-2)bölümünde gösterildiği gibi eklenmiş şekilde bir tane oluşturun.

### <a name="iloggert-and-iloggerfactory"></a>ILogger<T> ve ıloggerfactory

Konak, kurucularına `ILogger<T>` ve `ILoggerFactory` hizmet olarak eklenecek.  Bununla birlikte, varsayılan olarak bu yeni günlük filtreleri işlev günlüklerinden filtrelenecektir.  Ek filtreleri ve kategorileri kabul etmek `host.json` için dosyayı değiştirmeniz gerekir.  Aşağıdaki örnek, ana bilgisayar tarafından `ILogger<HttpTrigger>` açığa çıkarılan bir Günlükler eklemeyi gösterir.

```csharp
namespace MyNamespace
{
    public class HttpTrigger
    {
        private readonly ILogger<HttpTrigger> _log;

        public HttpTrigger(ILogger<HttpTrigger> log)
        {
            _log = log;
        }

        [FunctionName("HttpTrigger")]
        public async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)] HttpRequest req)
        {
            _log.LogInformation("C# HTTP trigger function processed a request.");

            // ...
    }
}
```

Ve günlük `host.json` filtresini ekleyen bir dosya.

```json
{
    "version": "2.0",
    "logging": {
        "applicationInsights": {
            "samplingExcludedTypes": "Request",
            "samplingSettings": {
                "isEnabled": true
            }
        },
        "logLevel": {
            "MyNamespace.HttpTrigger": "Information"
        }
    }
}
```

## <a name="function-app-provided-services"></a>İşlev uygulaması tarafından sunulan hizmetler

İşlev Konağı birçok hizmeti kaydeder. Aşağıdaki hizmetler uygulamanızda bir bağımlılık olarak ele alınır:

|Hizmet Türü|Ömür|Açıklama|
|--|--|--|
|`Microsoft.Extensions.Configuration.IConfiguration`|Adet|Çalışma zamanı yapılandırması|
|`Microsoft.Azure.WebJobs.Host.Executors.IHostIdProvider`|Adet|Konak örneğinin KIMLIĞINI sağlamaktan sorumlu|

Bağımlılığı almak istediğiniz başka hizmetler varsa, [bir sorun oluşturun ve bunları GitHub 'da önerin](https://github.com/azure/azure-functions-host).

### <a name="overriding-host-services"></a>Konak hizmetlerini geçersiz kılma

Konak tarafından belirtilen geçersiz kılma Hizmetleri şu anda desteklenmiyor.  Geçersiz kılmak istediğiniz hizmetler varsa, [bir sorun oluşturun ve bunları GitHub 'da önerin](https://github.com/azure/azure-functions-host).

## <a name="working-with-options-and-settings"></a>Seçeneklerle ve ayarlarla çalışma

[Uygulama ayarlarında](./functions-how-to-use-azure-function-app-settings.md#settings) tanımlanan değerler, başlangıç sınıfındaki uygulama ayarları `IConfiguration` değerlerini okumanızı sağlayan bir örnekte kullanılabilir.

`IConfiguration` Örnekten değerleri özel bir türe ayıklayabilirsiniz. Uygulama ayarları değerlerini özel bir türe kopyalamak, bu değerleri tablo haline getirerek, hizmetlerinizi test etmelerini kolaylaştırır. Yapılandırma örneğine okunan ayarların basit anahtar/değer çiftleri olması gerekir.

Uygulama ayarıyla tutarlı adlı bir özellik içeren aşağıdaki sınıfı göz önünde bulundurun:

```csharp
public class MyOptions
{
    public string MyCustomSetting { get; set; }
}
```

Ve özel `local.settings.json` ayarı aşağıdaki gibi yapılandırabilecek bir dosya:
```json
{
  "IsEncrypted": false,
  "Values": {
    "MyOptions:MyCustomSetting": "Foobar"
  }
}
```

`Startup.Configure` Yöntemi içinde, aşağıdaki kodu kullanarak `IConfiguration` örnekten değerleri özel türüne ayıklayabilirsiniz:

```csharp
builder.Services.AddOptions<MyOptions>()
                .Configure<IConfiguration>((settings, configuration) =>
                                           {
                                                configuration.GetSection("MyOptions").Bind(settings);
                                           });
```

Çağırma `Bind` özelliği, eşleşen özellik adlarına sahip değerleri özel örneğe kopyalar. Options örneği artık bir işleve eklemek için IoC kapsayıcısında kullanılabilir.

Options nesnesi, genel `IOptions` arabirimin bir örneği olarak işlevine eklenir. Yapılandırmanızda bulunan `Value` değerlere erişmek için özelliğini kullanın.

```csharp
using System;
using Microsoft.Extensions.Options;

public class HttpTrigger
{
    private readonly MyOptions _settings;

    public HttpTrigger(IOptions<MyOptions> options)
    {
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
