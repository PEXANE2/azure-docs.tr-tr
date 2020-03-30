---
title: Azure Hizmet Veri Servisi Uçtan Uca Izleme ve Tanılama | Microsoft Dokümanlar
description: Hizmet Veri Servisi istemci tanılama ve uçtan uca izleme genel bakış (işleme dahil olan tüm hizmetler aracılığıyla istemci.)
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2020
ms.author: aschhab
ms.openlocfilehash: 7c2efc9c736097873201505f280af5d47bed4847
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294162"
---
# <a name="distributed-tracing-and-correlation-through-service-bus-messaging"></a>Servis Veri Servisi mesajlaşması ile dağıtılmış izleme ve korelasyon

Mikro hizmet geliştirmede sık karşılaşılan sorunlardan biri, işleme de dahil olan tüm hizmetler aracılığıyla bir istemciden çalışma izleme yeteneğidir. Hata ayıklama, performans analizi, A/B testi ve diğer tipik tanılama senaryoları için yararlıdır.
Bu sorunun bir parçası işin mantıksal parçalarını izlemektir. İleti işleme sonucu ve gecikme ve dış bağımlılık çağrıları içerir. Başka bir parçası süreç sınırları ötesinde bu tanılama olaylarıkoredir.

Bir üretici bir sıra üzerinden ileti gönderdiğinde, genellikle başka bir istemci veya hizmet tarafından başlatılan başka bir mantıksal işlem kapsamında gerçekleşir. Aynı işlem, bir ileti aldıktan sonra tüketici tarafından devam edilir. Hem üretici hem de tüketici (ve işlemi işleyen diğer hizmetler), muhtemelen operasyon akışını ve sonucunu izlemek için telemetri olayları yontmaktadır. Bu tür olayları ilişkilendirmek ve işlemi uçuça izlemek için, telemetri raporlayan her hizmetin her olayı izleme bağlamıyla damgasıye basması zorundadır.

Microsoft Azure Hizmet Veri Servisi iletisi, üreticilerin ve tüketicilerin bu izleme bağlamından geçmek için kullanmaları gereken yük özelliklerini tanımlamıştır.
Protokol [HTTP Korelasyon protokolüne](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md)dayanır.

