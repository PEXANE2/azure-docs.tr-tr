---
title: Uçtan uca izlemeyi ve tanılamayı Azure Service Bus | Microsoft Docs
description: İstemci tanılama ve uçtan uca izlemeye (işleme dahil olan tüm hizmetler aracılığıyla istemci) genel Service Bus bakış.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 6138d3d6424364f28f55f81044768acb894bc651
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85340728"
---
# <a name="distributed-tracing-and-correlation-through-service-bus-messaging"></a>Service Bus mesajlaşma aracılığıyla dağıtılmış izleme ve bağıntı

Mikro hizmetler geliştirmenin yaygın sorunlarından biri, işleme dahil olan tüm hizmetler aracılığıyla bir istemciden işlem izleme yeteneğidir. Hata ayıklama, performans analizi, A/B testi ve diğer tipik tanılama senaryolarında yararlı olur.
Bu sorunun bir parçası mantıksal iş parçalarını izlemektir. İleti işleme sonucu ve gecikme ve dış bağımlılık çağrılarını içerir. Diğer bir bölüm, işlem sınırlarının ötesinde bu tanılama olaylarının bağıntısı olur.

Bir üretici kuyruk aracılığıyla bir ileti gönderdiğinde, genellikle başka bir istemci veya hizmet tarafından başlatılan başka bir mantıksal işlem kapsamında gerçekleşir. Aynı işlem tüketici tarafından bir ileti aldıktan sonra devam eder. Hem üretici hem de tüketici (ve işlemi işleyen diğer hizmetler), işlem akışını ve sonucu izlemek için telemetri olaylarını kabul edin. Bu tür olayları ve izleme işlemini uçtan uca ilişkilendirmek için telemetri raporlayan her hizmetin her olayı bir izleme bağlamıyla damgasına sahip olması gerekir.

Microsoft Azure Service Bus mesajlaşması, üreticileri ve tüketicilerin bu tür izleme bağlamını iletmek için kullanması gereken yük özelliklerini tanımladı.
Protokol, [http bağıntı protokolünü](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md)temel alır.

