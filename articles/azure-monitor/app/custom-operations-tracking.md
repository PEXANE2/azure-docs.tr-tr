---
title: Azure Application Insights .NET SDK ile özel işlemleri izleme
description: Azure Application Insights .NET SDK ile özel işlemleri izleme
ms.topic: conceptual
ms.date: 11/26/2019
ms.reviewer: sergkanz
ms.openlocfilehash: 31c1fb366e7b109ea1fa4977d8e2f908e766e0f2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79276107"
---
# <a name="track-custom-operations-with-application-insights-net-sdk"></a>Uygulama Öngörüleri .NET SDK ile özel işlemleri izleyin

Azure Uygulama Öngörüleri SDK'ları gelen HTTP isteklerini ve çağrıları HTTP istekleri ve SQL sorguları gibi bağımlı hizmetlere otomatik olarak izler. İstek lerin ve bağımlılıkların izlenmesi ve korelasyonu, bu uygulamayı birleştiren tüm mikro hizmetlerde tüm uygulamanın yanıt verme hızı ve güvenilirliğinde görünürlük verir. 

Genel olarak desteklenemez bir uygulama desenleri sınıfı vardır. Bu tür desenlerin doğru izlenmesi, manuel kod enstrümantasyonu gerektirir. Bu makalede, özel sıra işleme ve uzun süren arka plan görevleri çalıştırma gibi el ile enstrümantasyon gerektirebilecek birkaç desen kapsar.

Bu belge, Uygulama Öngörüleri SDK ile özel işlemlerin nasıl izlenebildiğini anlatan kılavuz sağlar. Bu dokümantasyon şu şekilde dir:

- .NET (Base SDK olarak da bilinir) sürüm 2.4+ için Uygulama Öngörüleri.
- Web uygulamaları (ASP.NET çalıştıran) sürüm 2.4+ için Uygulama Öngörüleri.
- ASP.NET Core sürüm 2.1+ için Uygulama Öngörüleri.

## <a name="overview"></a>Genel Bakış
İşlem, bir uygulama tarafından çalıştırılanan mantıksal bir çalışma dır. Bir adı, başlangıç zamanı, süresi, sonucu ve kullanıcı adı, özellikleri ve sonuç gibi yürütme bağlamı vardır. A işlemi B işlemi tarafından başlatıldıysa, B işlemi A için bir üst öğe olarak ayarlanır. Bir işlemin yalnızca bir üst öğesi olabilir, ancak birçok alt işlem olabilir. İşlemler ve telemetri korelasyonları hakkında daha fazla bilgi için Azure [Uygulama İstatistikleri telemetri semetor korelasyonuna](correlation.md)bakın.

Application Insights .NET SDK'da işlem, soyut sınıf [OperationTelemetry](https://github.com/microsoft/ApplicationInsights-dotnet/blob/7633ae849edc826a8547745b6bf9f3174715d4bd/BASE/src/Microsoft.ApplicationInsights/Extensibility/Implementation/OperationTelemetry.cs) ve onun torunları [RequestTelemetry](https://github.com/microsoft/ApplicationInsights-dotnet/blob/7633ae849edc826a8547745b6bf9f3174715d4bd/BASE/src/Microsoft.ApplicationInsights/DataContracts/RequestTelemetry.cs) ve [DependencyTelemetry](https://github.com/microsoft/ApplicationInsights-dotnet/blob/7633ae849edc826a8547745b6bf9f3174715d4bd/BASE/src/Microsoft.ApplicationInsights/DataContracts/DependencyTelemetry.cs)tarafından açıklanmıştır.

## <a name="incoming-operations-tracking"></a>Gelen operasyon izleme 
Application Insights web SDK, IIS boru hattında çalışan ASP.NET uygulamalar ve tüm ASP.NET Core uygulamaları için HTTP isteklerini otomatik olarak toplar. Diğer platformlar ve çerçeveler için topluluk destekli çözümler vardır. Ancak, uygulama standart veya topluluk destekli çözümlerden herhangi biri tarafından desteklenmiyorsa, uygulama el ile alet edebilirsiniz.

