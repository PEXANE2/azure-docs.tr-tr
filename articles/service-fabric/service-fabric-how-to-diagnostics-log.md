---
title: Bir .NET uygulamasından günlük olayları oluşturma
description: Bir Azure kümesinde veya tek başına kümede barındırılan .NET Service Fabric uygulamanıza günlük ekleme hakkında bilgi edinin.
author: srrengar
ms.topic: conceptual
ms.date: 03/27/2018
ms.author: srrengar
ms.openlocfilehash: 8c4721584e74bd7f7111c516f2d16bd190392bb5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75614375"
---
# <a name="add-logging-to-your-service-fabric-application"></a>Service Fabric uygulamanıza günlük kaydı ekleme

Başvurunuz, sorunlar ortaya çıktığında adli olarak hata ayıklamak için yeterli bilgi sağlamalıdır. Günlük, Servis Kumaşı uygulamanıza ekleyebileceğiniz en önemli şeylerden biridir. Bir hata oluştuğunda, iyi günlüğe kaydetme hataları araştırmak için bir yol verebilir. Günlük desenlerini analiz ederek, uygulamanızın performansını veya tasarımını geliştirmenin yollarını bulabilirsiniz. Bu belge birkaç farklı günlüğe kaydetme seçeneğini gösterir.

## <a name="eventflow"></a>Olay Akışı

