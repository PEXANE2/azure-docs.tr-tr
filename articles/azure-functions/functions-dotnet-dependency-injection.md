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
ms.date: 05/28/2019
ms.author: cshoe
ms.reviewer: jehollan
ms.openlocfilehash: 1dbbb6e7bd88e08520225515c422529dc260e1b2
ms.sourcegitcommit: b49431b29a53efaa5b82f9be0f8a714f668c38ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68377379"
---
# <a name="use-dependency-injection-in-net-azure-functions"></a>.NET Azure Işlevlerinde bağımlılık ekleme 'yi kullanma

Azure Işlevleri, sınıflar ve bunların bağımlılıkları arasında [denetimin (IOC) bir Iç sürümünü](https://docs.microsoft.com/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) elde etmek için bir tekniktir olan bağımlılık ekleme (dı) yazılım tasarım modelini destekler.

Azure Işlevleri ASP.NET Core bağımlılığı ekleme özelliklerinin üzerine oluşturulur. Azure Işlevleri uygulamasında dı özelliklerinin kullanılmadan önce, [ASP.NET Core bağımlılık ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) ' nin hizmetler, yaşam süreleri ve Tasarım desenlerinin farkında olması önerilir.

Bağımlılık ekleme desteği, Azure Işlevleri 2. x ile başlar.

## <a name="prerequisites"></a>Önkoşullar

Bağımlılık ekleme 'yi kullanabilmeniz için aşağıdaki NuGet paketlerini yüklemelisiniz:

- [Microsoft. Azure. Functions. uzantıları](https://www.nuget.org/packages/Microsoft.Azure.Functions.Extensions/)

- [Microsoft. net. SDK. Functions paket](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions/) sürümü 1.0.28 veya üzeri

- İsteğe bağlı: Yalnızca HttpClient 'ı başlangıçta kaydettirmek için [Microsoft. Extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) gereklidir

## <a name="register-services"></a>Hizmetleri Kaydet

Hizmetleri kaydetmek için bir `IFunctionsHostBuilder` örneğe bileşen yapılandırmak ve eklemek üzere bir yöntem oluşturabilirsiniz.  Azure işlevleri ana makinesi bir örneği oluşturur `IFunctionsHostBuilder` ve doğrudan kendi yönteğinize geçirir.

Yöntemi kaydetmek için başlangıç sırasında kullanılan tür `FunctionsStartup` adını belirten derleme özniteliğini ekleyin. Ayrıca kod, NuGet üzerindeki [Microsoft. Azure. Cosmos](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/) 'in bir ön sürümüne başvuruyorlar.

```csharp
using System;
using Microsoft.Azure.Functions.Extensions.DependencyInjection;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Http;
using Microsoft.Extensions.Logging;
using Microsoft.Azure.Cosmos;

[assembly: FunctionsStartup(typeof(MyNamespace.Startup))]

namespace MyNamespace
{
    public class Startup : FunctionsStartup
    {
        public override void Configure(IFunctionsHostBuilder builder)
        {
            builder.Services.AddHttpClient();
            builder.Services.AddSingleton((s) => {
                return new CosmosClient(Environment.GetEnvironmentVariable("COSMOSDB_CONNECTIONSTRING"));
            });
            builder.Services.AddSingleton<ILoggerProvider, MyLoggerProvider>();
        }
    }
}
```

## <a name="use-injected-dependencies"></a>Eklenen bağımlılıkları kullan

ASP.NET Core, bağımlılıklarınızı işleviniz için kullanılabilir hale getirmek için Oluşturucu ekleme işlemini kullanır. Aşağıdaki örnek, `IMyService` ve `HttpClient` bağımlılıklarının http ile tetiklenen bir işleve nasıl eklendiğini gösterir.

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;

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

Oluşturucu Ekleme kullanımı, bağımlılık ekleme özelliğinden faydalanmak istiyorsanız statik işlevler kullanmamalısınız.

## <a name="service-lifetimes"></a>Hizmet yaşam süreleri

Azure Işlevleri uygulamaları, [ASP.net bağımlılığı ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes): geçici, kapsamlı ve tek hizmet yaşam sürelerinin aynısını sunar.

Bir işlevler uygulamasında, kapsamlı hizmet ömrü bir işlev yürütme ömrüyle eşleşir. Kapsamlı hizmetler, her yürütme için bir kez oluşturulur. Yürütme sırasında o hizmetin sonraki istekleri, var olan hizmet örneğini yeniden kullanır. Tek bir hizmet ömrü, ana bilgisayar ömrüyle eşleşir ve bu örnekteki işlev Yürütmelerinde yeniden kullanılır.

Tek ömür Hizmetleri, bağlantılar ve istemciler için, örneğin `SqlConnection` `CloudBlobClient`veya `HttpClient` örnekler için önerilir.

GitHub üzerinde [farklı hizmet yaşam sürelerinin bir örneğini](https://aka.ms/functions/di-sample) görüntüleyin veya indirin.

## <a name="logging-services"></a>Günlüğe kaydetme hizmetleri

Kendi günlük sağlayıcınıza ihtiyacınız varsa, önerilen yol bir `ILoggerProvider` örneği kaydetmek olacaktır. Application Insights, Azure Işlevleri tarafından otomatik olarak eklenir.

> [!WARNING]
> Ortam tarafından sunulan `AddApplicationInsightsTelemetry()` hizmetlerle çakışan Hizmetleri kaydederken hizmetler koleksiyonuna eklemeyin.

## <a name="function-app-provided-services"></a>İşlev uygulaması tarafından sunulan hizmetler

İşlev Konağı birçok hizmeti kaydeder. Aşağıdaki hizmetler uygulamanızda bir bağımlılık olarak ele alınır:

|Hizmet türü|Süre|Açıklama|
|--|--|--|
|`Microsoft.Extensions.Configuration.IConfiguration`|Adet|Çalışma zamanı yapılandırması|
|`Microsoft.Azure.WebJobs.Host.Executors.IHostIdProvider`|Adet|Konak örneğinin KIMLIĞINI sağlamaktan sorumlu|

Bağımlılığı almak istediğiniz başka hizmetler varsa, [bir sorun oluşturun ve bunları GitHub 'da önerin](https://github.com/azure/azure-functions-host).

### <a name="overriding-host-services"></a>Konak hizmetlerini geçersiz kılma

Konak tarafından belirtilen geçersiz kılma Hizmetleri şu anda desteklenmiyor.  Geçersiz kılmak istediğiniz hizmetler varsa, [bir sorun oluşturun ve bunları GitHub 'da önerin](https://github.com/azure/azure-functions-host).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [İşlev uygulamanızı izleme](functions-monitoring.md)
- [İşlevler için en iyi uygulamalar](functions-best-practices.md)