| Özellik Adı        | Açıklama                                                 |
|----------------------|-------------------------------------------------------------|
|  Tanı-Id       | Üreticiden kuyruğa harici bir aramanın benzersiz tanımlayıcısı. Gerekçe, değerlendirme ve biçim için [HTTP protokolündeki Request-Id'ye](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md#request-id) bakın |
|  Korelasyon-Bağlam | Operasyon işleme ile ilgili tüm hizmetler arasında yayılan çalışma bağlamı. Daha fazla bilgi için [HTTP protokolündeki Korelasyon-Bağlam](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md#correlation-context) |

## <a name="service-bus-net-client-autotracing"></a>Servis Veri Servisi .NET İstemci otomatik izleme

.NET için sürüm 3.0.0 [Microsoft Azure ServiceBus Client](/dotnet/api/microsoft.azure.servicebus.queueclient) ile başlayarak izleme sistemleri veya istemci kodu parçası tarafından kanca olabilir izleme araçları sağlar.
Enstrümantasyon, istemci tarafından Servis Veri Servisi mesajlaşma hizmetine yapılan tüm çağrıları izlemenize olanak tanır. İleti işleme [ileti işleyicisi deseni](/dotnet/api/microsoft.azure.servicebus.queueclient.registermessagehandler)ile yapılırsa, ileti işleme de

### <a name="tracking-with-azure-application-insights"></a>Azure Uygulama Öngörüleri ile İzleme

[Microsoft Application Insights,](https://azure.microsoft.com/services/application-insights/) otomatik büyüleme isteği ve bağımlılık izleme gibi zengin performans izleme özellikleri sağlar.

Proje türünüze bağlı olarak Uygulama Öngörüleri SDK'yı yükleyin:
- [ASP.NET](../azure-monitor/app/asp-net.md) - sürüm 2.5-beta2 veya daha yüksek yükleyin
- [ASP.NET Core](../azure-monitor/app/asp-net-core.md) - sürüm 2.2.0-beta2 veya daha yüksek yükleyin.
Bu bağlantılar, SDK'nın yüklenmesi, kaynak oluşturma ve SDK'nın (gerekirse) yapılandırılması hakkında ayrıntılı bilgi sağlar. non-ASP.NET uygulamalar [için Konsol Uygulamaları için Azure Uygulama Öngörüleri](../azure-monitor/app/console.md) makalesine bakın.

İletileri işlemek için [ileti işleyicisi deseni](/dotnet/api/microsoft.azure.servicebus.queueclient.registermessagehandler) kullanırsanız, işlem yapılır: hizmetiniz tarafından yapılan tüm Servis Veri Yolu çağrıları otomatik olarak izlenir ve diğer telemetri öğeleriyle ilişkilendirilir. Aksi takdirde el ilemesaj işleme izleme için aşağıdaki örneğe bakın.

#### <a name="trace-message-processing"></a>İleti işlemeyi izleme

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

Bu örnekte, `RequestTelemetry` her işlenmiş ileti için, bir zaman damgası, süre ve sonuç (başarı) olan bildirilir. Telemetri de korelasyon özellikleri kümesi vardır.
İleti işleme sırasında bildirilen iç içe geçen izlemeler ve özel durumlar da onları `RequestTelemetry`'alt' olarak temsil eden korelasyon özellikleriyle damgalanır.

İleti işleme sırasında desteklenen dış bileşenlere arama yaparsanız, bunlar otomatik olarak izlenir ve ilişkilendirilir. Manuel izleme ve korelasyon için [Application Insights .NET SDK ile özel işlemleri izleyin'e](../azure-monitor/app/custom-operations-tracking.md) bakın.

Application Insights SDK'ya ek olarak herhangi bir harici kod çalıştırıyorsanız, Application Insights günlüklerini görüntülerken daha uzun **süre** görmeyi bekleyin. 

![Uygulama Öngörüleri günlüğünde daha uzun süre](./media/service-bus-end-to-end-tracing/longer-duration.png)

Bu, iletiyi almada bir gecikme olduğu anlamına gelmez. Bu senaryoda, ileti SDK koduna parametre olarak iletildiği için ileti zaten alınmıştır. Ayrıca, App Insights günlüklerinde **(İşlem)** **ad** etiketi, iletinin artık harici olay işleme kodunuz tarafından işlendiğini gösterir. Bu sorun Azure ile ilgili değildir. Bunun yerine, bu ölçümler, iletinin Hizmet Veri Servisi'nden zaten alınmış olması nedeniyle dış kodunuzun verimliliğini ifade eder. İleti Hizmet Veri Yolundan alındıktan sonra **İşlem** etiketinin nerede oluşturulduğunu ve atandığını görmek için [GitHub'daki bu dosyaya](https://github.com/Azure/azure-sdk-for-net/blob/4bab05144ce647cc9e704d46d3763de5f9681ee0/sdk/servicebus/Microsoft.Azure.ServiceBus/src/ServiceBusDiagnosticsSource.cs) bakın. 

### <a name="tracking-without-tracing-system"></a>Sistemi izlemeden izleme
Takip sisteminizin otomatik Servis Veri Mes'i aramalarını desteklememesi durumunda, bu tür bir desteği bir izleme sistemine veya uygulamanıza eklemeyi araştırabilirsiniz. Bu bölümde Service Bus .NET istemcisi tarafından gönderilen tanılama olayları açıklanmaktadır.  

Service Bus .NET Client ,NET izleme ilkel [System.Diagnostics.Activity](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) and [System.Diagnostics.Diagnosticsource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md)kullanılarak araçlandırılır.

`Activity`bir bildirim mekanizması `DiagnosticSource` iken bir izleme bağlamı olarak hizmet vermektedir. 

DiagnosticSource olayları için dinleyici yoksa, enstrümantasyon kapalıdır ve enstrümantasyon maliyeti sıfırdır. DiagnosticSource tüm kontrolü dinleyiciye verir:
- dinleyici hangi kaynakları ve olayları dinlemek için kontrol
- dinleyici olay hızını ve örneklemeyi kontrol eder
- olaylar sırasında İleti nesnesi'ne erişebilmeniz ve değiştirebilmeniz için tam bağlam sağlayan bir yük ile gönderilir

Uygulamaya geçmeden önce [DiagnosticSource Kullanım Kılavuzu'nu](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) tanıyın.

Microsoft.Extension.Logger ile günlük ler yazan ASP.NET Core uygulamasında Hizmet Veri Servisi etkinlikleri için bir dinleyici oluşturalım.
Bu DiagnosticSource abone [system.Reactive.Core](https://www.nuget.org/packages/System.Reactive.Core) kütüphane kullanır (aynı zamanda onsuz DiagnosticSource abone olmak kolaydır)

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

Bu örnekte, dinleyici her Hizmet Veri Servisi işlemi için süre, sonuç, benzersiz tanımlayıcı ve başlangıç saatini günlüğe kaydeder.

#### <a name="events"></a>Olaylar

Her işlem için iki olay gönderilir: 'Başlat' ve 'Durdur'. Büyük olasılıkla, sadece 'Stop' etkinlikleri ile ilgileniyorsunuz. Etkinlik özellikleri olarak çalışma sonucunun yanı sıra başlangıç saatini ve süresini de sağlarlar.

Olay yükü işlem bağlamı ile bir dinleyici sağlar, api gelen parametreleri ve iade değeri çoğaltıyor. 'Durdur' olay yükü 'Başlat' olay yükünün tüm özelliklerine sahiptir, böylece 'Başlat' olayını tamamen yok sayabilirsiniz.

Tüm olaylar da 'Varlık' ve 'Bitiş Noktası' özellikleri var, onlar aşağıdaki tabloda atlanır
  * `string Entity`- - Varlığın adı (sıra, konu, vb.)
  * `Uri Endpoint`- Servis Veri Günü bitiş noktası URL'si

Her 'Durdur' `Status` olayı, `TaskStatus` basitlik için aşağıdaki tabloda atlanan async işlemi ile tamamlanmış bir özelliğe sahiptir.

İşletilmiş operasyonların tam listesi aşağıda veda edilmiştir:

| Operasyon Adı | İzlenen API | Belirli Taşıma Kapasitesi Özellikleri|
|----------------|-------------|---------|
| Microsoft.Azure.ServiceBus.Gönder | [MessageSender.SendAsync](/dotnet/api/microsoft.azure.servicebus.core.messagesender.sendasync) | `IList<Message> Messages`- Gönderilen iletilerin listesi |
| Microsoft.Azure.ServiceBus.ScheduleMessage | [MessageSender.ScheduleMessageAsync](/dotnet/api/microsoft.azure.servicebus.core.messagesender.schedulemessageasync) | `Message Message`- İleti işleniyor<br/>`DateTimeOffset ScheduleEnqueueTimeUtc`- Zamanlanmış ileti mahsup<br/>`long SequenceNumber`- Zamanlanan iletinin sıra numarası ('Stop' olay yükü) |
| Microsoft.Azure.ServiceBus.Cancel | [MessageSender.CancelScheduledMessageAsync](/dotnet/api/microsoft.azure.servicebus.core.messagesender.cancelscheduledmessageasync) | `long SequenceNumber`- İptal edilecek te iletisinin sıra numarası | 
| Microsoft.Azure.ServiceBus.Receive | [MessageReceiver.ReceiveAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.receiveasync) | `int RequestedMessageCount`- Alınabilecek en fazla ileti sayısı.<br/>`IList<Message> Messages`- Alınan iletilerin listesi ('Stop' olay yükü) |
| Microsoft.Azure.ServiceBus.Peek | [MessageReceiver.PeekAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.peekasync) | `int FromSequenceNumber`- Bir dizi iletiye göz atmak için başlangıç noktası.<br/>`int RequestedMessageCount`- Alınacak ileti sayısı.<br/>`IList<Message> Messages`- Alınan iletilerin listesi ('Stop' olay yükü) |
| Microsoft.Azure.ServiceBus.ReceiveErtelenmiş | [MessageReceiver.ReceiveDeferredMessageAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.receivedeferredmessageasync) | `IEnumerable<long> SequenceNumbers`- Alacak sıra numaralarını içeren liste.<br/>`IList<Message> Messages`- Alınan iletilerin listesi ('Stop' olay yükü) |
| Microsoft.Azure.ServiceBus.Complete | [MessageReceiver.CompleteAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.completeasync) | `IList<string> LockTokens`- Tamamlamak için ilgili iletilerin kilit belirteçleri içeren liste.|
| Microsoft.Azure.ServiceBus.Terk | [MessageReceiver.AbandonAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.abandonasync) | `string LockToken`- Terk etmek için ilgili iletinin kilit belirteci. |
| Microsoft.Azure.ServiceBus.Defer | [MessageReceiver.DeferAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deferasync) | `string LockToken`- Ertelemek için ilgili iletinin kilit belirteci. | 
| Microsoft.Azure.ServiceBus.DeadLetter | [MessageReceiver.DeadLetterAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deadletterasync) | `string LockToken`- Ölü harfe karşılık gelen mesajın kilit belirteci. | 
| Microsoft.Azure.ServiceBus.RenewLock | [MessageReceiver.RenewLockAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.renewlockasync) | `string LockToken`- Kilitlemeyi yenilemek için ilgili iletinin kilit belirteci.<br/>`DateTime LockedUntilUtc`- UTC formatında yeni kilit belirteci son kullanma tarihi ve saati. ('Dur' olay yükü)|
| Microsoft.Azure.ServiceBus.Process | [IReceiverClient.RegisterMessageHandler'da](/dotnet/api/microsoft.azure.servicebus.core.ireceiverclient.registermessagehandler) sağlanan İleti İşleyicisi lambda işlevi | `Message Message`- İleti işleniyor. |
| Microsoft.Azure.ServiceBus.ProcessSession | [IQueueClient.RegisterSessionHandler'da](/dotnet/api/microsoft.azure.servicebus.iqueueclient.registersessionhandler) sağlanan İleti Oturum Handleyici lambda işlevi | `Message Message`- İleti işleniyor.<br/>`IMessageSession Session`- Oturum işleniyor |
| Microsoft.Azure.ServiceBus.AddRule | [AbonelikClient.AddRuleAsync](/dotnet/api/microsoft.azure.servicebus.subscriptionclient.addruleasync) | `RuleDescription Rule`- Eklenecek kuralı sağlayan kural açıklaması. |
| Microsoft.Azure.ServiceBus.RemoveRule | [AbonelikClient.RemoveRuleAsync](/dotnet/api/microsoft.azure.servicebus.subscriptionclient.removeruleasync) | `string RuleName`- Kaldırılacak kuralın adı. |
| Microsoft.Azure.ServiceBus.GetRules | [AbonelikClient.GetRulesAsync](/dotnet/api/microsoft.azure.servicebus.subscriptionclient.getrulesasync) | `IEnumerable<RuleDescription> Rules`- Abonelikle ilgili tüm kurallar. ('Stop' yükü yalnızca) |
| Microsoft.Azure.ServiceBus.AcceptMessageSession | [ISessionClient.AcceptMessageSessionAsync](/dotnet/api/microsoft.azure.servicebus.isessionclient.acceptmessagesessionasync) | `string SessionId`- İletilerde sessionId bulunur. |
| Microsoft.Azure.ServiceBus.GetSessionState | [IMessageSession.GetStateAsync](/dotnet/api/microsoft.azure.servicebus.imessagesession.getstateasync) | `string SessionId`- İletilerde sessionId bulunur.<br/>`byte [] State`- Oturum durumu ('Durdur' olay yükü) |
| Microsoft.Azure.ServiceBus.SetSessionState | [IMessageSession.SetStateAsync](/dotnet/api/microsoft.azure.servicebus.imessagesession.setstateasync) | `string SessionId`- İletilerde sessionId bulunur.<br/>`byte [] State`- Oturum durumu |
| Microsoft.Azure.ServiceBus.RenewSessionLock | [IMessageSession.RenewSessionLockAsync](/dotnet/api/microsoft.azure.servicebus.imessagesession.renewsessionlockasync) | `string SessionId`- İletilerde sessionId bulunur. |
| Microsoft.Azure.ServiceBus.Exception | herhangi bir enstrümantif API| `Exception Exception`- Özel durum örneği |

Her olayda, geçerli `Activity.Current` çalışma bağlamına sahip olana erişebilirsiniz.

#### <a name="logging-additional-properties"></a>Ek özellikleri günlüğe kaydetme

`Activity.Current`mevcut operasyonun ve ebeveynlerinin ayrıntılı bağlamını sağlar. Daha fazla bilgi için daha fazla ayrıntı için [Etkinlik belgelerine](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) bakın.
Service Bus instrumentation , `Activity.Current.Tags` tutun `MessageId` ve `SessionId` ne zaman kullanılabilir ek bilgi sağlar.

'Receive', 'Peek' ve 'ReceiveDeferred' olayını izleyen `RelatedTo` etkinliklerde de etiket olabilir. Sonuç olarak alınan `Diagnostic-Id`iletilerin (ler) ayrı bir listesini tutar.
Bu tür bir işlem, birkaç ilgisiz iletinin alınmasına neden olabilir. Ayrıca, `Diagnostic-Id` işlemin ne zaman başladığı bilinmemektedir, bu nedenle 'Receive' işlemleri yalnızca bu Etiketi kullanarak 'İşlem' işlemleriyle ilişkilendirilebilir. İletiyi almanın ne kadar sürdüğünü denetlemek için performans sorunlarını çözümlerken kullanışlıdır.

Etiketleri kaydetmenin etkili yolu, etiketleri üzerlerinde yinelemektir, bu nedenle önceki örneğe Etiketler eklemek aşağıdaki gibi görünür 

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

Bazı durumlarda, performans ek yükü veya depolama tüketimini azaltmak için olayların yalnızca bir kısmını günlüğe kaydetmek istenir. 'Durdur' olaylarını yalnızca (önceki örnekte olduğu gibi) veya olayların örnek yüzdesini günlüğe kaydedebilirsiniz. 
`DiagnosticSource`yüklem ile `IsEnabled` elde etmek için yol sağlar. Daha fazla bilgi için [DiagnosticSource'da Bağlam Tabanlı Filtreleme'ye](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md#context-based-filtering)bakın.

`IsEnabled`performans etkisini en aza indirmek için tek bir işlem için birden çok kez çağrılabilir.

`IsEnabled`aşağıdaki sırayla çağrılır:

1. `IsEnabled(<OperationName>, string entity, null)`örneğin, `IsEnabled("Microsoft.Azure.ServiceBus.Send", "MyQueue1")`. Sonunda 'Başlat' veya 'Dur' olmadığını unutmayın. Belirli işlemleri veya kuyrukları filtrelemek için kullanın. Geri arama `false`geri dönerse, işlem için olaylar gönderilmez

   * 'İşlem' ve 'İşlem Oturumu' işlemleri için `IsEnabled(<OperationName>, string entity, Activity activity)` de geri çağrı alırsınız. Olayları `activity.Id` veya Etiketler özelliklerine göre filtrelemek için kullanın.
  
2. `IsEnabled(<OperationName>.Start)`örneğin, `IsEnabled("Microsoft.Azure.ServiceBus.Send.Start")`. 'Başlat' olayının başlatılıp başlatılmayacağını denetler. Sonuç yalnızca 'Başlat' olayını etkiler, ancak daha fazla enstrümantasyon buna bağlı değildir.

'Dur' `IsEnabled` olayı için bir şey yoktur.

Bazı işlem sonucu özel `IsEnabled("Microsoft.Azure.ServiceBus.Exception")` ise, çağrılır. Yalnızca 'Özel Durum' olaylarına abone olabilir ve enstrümantasyonun geri kalanını engelleyebilirsiniz. Bu durumda, yine de bu tür özel durumları işlemek zorunda. Diğer enstrümantasyon devre dışı bırakıldığından, izleme bağlamının tüketiciden üreticiye gelen iletilerle akmasını beklememelisiniz.

Örnekleme stratejilerini de uygulayabilirsiniz. `IsEnabled` Örnekleme, `Activity.Id` `Activity.RootId` tüm lastiklerde tutarlı örnekleme sağlar (izleme sistemi veya kendi kodunuzla yayLımit ettiği sürece).

Aynı kaynak `DiagnosticSource` için birden fazla dinleyicinin varlığında, sadece bir dinleyicinin olayı `IsEnabled` kabul etmesi yeterlidir, bu nedenle çağrılması garanti edilmez,

## <a name="next-steps"></a>Sonraki adımlar

* [Uygulama Öngörüleri Korelasyon](../azure-monitor/app/correlation.md)
* [Application Insights,](../azure-monitor/app/asp-net-dependencies.md) REST, SQL veya diğer dış kaynakların sizi yavaşlatıp yavaşlatıp yavaşlatmamasını görmek için Bağımlılıkları izler.
* [Uygulama Öngörüleri .NET SDK ile özel işlemleri izleyin](../azure-monitor/app/custom-operations-tracking.md)
