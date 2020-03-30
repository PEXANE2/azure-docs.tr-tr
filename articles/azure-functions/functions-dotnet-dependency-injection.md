---
title: .NET Azure İşlevleri'nde bağımlılık eklemeyi kullanma
description: .NET işlevlerinde hizmetleri kaydetmek ve kullanmak için bağımlılık enjeksiyonu nasıl kullanılacağını öğrenin
author: craigshoemaker
ms.topic: reference
ms.date: 09/05/2019
ms.author: cshoe
ms.reviewer: jehollan
ms.openlocfilehash: f8f1eb353087c5121eaafb4c8789e7a2f7638b99
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79475130"
---
# <a name="use-dependency-injection-in-net-azure-functions"></a>.NET Azure İşlevleri'nde bağımlılık eklemeyi kullanma

Azure İşlevler, sınıflar ve bunların bağımlılıkları arasında Denetim [Inversion (IoC)](https://docs.microsoft.com/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) elde etmek için bir teknik olan bağımlılık enjeksiyonu (DI) yazılım tasarım deseni destekler.

- Azure İşlevlerinde Bağımlılık enjeksiyonu .NET Çekirdek Bağımlılık Enjeksiyonu özellikleri üzerine kuruludur. [.NET Core bağımlılık enjeksiyonuna](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) aşinalık önerilir. Ancak, bağımlılıkları nasıl geçersiz kdığınız ve Tüketim planında Azure İşlevleri ile yapılandırma değerlerinin nasıl okunduğu konusunda farklılıklar vardır.

- Bağımlılık enjeksiyonu desteği Azure İşlevler 2.x ile başlar.

## <a name="prerequisites"></a>Ön koşullar

Bağımlılık enjeksiyonu kullanamadan önce aşağıdaki NuGet paketlerini yüklemeniz gerekir:

- [Microsoft.Azure.Functions.Uzantılar](https://www.nuget.org/packages/Microsoft.Azure.Functions.Extensions/)

- [Microsoft.NET.Sdk.Functions paketi](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions/) sürüm 1.0.28 veya sonrası

## <a name="register-services"></a>Kayıt hizmetleri

Hizmetleri kaydetmek için, bir örneği yapılandırmak ve `IFunctionsHostBuilder` bileşenler eklemek için bir yöntem oluşturun.  Azure İşlevler ana bilgisayarı `IFunctionsHostBuilder` bir örneğini oluşturur ve doğrudan yönteminize aktarAr.

Yöntemi kaydetmek için, `FunctionsStartup` başlangıç sırasında kullanılan tür adını belirten derleme özniteliğini ekleyin.

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

Çalışma zamanından önce ve sonra çalışan bir dizi kayıt adımı başlangıç sınıfını işler. Bu nedenle, aşağıdaki öğeleri unutmayın:

- *Başlangıç sınıfı yalnızca kurulum ve kayıt içindir.* Başlatma işlemi sırasında başlangıç sırasında kayıtlı hizmetleri kullanmaktan kaçının. Örneğin, başlangıç sırasında kaydedilmekte olan bir günlüğe kaydetmeye çalışmayın. Kayıt sürecinin bu noktası, hizmetlerinizin kullanıma sunulması için çok erkendir. `Configure` Yöntem çalıştırıladıktan sonra, İşlevler çalışma zamanı, hizmetlerinizin çalışma şeklini etkileyebilecek ek bağımlılıkları kaydetmeye devam eder.

- *Bağımlılık enjeksiyon konteyneri yalnızca açıkça kaydedilmiş türleri tutar.* Enjekte edilebilir türleri olarak kullanılabilen tek hizmetler `Configure` yöntemde kurulum ne lerdir. Sonuç olarak, kurulum sırasında `BindingContext` veya `ExecutionContext` enjekte edilebilir türler gibi işlevlere özgü türler kullanılamaz.

## <a name="use-injected-dependencies"></a>Enjekte edilmiş bağımlılıkları kullanma

Yapıcı enjeksiyon bağımlılıklarınızı bir işlevde kullanılabilir hale getirmek için kullanılır. Konstrüktör enjeksiyonu kullanımı statik sınıflar kullanmamanızı gerektirir.

Aşağıdaki örnek, `IMyService` bağımlılıkların `HttpClient` VE bağımlılıkların HTTP tarafından tetiklenen bir işleve nasıl enjekte edildiğini göstermektedir. Bu örnek, başlangıçta bir `HttpClient` kayıt için gereken [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) paketini kullanır.

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

## <a name="service-lifetimes"></a>Hizmet ömürleri

Azure İşlevler uygulamaları, [bağımlılık enjeksiyonu](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes)yla aynı hizmet kullanım ömürlerini ASP.NET. Bir Fonksiyonlar uygulaması için farklı hizmet ömürleri aşağıdaki gibi çalışır:

- **Geçici**: Geçici hizmetler, hizmetin her talebi üzerine oluşturulur.
- **Kapsamlı**: Kapsamlı hizmet ömrü, bir işlev yürütme ömrüyle eşleşir. Kapsamlı hizmetler yürütme başına bir kez oluşturulur. Yürütme sırasında bu hizmet için daha sonra istekleri varolan hizmet örneğini yeniden kullanın.
- **Singleton : Singleton**hizmet ömrü ana bilgisayar ömrüyle eşleşir ve bu örnekteki işlev yürütmeleri arasında yeniden kullanılır. Singleton yaşam boyu hizmetleri, örneğin `SqlConnection` bağlantılar ve `HttpClient` istemciler için önerilir.

GitHub'da [farklı hizmet kullanım sürelerinden](https://aka.ms/functions/di-sample) bir örnek görüntüleyin veya indirin.

## <a name="logging-services"></a>Günlük hizmetleri

Kendi günlük sağlayıcınıza ihtiyacınız varsa, özel `ILoggerProvider` bir türü örnek olarak kaydedin. Uygulama Öngörüleri Azure İşlevleri tarafından otomatik olarak eklenir.

> [!WARNING]
> - Çevre tarafından `AddApplicationInsightsTelemetry()` sağlanan hizmetlerle çakışan hizmetleri kaydederken hizmet koleksiyonuna eklemeyin.
> - Kendi `TelemetryConfiguration` bilgilerinizi veya `TelemetryClient` yerleşik Application Insights işlevini kullanıyorsanız kaydetmeyin. Kendi `TelemetryClient` örneğini yapılandırmanız gerekiyorsa, Azure `TelemetryConfiguration` [İşlerini İzle'de](./functions-monitoring.md#version-2x-and-later-2)gösterildiği gibi enjekte edilen yoluyla bir tane oluşturun.

### <a name="iloggert-and-iloggerfactory"></a>ILogger<T> ve ILoggerFactory

Ev sahibi, `ILogger<T>` `ILoggerFactory` yapıcılara enjekte edecek ve hizmetleri sunacak.  Ancak, varsayılan olarak bu yeni günlük filtreleri işlev günlükleri dışarı filtrelenir.  Ek filtreleri ve `host.json` kategorileri devre dışı bırakmak için dosyayı değiştirmeniz gerekir.  Aşağıdaki örnek, ana `ILogger<HttpTrigger>` bilgisayar tarafından ortaya çıkacak günlükleri ile bir ekleme gösterir.

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

Ve `host.json` günlük filtresi ni ekleyen bir dosya.

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

## <a name="function-app-provided-services"></a>Fonksiyon uygulaması sağlanan hizmetler

İşlev ana bilgisayar birçok hizmeti kaydeder. Aşağıdaki hizmetleri uygulamanızda bir bağımlılık olarak almak güvenlidir:

|Hizmet Türü|Ömür|Açıklama|
|--|--|--|
|`Microsoft.Extensions.Configuration.IConfiguration`|Singleton|Çalışma zamanı yapılandırması|
|`Microsoft.Azure.WebJobs.Host.Executors.IHostIdProvider`|Singleton|Ev sahibi örneğinkimliğini sağlamaktan sorumlu|

Bağımlılık yapmak istediğiniz başka hizmetler varsa, [bir sorun oluşturun ve bunları GitHub'da önerin.](https://github.com/azure/azure-functions-host)

### <a name="overriding-host-services"></a>Ana bilgisayar hizmetlerinin geçersiz kılınması

Ana bilgisayar tarafından sağlanan geçersiz kılma hizmetleri şu anda desteklenmez.  Geçersiz kılmak istediğiniz hizmetler varsa, [bir sorun oluşturun ve bunları GitHub'da önerin.](https://github.com/azure/azure-functions-host)

## <a name="working-with-options-and-settings"></a>Seçenekler ve ayarlarla çalışma

[Uygulama ayarlarında](./functions-how-to-use-azure-function-app-settings.md#settings) tanımlanan değerler, `IConfiguration` başlangıç sınıfındauygulama ayarları değerlerini okumanızı sağlayan bir örnekte kullanılabilir.

`IConfiguration` Örnekteki değerleri özel bir türe ayıklayabilirsiniz. Uygulama ayarları değerlerini özel bir türe kopyalamak, bu değerleri enjekte edilebilir hale getirerek hizmetlerinizi test etmeyi kolaylaştırır. Yapılandırma örneğinde okunan ayarlar basit anahtar/değer çiftleri olmalıdır.

Uygulama ayarı ile tutarlı bir özellik içeren aşağıdaki sınıfı düşünün:

```csharp
public class MyOptions
{
    public string MyCustomSetting { get; set; }
}
```

Ve `local.settings.json` özel ayarı aşağıdaki gibi yapılandırabilecek bir dosya:
```json
{
  "IsEncrypted": false,
  "Values": {
    "MyOptions:MyCustomSetting": "Foobar"
  }
}
```

Yöntemin `Startup.Configure` içinden, aşağıdaki kodu `IConfiguration` kullanarak örnekteki değerleri özel türünüze ayıklayabilirsiniz:

```csharp
builder.Services.AddOptions<MyOptions>()
                .Configure<IConfiguration>((settings, configuration) =>
                                           {
                                                configuration.GetSection("MyOptions").Bind(settings);
                                           });
```

Yapılandırmadan özel örnek için eşleşen özellik adlarına sahip kopyaları çağırma. `Bind` Seçenekler örneği artık IoC kapsayıcısında bir işlevin içine enjekte edilebilebilir.

Seçenekler nesnesi genel `IOptions` arabirimin bir örneği olarak işleviçine enjekte edilir. Yapılandırmanızda `Value` bulunan değerlere erişmek için özelliği kullanın.

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

Seçeneklerle çalışma yla ilgili daha fazla bilgi için [ASP.NET Core'daki Seçenekler desenine](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/options) bakın.

> [!WARNING]
> *Local.settings.json* veya appsettings gibi dosyalardaki değerleri okumaya çalışmaktan *kaçının.{ çevre}.json* Tüketim planı üzerinde. Barındırma altyapısı yapılandırma bilgilerine erişimi olmadığından, tetikleyici bağlantılarla ilgili bu dosyalardan okunan değerler uygulama ölçeklendirildikçe kullanılamaz.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [İşlev uygulamanızı izleme](functions-monitoring.md)
- [Fonksiyonlar için en iyi uygulamalar](functions-best-practices.md)