Özel izleme gerektiren başka bir örnek, kuyruktan öğeleri alan alt öğedir. Bazı kuyruklar için, bu kuyruğa ileti ekleme çağrısı bağımlılık olarak izlenir. Ancak, ileti işlemeaçıklayan üst düzey işlem otomatik olarak toplanmaz.

Bakalım bu tür işlemler nasıl izlenebilir.

Yüksek düzeyde, görev oluşturmak `RequestTelemetry` ve bilinen özellikleri ayarlamaktır. İşlem bittikten sonra, telemetriyi izlersiniz. Aşağıdaki örnek, bu görevi göstermektedir.

### <a name="http-request-in-owin-self-hosted-app"></a>Owin kendi kendine barındırılan uygulamada HTTP isteği
Bu örnekte, izleme bağlamı [Korelasyon için HTTP Protokolü'ne](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md)göre yayılır. Orada açıklanan üstbilgi almayı beklemelisiniz.

```csharp
public class ApplicationInsightsMiddleware : OwinMiddleware
{
    // you may create a new TelemetryConfiguration instance, reuse one you already have
    // or fetch the instance created by Application Insights SDK.
    private readonly TelemetryConfiguration telemetryConfiguration = TelemetryConfiguration.CreateDefault();
    private readonly TelemetryClient telemetryClient = new TelemetryClient(telemetryConfiguration);
    
    public ApplicationInsightsMiddleware(OwinMiddleware next) : base(next) {}

    public override async Task Invoke(IOwinContext context)
    {
        // Let's create and start RequestTelemetry.
        var requestTelemetry = new RequestTelemetry
        {
            Name = $"{context.Request.Method} {context.Request.Uri.GetLeftPart(UriPartial.Path)}"
        };

        // If there is a Request-Id received from the upstream service, set the telemetry context accordingly.
        if (context.Request.Headers.ContainsKey("Request-Id"))
        {
            var requestId = context.Request.Headers.Get("Request-Id");
            // Get the operation ID from the Request-Id (if you follow the HTTP Protocol for Correlation).
            requestTelemetry.Context.Operation.Id = GetOperationId(requestId);
            requestTelemetry.Context.Operation.ParentId = requestId;
        }

        // StartOperation is a helper method that allows correlation of 
        // current operations with nested operations/telemetry
        // and initializes start time and duration on telemetry items.
        var operation = telemetryClient.StartOperation(requestTelemetry);

        // Process the request.
        try
        {
            await Next.Invoke(context);
        }
        catch (Exception e)
        {
            requestTelemetry.Success = false;
            telemetryClient.TrackException(e);
            throw;
        }
        finally
        {
            // Update status code and success as appropriate.
            if (context.Response != null)
            {
                requestTelemetry.ResponseCode = context.Response.StatusCode.ToString();
                requestTelemetry.Success = context.Response.StatusCode >= 200 && context.Response.StatusCode <= 299;
            }
            else
            {
                requestTelemetry.Success = false;
            }

            // Now it's time to stop the operation (and track telemetry).
            telemetryClient.StopOperation(operation);
        }
    }
    
    public static string GetOperationId(string id)
    {
        // Returns the root ID from the '|' to the first '.' if any.
        int rootEnd = id.IndexOf('.');
        if (rootEnd < 0)
            rootEnd = id.Length;

        int rootStart = id[0] == '|' ? 1 : 0;
        return id.Substring(rootStart, rootEnd - rootStart);
    }
}
```

HTTP Korelasyon Protokolü de `Correlation-Context` üstbilgi bildirir. Ancak, burada basitlik için atlanır.

## <a name="queue-instrumentation"></a>Sıra enstrümantasyonu
[Bağıntılılık](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md) ayrıntılarını HTTP isteğiyle geçirmek için [W3C İzleme Bağlamı](https://www.w3.org/TR/trace-context/) ve HTTP Protokolü olsa da, her sıra protokolü, sıra iletisi boyunca aynı ayrıntıların nasıl geçirildiğini tanımlamalı. Bazı sıra protokolleri (AMQP gibi) ek meta verilerin geçirilmesine izin verir ve bazılarının (Azure Depolama Sırası gibi) bağlamın ileti yüküne kodlanmasını gerektirir.

> [!NOTE]
> * **Çapraz bileşen izleme henüz kuyruklar için desteklenmez** HTTP ile, üreticiniz ve tüketiciniz farklı Application Insights kaynaklarına telemetri gönderirse, İşlem Tanılama Deneyimi ve Uygulama Haritası işlemleri ve haritauçtan uca gösterir. Kuyrukdurumunda bu henüz desteklenmez. 

### <a name="service-bus-queue"></a>Service Bus Kuyruğu
Application Insights, .NET sürüm 3.0.0 ve üzeri sürümler için yeni [Microsoft Azure ServiceBus İstemci](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus/) ile Servis Veri Servisi Mesajlaşma çağrılarını izler.
İletileri işlemek için [ileti işleyicisi deseni](/dotnet/api/microsoft.azure.servicebus.queueclient.registermessagehandler) kullanırsanız, işlem yapılır: hizmetiniz tarafından yapılan tüm Servis Veri Yolu çağrıları otomatik olarak izlenir ve diğer telemetri öğeleriyle ilişkilendirilir. İletileri el ile [işliyorsanız, Microsoft Application Insights ile Birlikte Hizmet Veri Servisi istemcisinin takibine](../../service-bus-messaging/service-bus-end-to-end-tracing.md) bakın.

[WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) paketini kullanıyorsanız, daha fazla bilgi edinin - Hizmet Veri Yolu kuyruğu AMQP protokolünü kullandığından ve Application Insights kuyruk işlemlerini otomatik olarak izlemediği için Servis Veri Yolu'na yapılan çağrıları nasıl izleyip ilişkilendireceğimizi aşağıdaki örnekler gösterilmektedir.
İleti özelliklerinde korelasyon tanımlayıcıları aktarılır.

#### <a name="enqueue"></a>Enqueue

```csharp
public async Task Enqueue(string payload)
{
    // StartOperation is a helper method that initializes the telemetry item
    // and allows correlation of this operation with its parent and children.
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("enqueue " + queueName);
    
    operation.Telemetry.Type = "Azure Service Bus";
    operation.Telemetry.Data = "Enqueue " + queueName;

    var message = new BrokeredMessage(payload);
    // Service Bus queue allows the property bag to pass along with the message.
    // We will use them to pass our correlation identifiers (and other context)
    // to the consumer.
    message.Properties.Add("ParentId", operation.Telemetry.Id);
    message.Properties.Add("RootId", operation.Telemetry.Context.Operation.Id);

    try
    {
        await queue.SendAsync(message);
        
        // Set operation.Telemetry Success and ResponseCode here.
        operation.Telemetry.Success = true;
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        // Set operation.Telemetry Success and ResponseCode here.
        operation.Telemetry.Success = false;
        throw;
    }
    finally
    {
        telemetryClient.StopOperation(operation);
    }
}
```

#### <a name="process"></a>İşleme
```csharp
public async Task Process(BrokeredMessage message)
{
    // After the message is taken from the queue, create RequestTelemetry to track its processing.
    // It might also make sense to get the name from the message.
    RequestTelemetry requestTelemetry = new RequestTelemetry { Name = "process " + queueName };

    var rootId = message.Properties["RootId"].ToString();
    var parentId = message.Properties["ParentId"].ToString();
    // Get the operation ID from the Request-Id (if you follow the HTTP Protocol for Correlation).
    requestTelemetry.Context.Operation.Id = rootId;
    requestTelemetry.Context.Operation.ParentId = parentId;

    var operation = telemetryClient.StartOperation(requestTelemetry);

    try
    {
        await ProcessMessage();
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        throw;
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetryClient.StopOperation(operation);
    }
}
```

### <a name="azure-storage-queue"></a>Azure Depolama kuyruğu
Aşağıdaki örnek, [Azure Depolama sıra](../../storage/queues/storage-dotnet-how-to-use-queues.md) işlemlerinin nasıl izleyip üretici, tüketici ve Azure Depolama sı arasında ilişkilendirilen telemetriyi gösterir. 

Depolama kuyruğunda bir HTTP API vardır. Kuyruğa yapılan tüm aramalar, HTTP istekleri için Uygulama Öngörüleri Bağımlılık Toplayıcısı tarafından izlenir.
ASP.NET ve ASP.NET Core uygulamalarında varsayılan olarak yapılandırılır, diğer uygulama türleri ile konsol [uygulamaları belgelerine](../../azure-monitor/app/console.md) başvurabilirsiniz

Ayrıca, Uygulama Öngörüleri çalışma kimliğini Depolama isteği kimliğiyle ilişkilendirmek de isteyebilirsiniz. Depolama isteği istemcisi ve sunucu istek kimliği nin nasıl ayarlandığı ve alınabildiğini öğrenmek için [Azure Depolama'yı İzle, tanıve sorun giderme](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md#end-to-end-tracing)kimliğine bakın.

#### <a name="enqueue"></a>Enqueue
Depolama kuyrukları HTTP API'yi desteklediğinden, kuyruktaki tüm işlemler Uygulama Öngörüleri tarafından otomatik olarak izlenir. Birçok durumda, bu enstrümantasyon yeterli olmalıdır. Ancak, tüketici tarafındaki izleri üretici izleriyle ilişkilendirmek için, http Korelasyon Protokolü'nde bunu nasıl yaptığımıza benzer bir korelasyon bağlamını geçmeniz gerekir. 

Bu örnek, işlemin `Enqueue` nasıl izleyilizleyeceğini gösterir. Şunları yapabilirsiniz:

 - **Korelasyon yeniden denemeleri (varsa)**: Hepsinin işlem olan `Enqueue` ortak bir ebeveyni vardır. Aksi takdirde, gelen istek çocuk olarak izlenir. Kuyruğa birden çok mantıksal istek varsa, hangi çağrının yeniden denemeyle sonuçlandığını bulmak zor olabilir.
 - **Bağdaştırma Depolama günlükleri (gerekirse ve gerektiğinde)**: Uygulama Öngörüleri telemetrisi ile ilişkilidir.

İşlem, `Enqueue` bir üst işlemin alt öğesidir (örneğin, gelen bir HTTP isteği). HTTP bağımlılık `Enqueue` çağrısı, işlemin alt ve gelen isteğinin torunudur:

```csharp
public async Task Enqueue(CloudQueue queue, string message)
{
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("enqueue " + queue.Name);
    operation.Telemetry.Type = "Azure queue";
    operation.Telemetry.Data = "Enqueue " + queue.Name;

    // MessagePayload represents your custom message and also serializes correlation identifiers into payload.
    // For example, if you choose to pass payload serialized to JSON, it might look like
    // {'RootId' : 'some-id', 'ParentId' : '|some-id.1.2.3.', 'message' : 'your message to process'}
    var jsonPayload = JsonConvert.SerializeObject(new MessagePayload
    {
        RootId = operation.Telemetry.Context.Operation.Id,
        ParentId = operation.Telemetry.Id,
        Payload = message
    });
    
    CloudQueueMessage queueMessage = new CloudQueueMessage(jsonPayload);

    // Add operation.Telemetry.Id to the OperationContext to correlate Storage logs and Application Insights telemetry.
    OperationContext context = new OperationContext { ClientRequestID = operation.Telemetry.Id};

    try
    {
        await queue.AddMessageAsync(queueMessage, null, null, new QueueRequestOptions(), context);
    }
    catch (StorageException e)
    {
        operation.Telemetry.Properties.Add("AzureServiceRequestID", e.RequestInformation.ServiceRequestID);
        operation.Telemetry.Success = false;
        operation.Telemetry.ResultCode = e.RequestInformation.HttpStatusCode.ToString();
        telemetryClient.TrackException(e);
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetryClient.StopOperation(operation);
    }
}  
```

Uygulama raporlarınızın telemetri miktarını azaltmak veya `Enqueue` işlemi başka nedenlerle izlemek istemiyorsanız, `Activity` API'yi doğrudan kullanın:

- Uygulama Öngörüleri işlemini `Activity` başlatmak yerine yeni bir yeni oluşturun (ve başlatın). İşlem adı dışında üzerinde herhangi bir özellik atamanız *gerekmez.*
- İleti `yourActivity.Id` yüküne serihale `operation.Telemetry.Id`yerine. Ayrıca kullanabilirsiniz. `Activity.Current.Id`


#### <a name="dequeue"></a>Dequeue
Benzer `Enqueue`şekilde, Depolama kuyruğuna gerçek bir HTTP isteği Uygulama Öngörüleri tarafından otomatik olarak izlenir. Ancak, `Enqueue` işlem büyük olasılıkla gelen istek bağlamı gibi üst bağlamda olur. Uygulama Öngörüleri SDK'ları, böyle bir işlemi (ve http kısmını) aynı kapsamda bildirilen üst istek ve diğer telemetrilerle otomatik olarak ilişkilendirer.

Operasyon `Dequeue` zor. Uygulama Öngörüleri SDK, HTTP isteklerini otomatik olarak izler. Ancak, ileti ayrıştırılana kadar korelasyon bağlamını bilmez. Özellikle birden fazla ileti alındığı zaman, telemetrinin geri kalanıyla iletiyi almak için HTTP isteğini ilişkilendirmek mümkün değildir.

```csharp
public async Task<MessagePayload> Dequeue(CloudQueue queue)
{
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("dequeue " + queue.Name);
    operation.Telemetry.Type = "Azure queue";
    operation.Telemetry.Data = "Dequeue " + queue.Name;
    
    try
    {
        var message = await queue.GetMessageAsync();
    }
    catch (StorageException e)
    {
        operation.telemetry.Properties.Add("AzureServiceRequestID", e.RequestInformation.ServiceRequestID);
        operation.telemetry.Success = false;
        operation.telemetry.ResultCode = e.RequestInformation.HttpStatusCode.ToString();
        telemetryClient.TrackException(e);
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetryClient.StopOperation(operation);
    }

    return null;
}
```

#### <a name="process"></a>İşleme

Aşağıdaki örnekte, gelen ileti gelen HTTP isteğine benzer şekilde izlenir:

```csharp
public async Task Process(MessagePayload message)
{
    // After the message is dequeued from the queue, create RequestTelemetry to track its processing.
    RequestTelemetry requestTelemetry = new RequestTelemetry { Name = "process " + queueName };
    
    // It might also make sense to get the name from the message.
    requestTelemetry.Context.Operation.Id = message.RootId;
    requestTelemetry.Context.Operation.ParentId = message.ParentId;

    var operation = telemetryClient.StartOperation(requestTelemetry);

    try
    {
        await ProcessMessage();
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        throw;
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetryClient.StopOperation(operation);
    }
}
```

Benzer şekilde, diğer sıra işlemleri işletilebilir. Bir gözetleme işlemi, dequeue işlemi gibi benzer bir şekilde enstrümantve olmalıdır. Sıra yönetimi işlemlerini enstrümanting gerekli değildir. Application Insights HTTP gibi işlemleri izler ve çoğu durumda yeterlidir.

İleti silme işlemini enstrüman tettiğinizde, işlem (korelasyon) tanımlayıcılarını ayarladığınızdan emin olun. Alternatif olarak, `Activity` API kullanabilirsiniz. Uygulama Öngörüleri SDK sizin için yapar, çünkü o zaman telemetri öğeleri üzerinde işlem tanımlayıcıları ayarlamak gerekmez:

- Kuyruktan `Activity` bir öğe aldıktan sonra yeni bir öğe oluşturun.
- Tüketici `Activity.SetParentId(message.ParentId)` ve üretici günlüklerini ilişkilendirmek için kullanın.
- Başlat `Activity`ın.
- Yardımcıları kullanarak `Start/StopOperation` dequeue'yi izleme, işleme ve silme işlemlerini. Aynı eşzamanlı kontrol akışından (yürütme bağlamından) yapın. Bu şekilde, onlar düzgün korelasyon konum.
- `Activity`nu durdurun.
- Telemetriyi el ile kullanın `Start/StopOperation`veya arayın. `Track`

### <a name="dependency-types"></a>Bağımlılık Türleri

Application Insights, Kullanıcı Bira'sı deneyimlerini birleştirmeiçin bağımlılık türünü kullanır. Sıralar `DependencyTelemetry` [için, İşlem tanılama deneyimini](/azure/azure-monitor/app/transaction-diagnostics)geliştiren aşağıdaki türleri tanır:
- `Azure queue`Azure Depolama Kuyrukları için
- `Azure Event Hubs`Azure Etkinlik Hub'ları için
- `Azure Service Bus`Azure Servis Veri Servisi için

### <a name="batch-processing"></a>Toplu işleme
Bazı kuyruklarda, tek bir istekle birden çok iletinin kuyruğundan ayırabilirsiniz. Bu tür iletilerin işlenmesi büyük olasılıkla bağımsızdır ve farklı mantıksal işlemlere aittir. İşlemi `Dequeue` işlenen belirli bir iletiyle ilişkilendirmek mümkün değildir.

Her ileti kendi eşzamanlı kontrol akışında işlenmelidir. Daha fazla bilgi için [Giden bağımlılıklar izleme](#outgoing-dependencies-tracking) bölümüne bakın.

## <a name="long-running-background-tasks"></a>Uzun süren arka plan görevleri

Bazı uygulamalar, kullanıcı isteklerinin neden olabileceği uzun süren işlemleri başlatabilir. İzleme/enstrümantasyon açısından, istek veya bağımlılık enstrümantasyonundan farklı değildir: 

```csharp
async Task BackgroundTask()
{
    var operation = telemetryClient.StartOperation<DependencyTelemetry>(taskName);
    operation.Telemetry.Type = "Background";
    try
    {
        int progress = 0;
        while (progress < 100)
        {
            // Process the task.
            telemetryClient.TrackTrace($"done {progress++}%");
        }
        // Update status code and success as appropriate.
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        // Update status code and success as appropriate.
        throw;
    }
    finally
    {
        telemetryClient.StopOperation(operation);
    }
}
```

Bu örnekte, `telemetryClient.StartOperation` `DependencyTelemetry` korelasyon bağlamını oluşturur ve doldurur. İşlemi zamanlayan gelen istekler tarafından oluşturulan bir üst işleminizin olduğunu varsayalım. Gelen istekle aynı eşzamanlı denetim akışında başladığı sürece, `BackgroundTask` bu üst işlemle ilişkilidir. `BackgroundTask`ve iç içe bütün telemetri öğeleri, istek sona erdikten sonra bile, buna neden olan istekle otomatik olarak ilişkilidir.

Görev, herhangi bir`Activity`işlemi olmayan arka plan iş parçacığından başladığında, `BackgroundTask` ebeveyni yoktur. Ancak, iç içe işlemleri olabilir. Görevden bildirilen tüm telemetri `DependencyTelemetry` öğeleri, 'de `BackgroundTask`oluşturulan öğelerle ilişkilidir.

## <a name="outgoing-dependencies-tracking"></a>Giden bağımlılıkizleme
Kendi bağımlılık türünüzü veya Application Insights tarafından desteklenmeyen bir işlemi izleyebilirsiniz.

Servis `Enqueue` Veri Servisi kuyruğundaki veya Depolama kuyruğundaki yöntem, bu tür özel izlemelere örnek teşkil edebilir.

Özel bağımlılık izleme için genel yaklaşım:

- Korelasyon `TelemetryClient.StartOperation` ve diğer bazı özellikler `DependencyTelemetry` (başlangıç saati damgası, süre) için gerekli olan özellikleri dolduran (uzantı) yöntemini arayın.
- Ad ve ihtiyacınız `DependencyTelemetry`olan diğer bağlam lar gibi diğer özel özellikleri ayarlayın.
- Bağımlılık çağrısı yapın ve bekleyin.
- Bittiğinde işlemi `StopOperation` durdurun.
- Özel durumları işleyebilir.

```csharp
public async Task RunMyTaskAsync()
{
    using (var operation = telemetryClient.StartOperation<DependencyTelemetry>("task 1"))
    {
        try 
        {
            var myTask = await StartMyTaskAsync();
            // Update status code and success as appropriate.
        }
        catch(...) 
        {
            // Update status code and success as appropriate.
        }
    }
}
```

İşlemi ortadan katarak işlem durdurulabilir, bu nedenle `StopOperation`aramak yerine yapabilirsiniz.

*Uyarı*: Bazı durumlarda teslimsiz özel durum, işlemlerin izlenmemesi için çağrılmayı [engelleyebilir.](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/try-finally) `finally`

### <a name="parallel-operations-processing-and-tracking"></a>Paralel işlemler işleme ve izleme

`StopOperation`yalnızca başlatılan işlemi durdurur. Geçerli çalışan işlem durdurmak istediğiniz işlemle eşleşmiyorsa, `StopOperation` hiçbir şey yapmaz. Aynı yürütme bağlamında paralel olarak birden çok işlemi başlatırsanız, bu durum olabilir:

```csharp
var firstOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 1");
var firstTask = RunMyTaskAsync();

var secondOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 2");
var secondTask = RunMyTaskAsync();

await firstTask;

// FAILURE!!! This will do nothing and will not report telemetry for the first operation
// as currently secondOperation is active.
telemetryClient.StopOperation(firstOperation); 

await secondTask;
```

Paralel çalışan işlemleri `StartOperation` yalıtmak için her zaman aynı **async** yönteminde arama ve işlem işlem emin olun. İşlem eşzamanlı (veya eşit değilse), işlemi sarın `Task.Run`ve şu şekilde izleyin:

```csharp
public void RunMyTask(string name)
{
    using (var operation = telemetryClient.StartOperation<DependencyTelemetry>(name))
    {
        Process();
        // Update status code and success as appropriate.
    }
}

public async Task RunAllTasks()
{
    var task1 = Task.Run(() => RunMyTask("task 1"));
    var task2 = Task.Run(() => RunMyTask("task 2"));
    
    await Task.WhenAll(task1, task2);
}
```

## <a name="applicationinsights-operations-vs-systemdiagnosticsactivity"></a>ApplicationInsights işlemleri vs System.Diagnostics.Activity
`System.Diagnostics.Activity`dağıtılmış izleme bağlamını temsil eder ve çerçeveler ve kitaplıklar tarafından sürecin içinde ve dışında bağlam oluşturmak ve yaymak ve telemetri öğelerini ilişkilendirmek için kullanılır. Etkinlik, ilginç `System.Diagnostics.DiagnosticSource` olaylar (gelen veya giden istekler, özel durumlar, vb.) hakkında bilgi vermek için çerçeve/kitaplık arasındaki bildirim mekanizmasıyla birlikte çalışır.

Uygulamalar Öngörüleri'nde birinci sınıf vatandaşlar ve otomatik bağımlılık ve istek toplama `DiagnosticSource` etkinlikleri ile birlikte büyük ölçüde onlara dayanır. Uygulamanızda Etkinlik oluşturursanız , uygulama öngörüleri telemetrisi oluşturulmasına neden olmaz. Application Insights'ın DiagnosticSource etkinliklerini alması ve Etkinliği telemetriye çevirmek için etkinlik adlarını ve yüklerini bilmesi gerekir.

Her Uygulama Öngörüleri işlemi (istek `Activity` veya `StartOperation` bağımlılık) içerir - çağrıldığında, altında Etkinlik oluşturur. `StartOperation`istek veya bağımlılık telemetries el ile izlemek ve her şeyi korelasyon sağlamak için önerilen yoludur.

## <a name="next-steps"></a>Sonraki adımlar

- Uygulama Öngörüleri'nde [telemetri korelasyonunun](correlation.md) temellerini öğrenin.
- İlişkili verilerin Işlem [Tanılama Deneyimi](../../azure-monitor/app/transaction-diagnostics.md) ve [Uygulama Haritası'nı](../../azure-monitor/app/app-map.md)nasıl yetkilerini nasıl elde ettiklerine göz atın.
- Uygulama Öngörüleri türleri ve veri modeli için [veri modeline](../../azure-monitor/app/data-model.md) bakın.
- Özel [olayları ve ölçümleri](../../azure-monitor/app/api-custom-events-metrics.md) Uygulama Öngörüleri'ne bildirin.
- Bağlam özellikleri koleksiyonu için standart [yapılandırmaya](configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet) göz atın.
- Telemetriyi nasıl ilişkilendirdiğimizi görmek için [System.Diagnostics.Activity Kullanım Kılavuzu'nu](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) kontrol edin.