| Özellik Adı        | Açıklama                                                 |
|----------------------|-------------------------------------------------------------|
|  Tanılama kimliği       | Bir üreticinin bir dış çağrısının sıraya özgü tanıtıcısı. [Http protokolündeki Istek kimliği](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md#request-id) , dikkat edilecek noktalar ve biçim için bkz. |
|  Bağıntı bağlamı | İşlem işleme dahil olmak üzere tüm hizmetlere yayılan işlem bağlamı. Daha fazla bilgi için bkz. [http protokolünde bağıntı bağlamı](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md#correlation-context) |

## <a name="service-bus-net-client-autotracing"></a>Service Bus .NET Client 'ı oto izleme

[.NET için Microsoft Azure 3.0.0 Service Bus Client](/dotnet/api/microsoft.azure.servicebus.queueclient) sürümünden itibaren, izleme sistemleri veya istemci kodu parçası tarafından bağlanabilen izleme noktaları sağlar.
Bu izleme, istemci tarafında Service Bus mesajlaşma hizmetine yapılan tüm çağrıların izlenmesini sağlar. İleti işleme, ileti [işleyicisi düzeniyle](/dotnet/api/microsoft.azure.servicebus.queueclient.registermessagehandler)yapıldığında ileti işleme de işaretlenir

### <a name="tracking-with-azure-application-insights"></a>Azure Application Insights izleme

[Microsoft Application Insights](https://azure.microsoft.com/services/application-insights/) , automagical isteği ve bağımlılık izleme dahil zengin performans izleme olanakları sağlar.

Proje türüne bağlı olarak Application Insights SDK 'Yı yüklemelisiniz:
- [ASP.net](../azure-monitor/app/asp-net.md) -ınstall sürüm 2,5-beta2 veya üzeri
- [ASP.NET Core](../azure-monitor/app/asp-net-core.md) -Version 2.2.0-Beta2 veya üstünü yükler.
Bu bağlantılar SDK yükleme, kaynak oluşturma ve SDK 'Yı yapılandırma (gerekirse) hakkında ayrıntılar sağlar. Non-ASP.NET uygulamalar için, [konsol uygulamaları Için Azure Application Insights](../azure-monitor/app/console.md) makalesine başvurun.

İletileri işlemek için [ileti işleyici modelini](/dotnet/api/microsoft.azure.servicebus.queueclient.registermessagehandler) kullanırsanız, işiniz bitti: hizmetiniz tarafından gerçekleştirilen tüm Service Bus çağrıları otomatik olarak izlenir ve diğer telemetri öğeleriyle bağıntılı yapılır. Aksi takdirde, el ile ileti işleme izleme için aşağıdaki örneğe bakın.

#### <a name="trace-message-processing"></a>İleti işlemeyi izle

```csharp
private readonly TelemetryClient telemetryClient;

async Task ProcessAsync(Message message)
{
    var activity = message.ExtractActivity();
    
    // If you are using Microsoft.ApplicationInsights package version 2.6-beta or higher, you should call StartOperation<RequestTelemetry>(activity) instead
    using (var operation = telemetryClient.StartOperation<RequestTelemetry>("Process", activity.RootId, activity.ParentId))
    {
        telemetryClient.TrackTrace("Received message");
        try 
        {
           // process message
        }
        catch (Exception ex)
        {
             telemetryClient.TrackException(ex);
             operation.Telemetry.Success = false;
             throw;
        }

        telemetryClient.TrackTrace("Done");
   }
}
```

Bu örnekte, `RequestTelemetry` işlenen her ileti için bir zaman damgası, süre ve sonuca (başarılı) sahip olan raporlanır. Telemetride bir bağıntı özellikleri kümesi de vardır.
İleti işleme sırasında bildirilen iç içe izlemeler ve özel durumlar, bunları ' alt öğeleri ' olarak temsil eden bağıntı özellikleriyle da damgalı `RequestTelemetry` .

İleti işleme sırasında desteklenen dış bileşenlere çağrılar yaparsanız, bunlar otomatik olarak izlenir ve bağıntılı bir durumdur. El ile izleme ve bağıntı için [Application Insights .NET SDK ile özel Işlemleri izleme](../azure-monitor/app/custom-operations-tracking.md) bölümüne bakın.

Application Insights SDK 'ya ek olarak herhangi bir dış kod çalıştırıyorsanız, Application Insights günlüklerini görüntülerken daha uzun **süre** görmeyi bekler. 

![Application Insights günlüğünde uzun süre](./media/service-bus-end-to-end-tracing/longer-duration.png)

İletiyi alırken bir gecikme olduğu anlamına gelmez. Bu senaryoda, ileti SDK koduna parametre olarak geçirildiğinden ileti zaten alındı. Uygulama öngörüleri günlüklerinde (**işlem**) **ad** etiketi, iletinin artık dış olay işleme kodunuz tarafından işlendiğini gösterir. Bu sorun Azure ile ilgili değildir. Bunun yerine, bu ölçümler, iletinin Service Bus zaten alındığını belirten dış kodunuzun verimliliğini ifade eder. İleti Service Bus alındıktan sonra **işlem** etiketinin nerede oluşturulduğunu ve atandığını görmek için [bu dosyayı GitHub '](https://github.com/Azure/azure-sdk-for-net/blob/4bab05144ce647cc9e704d46d3763de5f9681ee0/sdk/servicebus/Microsoft.Azure.ServiceBus/src/ServiceBusDiagnosticsSource.cs) da görebilirsiniz. 

### <a name="tracking-without-tracing-system"></a>İzleme sistemi olmadan izleme
İzleme sisteminizin otomatik Service Bus çağrıları izlemeyi desteklememesi durumunda, bu tür desteği bir izleme sistemine veya uygulamanıza eklemeyi düşünüyorsunuz. Bu bölümde, Service Bus .NET istemcisi tarafından gönderilen tanılama olayları açıklanmaktadır.  

Service Bus .NET Client, .NET izleme temelleri [System. Diagnostics. Activity](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) ve [System. Diagnostics. diagnosticsource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md)kullanılarak işaretlendi.

`Activity`bir bildirim mekanizması iken bir izleme bağlamı işlevi görür `DiagnosticSource` . 

DiagnosticSource olayları için dinleyici yoksa, izleme devre dışı, sıfır izleme maliyetlerine sahip olur. DiagnosticSource tüm denetimi dinleyiciye verir:
- dinleyici, hangi kaynak ve olayların dinleneceğini denetler
- dinleyici, olay hızını ve örneklemesi denetler
- olaylar, olay sırasında Ileti nesnesine erişip değiştirebilmeniz için tam bağlam sağlayan bir yük ile gönderilir

Uygulamayla devam etmeden önce [Diagnosticsource Kullanıcı kılavuzuyla](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) ilgili bilgi edinin.

Microsoft. Extension. günlükçü ile günlükleri yazan ASP.NET Core uygulamasındaki Service Bus olayları için bir dinleyici oluşturalım.
DiagnosticSource 'a abone olmak için [System. reak. Core](https://www.nuget.org/packages/System.Reactive.Core) kitaplığını kullanır (Ayrıca, diagnosticsource 'a bu olmadan abone olmak da kolaydır)

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory factory, IApplicationLifetime applicationLifetime)
{
    // configuration...

    var serviceBusLogger = factory.CreateLogger("Microsoft.Azure.ServiceBus");

    IDisposable innerSubscription = null;
    IDisposable outerSubscription = DiagnosticListener.AllListeners.Subscribe(delegate (DiagnosticListener listener)
    {
        // subscribe to the Service Bus DiagnosticSource
        if (listener.Name == "Microsoft.Azure.ServiceBus")
        {
            // receive event from Service Bus DiagnosticSource
            innerSubscription = listener.Subscribe(delegate (KeyValuePair<string, object> evnt)
            {
                // Log operation details once it's done
                if (evnt.Key.EndsWith("Stop"))
                {
                    Activity currentActivity = Activity.Current;
                    TaskStatus status = (TaskStatus)evnt.Value.GetProperty("Status");
                    serviceBusLogger.LogInformation($"Operation {currentActivity.OperationName} is finished, Duration={currentActivity.Duration}, Status={status}, Id={currentActivity.Id}, StartTime={currentActivity.StartTimeUtc}");
                }
            });
        }
    });

    applicationLifetime.ApplicationStopping.Register(() =>
    {
        outerSubscription?.Dispose();
        innerSubscription?.Dispose();
    });
}
```

Bu örnekte, dinleyici günlük süre, sonuç, benzersiz tanımlayıcı ve her Service Bus işlemi için başlangıç zamanı.

#### <a name="events"></a>Olaylar

Her işlem için, iki olay gönderilir: ' Başlat ' ve ' Durdur '. Büyük olasılıkla, yalnızca ' Durdur ' olayları ile ilgileniyorsunuz. İşlem sonucunu ve etkinlik özellikleri olarak başlangıç saatini ve süresini sağlarlar.

Olay yükü işlemin bağlamına sahip bir dinleyici sağlar, API gelen parametrelerini ve dönüş değerini çoğaltır. ' Stop ' olay yükü, ' Start ' olay yükünün tüm özelliklerine sahiptir, bu nedenle ' Start ' olayını tamamen yoksayabilirsiniz.

Tüm olaylarda Ayrıca ' Entity ' ve ' Endpoint ' özellikleri bulunur, bunlar aşağıdaki tabloda atlanır
  * `string Entity`--Varlığın adı (kuyruk, konu, vb.)
  * `Uri Endpoint`-Service Bus uç nokta URL 'SI

Her bir ' Stop ' olayının `Status` `TaskStatus` , zaman uyumsuz işlem ile tamamlandı ve bu, basitlik için aşağıdaki tabloda da atlanmıştır.

Aşağıda, Araçlı işlemlerin tam listesi verilmiştir:

| İşlem adı | İzlenen API | Belirli yük özellikleri|
|----------------|-------------|---------|
| Microsoft. Azure. ServiceBus. Send | [Iletileyici. Sendadsync](/dotnet/api/microsoft.azure.servicebus.core.messagesender.sendasync) | `IList<Message> Messages`-Gönderilen iletilerin listesi |
| Microsoft. Azure. ServiceBus. ScheduleMessage | [Iletileyici. ScheduleMessageAsync](/dotnet/api/microsoft.azure.servicebus.core.messagesender.schedulemessageasync) | `Message Message`-İleti işlendi<br/>`DateTimeOffset ScheduleEnqueueTimeUtc`-Zamanlanan ileti kayması<br/>`long SequenceNumber`-Zamanlanmış iletinin sıra sayısı (' Durdur ' olay yükü) |
| Microsoft. Azure. ServiceBus. Cancel | [Iletileyici. CancelScheduledMessageAsync](/dotnet/api/microsoft.azure.servicebus.core.messagesender.cancelscheduledmessageasync) | `long SequenceNumber`-İptal edilecek te iletisinin sıra numarası | 
| Microsoft. Azure. ServiceBus. Receive | [MessageReceiver. ReceiveAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.receiveasync) | `int RequestedMessageCount`-Alınabilecek en fazla ileti sayısı.<br/>`IList<Message> Messages`-Alınan iletilerin listesi (' Durdur ' olay yükü) |
| Microsoft. Azure. ServiceBus. Peek | [MessageReceiver.PeekAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.peekasync) | `int FromSequenceNumber`-Bir ileti toplu işlemine gözatabileceği başlangıç noktası.<br/>`int RequestedMessageCount`-Alınacak ileti sayısı.<br/>`IList<Message> Messages`-Alınan iletilerin listesi (' Durdur ' olay yükü) |
| Microsoft. Azure. ServiceBus. Receiveertelenmiş | [MessageReceiver. ReceiveDeferredMessageAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.receivedeferredmessageasync) | `IEnumerable<long> SequenceNumbers`-Alacak sıra numaralarını içeren liste.<br/>`IList<Message> Messages`-Alınan iletilerin listesi (' Durdur ' olay yükü) |
| Microsoft. Azure. ServiceBus. tamamlanmış | [MessageReceiver. tamamlana eşitleme](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.completeasync) | `IList<string> LockTokens`-Tamamlanacak karşılık gelen iletilerin kilit belirteçlerini içeren liste.|
| Microsoft. Azure. ServiceBus. Abandon | [MessageReceiver. bırakma zaman uyumsuz](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.abandonasync) | `string LockToken`-İptal edilecek karşılık gelen iletinin kilit belirteci. |
| Microsoft. Azure. ServiceBus. erteleme | [MessageReceiver. Defsilinebilir eşitleme](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deferasync) | `string LockToken`-Erteleyecek karşılık gelen iletinin kilit belirteci. | 
| Microsoft. Azure. ServiceBus. Deadmektup | [MessageReceiver.DeadLetterAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deadletterasync) | `string LockToken`-Karşılık gelen iletinin yok sayılma harfine kilit belirteci. | 
| Microsoft. Azure. ServiceBus. RenewLock | [MessageReceiver. RenewLockAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.renewlockasync) | `string LockToken`-Kilidi yenilemek için ilgili iletinin kilit belirteci.<br/>`DateTime LockedUntilUtc`-UTC biçiminde yeni kilit belirteci bitiş tarihi ve saati. (' Durdur ' olay yükü)|
| Microsoft. Azure. ServiceBus. Process | [IReceiverClient. RegisterMessageHandler](/dotnet/api/microsoft.azure.servicebus.core.ireceiverclient.registermessagehandler) içinde sunulan ileti işleyici Lambda işlevi | `Message Message`-İleti işlendi. |
| Microsoft. Azure. ServiceBus. ProcessSession | [Iqueueclient. RegisterSessionHandler](/dotnet/api/microsoft.azure.servicebus.iqueueclient.registersessionhandler) içinde sunulan Ileti oturumu işleyicisi Lambda işlevi | `Message Message`-İleti işlendi.<br/>`IMessageSession Session`-Oturum işleniyor |
| Microsoft. Azure. ServiceBus. AddRule | [SubscriptionClient. AddRuleAsync](/dotnet/api/microsoft.azure.servicebus.subscriptionclient.addruleasync) | `RuleDescription Rule`-Eklenecek kuralı sağlayan kural açıklaması. |
| Microsoft. Azure. ServiceBus. RemoveRule | [SubscriptionClient. RemoveRuleAsync](/dotnet/api/microsoft.azure.servicebus.subscriptionclient.removeruleasync) | `string RuleName`-Kaldırılacak kuralın adı. |
| Microsoft. Azure. ServiceBus. GetRules | [SubscriptionClient. GetRulesAsync](/dotnet/api/microsoft.azure.servicebus.subscriptionclient.getrulesasync) | `IEnumerable<RuleDescription> Rules`-Abonelikle ilişkili tüm kurallar. (Yalnızca ' Durdur ' yük) |
| Microsoft. Azure. ServiceBus. AcceptMessageSession | [Isessionclient. AcceptMessageSessionAsync](/dotnet/api/microsoft.azure.servicebus.isessionclient.acceptmessagesessionasync) | `string SessionId`-İletilerde SessionID var. |
| Microsoft. Azure. ServiceBus. GetSessionState | [Imessagesession. GetStateAsync](/dotnet/api/microsoft.azure.servicebus.imessagesession.getstateasync) | `string SessionId`-İletilerde SessionID var.<br/>`byte [] State`-Oturum durumu (' Durdur ' olay yükü) |
| Microsoft. Azure. ServiceBus. SetSessionState | [Imessagesession. SetStateAsync](/dotnet/api/microsoft.azure.servicebus.imessagesession.setstateasync) | `string SessionId`-İletilerde SessionID var.<br/>`byte [] State`-Oturum durumu |
| Microsoft. Azure. ServiceBus. RenewSessionLock | [Imessagesession. RenewSessionLockAsync](/dotnet/api/microsoft.azure.servicebus.imessagesession.renewsessionlockasync) | `string SessionId`-İletilerde SessionID var. |
| Microsoft. Azure. ServiceBus. Exception | Tüm belgelenmiş API 'leri| `Exception Exception`-Özel durum örneği |

Her olayda `Activity.Current` geçerli işlem bağlamını tutan erişim sağlayabilirsiniz.

#### <a name="logging-additional-properties"></a>Ek özellikleri günlüğe kaydetme

`Activity.Current`geçerli işlemin ve üst öğelerinin ayrıntılı bağlamını sağlar. Daha fazla bilgi için bkz. [etkinlik belgeleri](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) .
Service Bus izleme, içinde bulunan `Activity.Current.Tags` `MessageId` ve her seferinde ek bilgi sağlar `SessionId` .

' Receive ', ' Peek ' ve ' Receiveertelenmiş ' olaylarını izleyen etkinliklerin etiketi de olabilir `RelatedTo` . `Diagnostic-Id`Sonuç olarak alınan iletilerin ayrı listesini barındırır.
Bu tür bir işlem, ilgisiz birçok iletinin alınmasına neden olabilir. Ayrıca, `Diagnostic-Id` işlem başladığında bilinmiyor, bu nedenle ' Receive ' işlemleri yalnızca bu etiketi kullanan ' Process ' işlemleriyle bağıntılı olabilir. İletiyi almak için geçen süreyi denetlemek üzere performans sorunlarını analiz etmek yararlı olur.

Etiketleri günlüğe kaydetmek için etkili bir yol, bunları yinelemek ve önceki örneğe etiket eklemek gibi görünüyor 

```csharp
Activity currentActivity = Activity.Current;
TaskStatus status = (TaskStatus)evnt.Value.GetProperty("Status");

var tagsList = new StringBuilder();
foreach (var tags in currentActivity.Tags)
{
    tagsList.Append($", "{tags.Key}={tags.Value}");
}

serviceBusLogger.LogInformation($"{currentActivity.OperationName} is finished, Duration={currentActivity.Duration}, Status={status}, Id={currentActivity.Id}, StartTime={currentActivity.StartTimeUtc}{tagsList}");
```

#### <a name="filtering-and-sampling"></a>Filtreleme ve örnekleme

Bazı durumlarda, performans yükünü veya depolama tüketimini azaltmak için olayların yalnızca bir kısmını günlüğe kaydetmek tercih edilir. Yalnızca ' Durdur ' olaylarını (önceki örnekte olduğu gibi) veya olayların örnek yüzdesini günlüğe yazabilirsiniz. 
`DiagnosticSource`koşul ile elde etmenin yolunu sağlayın `IsEnabled` . Daha fazla bilgi için bkz. [DiagnosticSource 'Ta bağlam tabanlı filtreleme](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md#context-based-filtering).

`IsEnabled`, performans etkisini en aza indirmek için tek bir işlem için birden çok kez çağrılabilir.

`IsEnabled`Şu sırada çağrılır:

1. `IsEnabled(<OperationName>, string entity, null)`Örneğin, `IsEnabled("Microsoft.Azure.ServiceBus.Send", "MyQueue1")` . Sonunda ' Start ' veya ' Stop ' yok. Belirli işlemleri veya kuyrukları filtrelemek için bu uygulamayı kullanın. Geri çağırma döndürürse `false` , işlem için olaylar gönderilmez

   * ' Process ' ve ' ProcessSession ' işlemleri için `IsEnabled(<OperationName>, string entity, Activity activity)` geri çağırma de alırsınız. Olayları ya da Etiketler özelliklerine göre filtrelemek için kullanın `activity.Id` .
  
2. `IsEnabled(<OperationName>.Start)`Örneğin, `IsEnabled("Microsoft.Azure.ServiceBus.Send.Start")` . ' Start ' olayının tetiklenip tetiklenmeyeceğini denetler. Sonuç yalnızca ' Başlat ' olayını etkiler, ancak daha fazla izleme buna bağlı değildir.

`IsEnabled`' Durdur ' olayı yok.

Eğer bir işlem sonucu özel durum ise, `IsEnabled("Microsoft.Azure.ServiceBus.Exception")` çağırılır. Yalnızca ' özel durum ' olaylarına abone olabilir ve izleme geri kalanını önleyebilirsiniz. Bu durumda, bu tür özel durumları yine de işlemeniz gerekir. Diğer izleme devre dışı olduğundan, izleme bağlamını tüketiciden üretici ile olan iletilerle akışa almayı beklememelisiniz.

`IsEnabled`Ayrıca, örnekleme stratejilerini de kullanabilirsiniz. Veya temel alınarak `Activity.Id` `Activity.RootId` tutarlı örnekleme sağlar (izleme sistemine veya kendi kodunuz tarafından yayıldığında).

Aynı kaynak için birden fazla dinleyici olması halinde `DiagnosticSource` , olayı kabul etmek için yalnızca bir dinleyicinin olması yeterlidir, bu nedenle `IsEnabled` çağrılması garanti edilmez,

## <a name="next-steps"></a>Sonraki adımlar

* [Application Insights bağıntı](../azure-monitor/app/correlation.md)
* REST, SQL veya diğer dış kaynakların sizi yavaşlatıyor olup olmadığını görmek için [Application Insights bağımlılıkları izleyin](../azure-monitor/app/asp-net-dependencies.md) .
* [.NET SDK Application Insights özel işlemleri izleme](../azure-monitor/app/custom-operations-tracking.md)
