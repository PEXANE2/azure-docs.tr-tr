---
title: .NET ve AMQP 1.0 ile Azure Servis Veri Servisi | Microsoft Dokümanlar
description: Bu makalede, BIR .NET uygulamasından Gelen Azure Hizmet Veri Tos'un amqp (Gelişmiş İleti Sıraya Protokolü) kullanarak nasıl kullanılacağı açıklanmaktadır.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 332bcb13-e287-4715-99ee-3d7d97396487
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2020
ms.author: aschhab
ms.openlocfilehash: 8157efac5ff1fc135659a84b4f4825ff36307480
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80297662"
---
# <a name="use-service-bus-from-net-with-amqp-10"></a>AMQP 1.0 ile .NET'ten Servis Veri Servisi'ni kullanma

AMQP 1.0 desteği Service Bus paketi sürüm 2.1 veya sonraki sürümde kullanılabilir. [NuGet'den][NuGet]Servis Veri Servisi bitlerini indirerek en son sürüme sahip olduğunuzdan emin olabilirsiniz.

## <a name="configure-net-applications-to-use-amqp-10"></a>.NET uygulamalarını AMQP 1.0 kullanacak şekilde yapılandırın

Varsayılan olarak, Service Bus .NET istemci kitaplığı özel bir SOAP tabanlı protokol kullanarak Servis Veri Servisi ile iletişim kurar. Varsayılan protokol yerine AMQP 1.0 kullanmak, bir sonraki bölümde açıklandığı gibi Servis Veri Servisi bağlantı dizesinde açık yapılandırma gerektirir. Bu değişiklik dışında, AMQP 1.0 kullanırken uygulama kodu değişmeden kalır.

Geçerli sürümde, AMQP kullanırken desteklenmeyen birkaç API özelliği vardır. Bu desteklenmeyen özellikler davranış [farklılıkları](#behavioral-differences)bölümünde listelenir. Bazı gelişmiş yapılandırma ayarları da AMQP kullanırken farklı bir anlamı vardır.

### <a name="configuration-using-appconfig"></a>App.config kullanarak yapılandırma

Uygulamaların ayarları depolamak için App.config yapılandırma dosyasını kullanması iyi bir uygulamadır. Servis Veri Servisi uygulamaları için Servis Veri Servisi bağlantı dizesini depolamak için App.config'i kullanabilirsiniz. Örnek bir App.config dosyası aşağıdaki gibidir:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
    </appSettings>
</configuration>
```

`Microsoft.ServiceBus.ConnectionString` Ayarın değeri, Hizmet Veri Çaracı'na olan bağlantıyı yapılandırmak için kullanılan Servis Veri Servisi bağlantı dizesidir. Bunun biçimi aşağıdaki gibidir:

`Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp`

Hizmet `namespace` `SAS key` Veri Cemi'si ad alanı oluşturduğunuzda [Azure portalından][Azure portal] nereden ve nereden elde edilir. Daha fazla bilgi için Azure [portalını kullanarak Hizmet Veri Servisi ad alanı oluşturma'ya][Create a Service Bus namespace using the Azure portal]bakın.

AMQP kullanırken, bağlantı dizesini `;TransportType=Amqp`. Bu notasyon, istemci kitaplığına AMQP 1.0 kullanarak Servis Veri Servisi'ne bağlantısını kurmasını bildirir.

## <a name="message-serialization"></a>İleti serileştirme

Varsayılan protokolü kullanırken, .NET istemci kitaplığı varsayılan serileştirme davranışı istemci kitaplığı ve Hizmet Veri Servisi hizmeti arasında aktarım için bir [Aracılı Mesaj][BrokeredMessage] örneği serileştirmek için [DataContractSerializer][DataContractSerializer] türünü kullanmaktır. AMQP aktarım modunu kullanırken, istemci kitaplığı [aracılı iletinin][BrokeredMessage] bir AMQP iletisine serileştirilmesi için AMQP türü sistemini kullanır. Bu serileştirme, iletinin, hizmet veri yolundan erişmek için JMS API'sını kullanan bir Java uygulaması gibi farklı bir platformda çalışan alıcı bir uygulama tarafından alınmasını ve yorumlanmasını sağlar.

[Aracılı İleti][BrokeredMessage] örneğini oluştursanız, iletinin gövdesi olarak hizmet vermek üzere oluşturucuya parametre olarak bir .NET nesnesi sağlayabilirsiniz. AMQP ilkel türleri eşlenebilir nesneler için, gövde AMQP veri türlerine seri leştirilmiştir. Nesne doğrudan bir AMQP ilkel türüne eşlenemezse; diğer bir deyişle, uygulama tarafından tanımlanan özel bir tür, daha sonra nesne [DataContractSerializer][DataContractSerializer]kullanılarak seri hale getirilir ve serileştirilmiş baytbir AMQP veri iletisi olarak gönderilir.

non-.NET istemcilerle birlikte çalışabilirliği kolaylaştırmak için, iletinin gövdesi için doğrudan AMQP türlerine serileştirilebilen yalnızca .NET türlerini kullanın. Aşağıdaki tabloda bu türler ve AMQP türü sistemine karşılık gelen eşleme ayrıntıları.

| .NET Gövde Nesne Türü | Eşlenen AMQP Türü | AMQP Gövde Kesit Tipi |
| --- | --- | --- |
| bool |boole |AMQP Değeri |
| byte |ubayt |AMQP Değeri |
| ushort |ushort |AMQP Değeri |
| uint |uint |AMQP Değeri |
| ulong |ulong |AMQP Değeri |
| sbyte |byte |AMQP Değeri |
| short |short |AMQP Değeri |
| int |int |AMQP Değeri |
| long |long |AMQP Değeri |
| float |float |AMQP Değeri |
| double |double |AMQP Değeri |
| decimal |ondalık128 |AMQP Değeri |
| char |char |AMQP Değeri |
| DateTime |timestamp |AMQP Değeri |
| Guid |uuid |AMQP Değeri |
| bayt[] |ikili |AMQP Değeri |
| string |string |AMQP Değeri |
| System.Collections.IList |list |AMQP Değeri: koleksiyonda bulunan öğeler yalnızca bu tabloda tanımlanan öğeler olabilir. |
| Array |array |AMQP Değeri: koleksiyonda bulunan öğeler yalnızca bu tabloda tanımlanan öğeler olabilir. |
| ıdictionary |map |AMQP Değeri: koleksiyonda bulunan öğeler yalnızca bu tabloda tanımlanan öğeler olabilir. Not: yalnızca String tuşları desteklenir. |
| Urı |Açıklanan dize (aşağıdaki tabloya bakın) |AMQP Değeri |
| DateTimeOffset |Açıklanan uzun (aşağıdaki tabloya bakın) |AMQP Değeri |
| TimeSpan |Açıklanan uzun (aşağıdaki bakınız) |AMQP Değeri |
| Akış |ikili |AMQP Verileri (birden fazla olabilir). Veri bölümleri, Akış nesnesinden okunan ham baytları içerir. |
| Diğer Nesne |ikili |AMQP Verileri (birden fazla olabilir). DataContractSerializer'ı veya uygulama tarafından sağlanan bir serileştiriciyi kullanan nesnenin serileştirilmiş ikilisini içerir. |

| .NET Türü | Eşlenen AMQP Tarifli Tip | Notlar |
| --- | --- | --- |
| Urı |`<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>` |Uri.Absoluteuri |
| DateTimeOffset |`<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` |DateTimeOffset.UtcTicks |
| TimeSpan |`<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type>` |TimeSpan.Ticks |

## <a name="behavioral-differences"></a>Davranış farklılıkları

Varsayılan protokole kıyasla, AMQP kullanırken Servis Veri Çöse .NET API'sinin davranışında bazı küçük farklılıklar vardır:

* [OperationTimeout][OperationTimeout] özelliği yoksayılır.
* `MessageReceiver.Receive(TimeSpan.Zero)`olarak `MessageReceiver.Receive(TimeSpan.FromSeconds(10))`uygulanır.
* İletileri kilit belirteçleri ile tamamlamak yalnızca iletileri başlangıçta alan ileti alıcıları tarafından yapılabilir.

## <a name="control-amqp-protocol-settings"></a>AMQP protokol ayarlarını denetleme

[.NET API'leri,](/dotnet/api/) AMQP protokolünün davranışını denetlemek için çeşitli ayarları ortaya çıkarır:

* **[MessageReceiver.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagereceiver.prefetchcount?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessageReceiver_PrefetchCount)**: Bir bağlantıya uygulanan ilk krediyi denetler. Varsayılan değer, 0'dur.
* **[MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.maxframesize?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_MaxFrameSize)**: Bağlantı açık zamanda görüşme sırasında sunulan maksimum AMQP çerçeve boyutunu denetler. Varsayılan değer 65.536 bayt.
* **[MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.batchflushinterval?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_BatchFlushInterval)**: Aktarımlar toplu olarak işlenebilirse, bu değer gönderim göndermek için maksimum gecikmeyi belirler. Gönderenler/alıcılar tarafından varsayılan olarak devralındı. Tek tek gönderen/alıcı, 20 milisaniye olan varsayılanı geçersiz kılabilir.
* **[MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.usesslstreamsecurity?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_UseSslStreamSecurity)**: TLS bağlantısı üzerinden AMQP bağlantılarının kurulup kurulmadığını kontrol eder. Varsayılan **değer doğrudur.**

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmeye hazır mısınız? Aşağıdaki bağlantıları ziyaret edin:

* [Servis Veri Servisi AMQP'ye genel bakış]
* [AMQP 1.0 protokol kılavuzu]

[Create a Service Bus namespace using the Azure portal]: service-bus-create-namespace-portal.md
[DataContractSerializer]: https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azureservicebus-4.0.0
[Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory.acceptmessagesession?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactory_AcceptMessageSession
[OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout
[NuGet]: https://nuget.org/packages/WindowsAzure.ServiceBus/
[Azure portal]: https://portal.azure.com
[Servis Veri Servisi AMQP'ye genel bakış]: service-bus-amqp-overview.md
[AMQP 1.0 protokol kılavuzu]: service-bus-amqp-protocol-guide.md