[EventFlow kitaplık](https://github.com/Azure/diagnostics-eventflow) paketi, uygulamaların hangi tanılama verilerinin toplandığını ve bunların nereden çıkarılanacağını tanımlamasına olanak tanır. Tanılama verileri performans sayaçlarından uygulama izlemelerine kadar her şey olabilir. Uygulamayla aynı işlemde çalıştığından, iletişim yükü en aza indirilir. EventFlow ve Hizmet Kumaşı hakkında daha fazla bilgi [için, EventFlow ile Azure Hizmet Kumaşı Olay Toplama](service-fabric-diagnostics-event-aggregation-eventflow.md)konusuna bakın.

### <a name="using-structured-eventsource-events"></a>Yapılandırılmış EventSource olaylarını kullanma

İleti olaylarını kullanım örneğine göre tanımlamak, olay bağlamında olayla ilgili verileri paketlemenize olanak tanır. Belirtilen olay özelliklerinin adlarını veya değerlerini temel alınarak daha kolay arama yapabilir ve filtreleyebilirsiniz. Enstrümantasyon çıktısının yapılandırılması okunmasını kolaylaştırır, ancak her kullanım örneği için bir olayı tanımlamak için daha fazla düşünce ve zaman gerektirir. 

Bazı olay tanımları tüm uygulama boyunca paylaşılabilir. Örneğin, bir yöntem başlatma veya durdurma olayı, bir uygulama içindeki birçok hizmette yeniden kullanılır. Sipariş sistemi gibi etki alanına özgü bir hizmetin, kendine özgü bir etkinliği olan bir **CreateOrder** etkinliği olabilir. Bu yaklaşım birçok olay oluşturabilir ve proje ekipleri arasında tanımlayıcıların eşgüdümlenmesini gerektirebilir. 

```csharp
[EventSource(Name = "MyCompany-VotingState-VotingStateService")]
internal sealed class ServiceEventSource : EventSource
{
    public static readonly ServiceEventSource Current = new ServiceEventSource();

    // The instance constructor is private to enforce singleton semantics.
    private ServiceEventSource() : base() { }

    ...

    // The ServiceTypeRegistered event contains a unique identifier, an event attribute that defined the event, and the code implementation of the event.
    private const int ServiceTypeRegisteredEventId = 3;
    [Event(ServiceTypeRegisteredEventId, Level = EventLevel.Informational, Message = "Service host process {0} registered service type {1}", Keywords = Keywords.ServiceInitialization)]
    public void ServiceTypeRegistered(int hostProcessId, string serviceType)
    {
        WriteEvent(ServiceTypeRegisteredEventId, hostProcessId, serviceType);
    }

    // The ServiceHostInitializationFailed event contains a unique identifier, an event attribute that defined the event, and the code implementation of the event.
    private const int ServiceHostInitializationFailedEventId = 4;
    [Event(ServiceHostInitializationFailedEventId, Level = EventLevel.Error, Message = "Service host initialization failed", Keywords = Keywords.ServiceInitialization)]
    public void ServiceHostInitializationFailed(string exception)
    {
        WriteEvent(ServiceHostInitializationFailedEventId, exception);
    }

    ...

```

### <a name="using-eventsource-generically"></a>EventSource'u genel olarak kullanma

Belirli olayları tanımlamak zor olabileceğinden, birçok kişi genellikle bilgilerini bir dize olarak çıktı parametreleri ortak bir dizi ile birkaç olay tanımlar. Yapılandırılmış yönün çoğu kaybolur ve sonuçları aramak ve filtrelemek daha zordur. Bu yaklaşımda, genellikle günlük düzeylerine karşılık gelen birkaç olay tanımlanır. Aşağıdaki snippet hata ayıklama ve hata iletisi tanımlar:

```csharp
[EventSource(Name = "MyCompany-VotingState-VotingStateService")]
internal sealed class ServiceEventSource : EventSource
{
    public static readonly ServiceEventSource Current = new ServiceEventSource();

    // The Instance constructor is private, to enforce singleton semantics.
    private ServiceEventSource() : base() { }

    ...

    private const int DebugEventId = 10;
    [Event(DebugEventId, Level = EventLevel.Verbose, Message = "{0}")]
    public void Debug(string msg)
    {
        WriteEvent(DebugEventId, msg);
    }

    private const int ErrorEventId = 11;
    [Event(ErrorEventId, Level = EventLevel.Error, Message = "Error: {0} - {1}")]
    public void Error(string error, string msg)
    {
        WriteEvent(ErrorEventId, error, msg);
    }

    ...

```

Yapılandırılmış ve jenerik enstrümantasyon bir melez kullanarak da iyi çalışabilir. Yapılandırılmış enstrümantasyon hataları ve ölçümleri raporlamak için kullanılır. Genel olaylar, mühendisler tarafından sorun giderme için tüketilen ayrıntılı günlüğe kaydetme için kullanılabilir.

## <a name="microsoftextensionslogging"></a>Microsoft.Extensions.Logging

ASP.NET Core günlük[(Microsoft.Extensions.Logging NuGet paketi),](https://www.nuget.org/packages/Microsoft.Extensions.Logging)uygulamanız için standart bir günlük API'sı sağlayan bir günlük çerçevesidir. Diğer günlük arka uçları için destek ASP.NET Core günlük takılabilir. Bu, çok fazla kod değiştirmek zorunda kalmadan, uygulamanızda günlüğe kaydetme nin işlendiği için çok çeşitli destek sağlar.

1. **Microsoft.Extensions.Logging** NuGet paketini enstrüman istediğiniz projeye ekleyin. Ayrıca, herhangi bir sağlayıcı paketleri ekleyin. Daha fazla bilgi için ASP.NET [Core'da Günlüğe Kaydetme'ye](https://docs.microsoft.com/aspnet/core/fundamentals/logging)bakın.
2. **Microsoft.Extensions.Logging** için hizmet dosyanıza **bir kullanım** yönergesi ekleyin.
3. Hizmet sınıfınızda özel bir değişken tanımlayın.

   ```csharp
   private ILogger _logger = null;
   ```

4. Hizmet sınıfınızın oluşturucusuna şu kodu ekleyin:

   ```csharp
   _logger = new LoggerFactory().CreateLogger<Stateless>();
   ```

5. Kodunuzu yöntemlerinizde enstrümanting başlayın. İşte birkaç örnek:

   ```csharp
   _logger.LogDebug("Debug-level event from Microsoft.Logging");
   _logger.LogInformation("Informational-level event from Microsoft.Logging");

   // In this variant, we're adding structured properties RequestName and Duration, which have values MyRequest and the duration of the request.
   // Later in the article, we discuss why this step is useful.
   _logger.LogInformation("{RequestName} {Duration}", "MyRequest", requestDuration);
   ```

### <a name="using-other-logging-providers"></a>Diğer günlük sağlayıcılarını kullanma

Bazı üçüncü taraf [sağlayıcılar, Serilog,](https://serilog.net/) [NLog](https://nlog-project.org/)ve [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging)dahil olmak üzere önceki bölümde açıklanan yaklaşımı kullanır. Bunların her birini ASP.NET Core günlüğe takabilir veya ayrı ayrı kullanabilirsiniz. Serilog' un bir logger' dan gönderilen tüm iletileri zenginleyen bir özelliği vardır. Bu özellik, hizmet adı, türü ve bölüm bilgilerini çıktıalmak için yararlı olabilir. Bu özelliği ASP.NET Çekirdek altyapısında kullanmak için aşağıdaki adımları yapın:

1. **Serilog**ekleyin , **Serilog.Extensions.Logging**, **Serilog.Sinks.Literate**, ve **Serilog.Sinks.Observable** NuGet paketleri projeye. 
2. Bir `LoggerConfiguration` ve logger örneği oluşturun.

   ```csharp
   Log.Logger = new LoggerConfiguration().WriteTo.LiterateConsole().CreateLogger();
   ```

3. Hizmet `Serilog.ILogger` oluşturucuya bir bağımsız değişken ekleyin ve yeni oluşturulan kaydediciyi geçirin.

   ```csharp
   ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
   ```

4. Hizmet oluşturucu olarak, **ServiceTypeName**, **ServiceName**, **PartitionId**ve **InstanceId**için özellik zenginleştirme oluşturur.

   ```csharp
   public Stateless(StatelessServiceContext context, Serilog.ILogger serilog)
       : base(context)
   {
       PropertyEnricher[] properties = new PropertyEnricher[]
       {
           new PropertyEnricher("ServiceTypeName", context.ServiceTypeName),
           new PropertyEnricher("ServiceName", context.ServiceName),
           new PropertyEnricher("PartitionId", context.PartitionId),
           new PropertyEnricher("InstanceId", context.ReplicaOrInstanceId),
       };

       serilog.ForContext(properties);

       _logger = new LoggerFactory().AddSerilog(serilog.ForContext(properties)).CreateLogger<Stateless>();
   }
   ```

5. Kodu, Serilog'suz ASP.NET Core kullanıyormuş gibi enstrümanlayın.

   >[!NOTE]
   >Statik cihazı önceki `Log.Logger` örnekte *kullanmamanızı* öneririz. Service Fabric, tek bir işlem içinde aynı hizmet türünden birden çok örneği barındırabilir. Statik `Log.Logger`kullanırsanız, özellik zenginleştiricilerin son yazarı çalışan tüm örnekler için değerleri gösterir. Bu, _logger değişkeninin hizmet sınıfının özel bir üye değişkeni olmasının nedenlerinden biridir. Ayrıca, hizmetler arasında `_logger` kullanılabilecek ortak kod için kullanılabilir hale getirmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Service Fabric'de uygulama izleme](service-fabric-diagnostics-event-generation-app.md)hakkında daha fazla bilgi edinin.
- [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) ve Windows [Azure Tanılama](service-fabric-diagnostics-event-aggregation-wad.md)ile günlüğe kaydetme hakkında bilgi edinin.










