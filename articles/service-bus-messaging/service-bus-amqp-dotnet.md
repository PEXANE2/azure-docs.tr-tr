---
title: .NET ve AMQP 1,0 ile Azure Service Bus | Microsoft Docs
description: Bu makalede, AMQP (gelişmiş mesajlaşma sıraya alma Protokolü) kullanarak bir .NET uygulamasından Azure Service Bus nasıl kullanılacağı açıklanır.
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
ms.date: 06/10/2020
ms.author: aschhab
ms.openlocfilehash: 6555a1718acb0574640e7b7d5d4d47d84b8a72d0
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84711060"
---
# <a name="use-service-bus-from-net-with-amqp-10"></a>AMQP 1,0 ile .NET 'ten Service Bus kullanma

AMQP 1,0 desteği Service Bus paketi sürümü 2,1 veya sonraki bir sürümünde kullanılabilir. Service Bus bitlerini [NuGet][NuGet]'den indirerek en son sürüme sahip olduğunuzdan emin olabilirsiniz.

## <a name="configure-net-applications-to-use-amqp-10"></a>.NET uygulamalarını AMQP 1,0 kullanacak şekilde yapılandırma

Service Bus .NET istemci kitaplığı, varsayılan olarak AMQP protokolünü kullanarak Service Bus hizmetiyle iletişim kurar. Ayrıca, aşağıdaki bölümde gösterildiği gibi, aktarım türü olarak AMQP 'yi açıkça belirtebilirsiniz. 

Geçerli sürümde, AMQP kullanılırken desteklenmeyen birkaç API özelliği vardır. Bu desteklenmeyen özellikler, [davranış farklılıkları](#behavioral-differences)bölümünde listelenmiştir. Gelişmiş yapılandırma ayarlarından bazılarının AMQP kullanılırken de farklı bir anlamı vardır.

### <a name="configuration-using-appconfig"></a>App.config kullanarak yapılandırma

Uygulamaların ayarları depolamak için App.config yapılandırma dosyasını kullanması iyi bir uygulamadır. Service Bus uygulamalar için Service Bus bağlantı dizesini depolamak üzere App.config kullanabilirsiniz. Örnek bir App.config dosyası aşağıdaki gibidir:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
    </appSettings>
</configuration>
```

Ayarın değeri, `Microsoft.ServiceBus.ConnectionString` Service Bus bağlantısını yapılandırmak için kullanılan Service Bus bağlantı dizesidir. Bunun biçimi aşağıdaki gibidir:

`Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp`

`namespace` `SAS key` Service Bus ad alanı oluşturduğunuz zaman ve [Azure Portal][Azure portal] alınır. Daha fazla bilgi için bkz. [Azure Portal kullanarak Service Bus ad alanı oluşturma][Create a Service Bus namespace using the Azure portal].

AMQP kullanırken, bağlantı dizesini ile ekleyin `;TransportType=Amqp` . Bu gösterim, istemci kitaplığına AMQP 1,0 kullanarak Service Bus bağlantısını yapmasını söyler.

## <a name="message-serialization"></a>İleti serileştirme

Varsayılan protokol kullanılırken, .NET istemci kitaplığının varsayılan serileştirme davranışı, istemci kitaplığı ve Service Bus hizmeti arasında aktarım için bir [Brokeredmessage][BrokeredMessage] örneğini seri hale getirmek üzere [DataContractSerializer][DataContractSerializer] türünü kullanmaktır. AMQP aktarım modunu kullanırken, istemci kitaplığı, [aracılı iletiyi][BrokeredMessage] BIR AMQP iletisine serileştirmek IÇIN AMQP tür sistemini kullanır. Bu seri hale getirme, iletinin farklı bir platformda çalışan bir alıcı uygulama tarafından alınıp yorumlanmasına olanak sağlar. Örneğin, Service Bus erişmek için JMS API kullanan bir Java uygulaması.

Bir [Brokeredmessage][BrokeredMessage] örneği oluşturduğunuzda, bir .net nesnesini, iletinin gövdesi olarak kullanılacak oluşturucuya bir parametre olarak sağlayabilirsiniz. AMQP ilkel türlerine eşlenemeyen nesneler için gövde AMQP veri türlerine serileştirilir. Nesne bir AMQP temel türü ile doğrudan eşlenemez; diğer bir deyişle, uygulama tarafından tanımlanan özel bir tür, ardından nesne [DataContractSerializer][DataContractSerializer]kullanılarak serileştirilir ve serileştirilmiş baytlar AMQP veri iletisinde gönderilir.

Non-.NET istemcilerle birlikte çalışabilirliği kolaylaştırmak için yalnızca ileti gövdesi için doğrudan AMQP türlerine seri hale getirilebilen .NET türlerini kullanın. Aşağıdaki tabloda bu türlerin ve AMQP türü sistemine karşılık gelen eşleme ayrıntıları verilmiştir.

| .NET Body nesne türü | Eşlenmiş AMQP türü | AMQP gövdesi bölüm türü |
| --- | --- | --- |
| bool |boole |AMQP değeri |
| byte |ubde |AMQP değeri |
| ushort |ushort |AMQP değeri |
| uint |uint |AMQP değeri |
| ulong |ulong |AMQP değeri |
| sbyte |byte |AMQP değeri |
| short |short |AMQP değeri |
| int |int |AMQP değeri |
| long |long |AMQP değeri |
| float |float |AMQP değeri |
| double |double |AMQP değeri |
| decimal |decimal128 |AMQP değeri |
| char |char |AMQP değeri |
| DateTime |timestamp |AMQP değeri |
| Guid |uuid |AMQP değeri |
| Byte [] |ikili |AMQP değeri |
| string |string |AMQP değeri |
| System. Collections. IList |list |AMQP değeri: koleksiyonda bulunan öğeler yalnızca bu tabloda tanımlanmış olanlar olabilir. |
| System. Array |array |AMQP değeri: koleksiyonda bulunan öğeler yalnızca bu tabloda tanımlanmış olanlar olabilir. |
| System. Collections. IDictionary |map |AMQP değeri: koleksiyonda bulunan öğeler yalnızca bu tabloda tanımlanmış olanlar olabilir. Note: yalnızca dize anahtarları desteklenir. |
| Kullanılmamışsa |Açıklanan dize (aşağıdaki tabloya bakın) |AMQP değeri |
| DateTimeOffset |Açıklanan süre (aşağıdaki tabloya bakın) |AMQP değeri |
| TimeSpan |Açıklanan süre (aşağıdakilere bakın) |AMQP değeri |
| Akış |ikili |AMQP verileri (birden fazla olabilir). Veri bölümleri Stream nesnesinden okunan ham baytları içerir. |
| Diğer nesne |ikili |AMQP verileri (birden fazla olabilir). Uygulama tarafından sağlanan DataContractSerializer veya seri hale getirici kullanan nesnenin seri hale getirilmiş ikilisini içerir. |

| .NET türü | Eşlenmiş AMQP ile tanımlanmış tür | Notlar |
| --- | --- | --- |
| Kullanılmamışsa |`<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>` |Uri. AbsoluteUri |
| DateTimeOffset |`<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` |DateTimeOffset. UtcTicks |
| TimeSpan |`<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type>` |TimeSpan. Ticks |

## <a name="behavioral-differences"></a>Davranış farkları

Varsayılan protokolle karşılaştırıldığında AMQP kullanılırken Service Bus .NET API 'SI davranışında bazı küçük farklılıklar vardır:

* [OperationTimeout][OperationTimeout] özelliği yoksayıldı.
* `MessageReceiver.Receive(TimeSpan.Zero)`olarak uygulanır `MessageReceiver.Receive(TimeSpan.FromSeconds(10))` .
* Kilit belirteçlerine göre iletilerin tamamlanması, yalnızca ilk olarak iletileri alan ileti alıcıları tarafından yapılabilir.

## <a name="control-amqp-protocol-settings"></a>AMQP protokol ayarlarını denetleme

[.NET API 'leri](/dotnet/api/) AMQP protokolünün davranışını denetlemek için çeşitli ayarlar sunar:

* **[MessageReceiver. PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagereceiver.prefetchcount?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessageReceiver_PrefetchCount)**: bir bağlantıya uygulanan ilk kredisi denetler. Varsayılan değer, 0'dur.
* **[Messagingfactorysettings. AmqpTransportSettings. MaxFrameSize](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.maxframesize?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_MaxFrameSize)**: bağlantı açma zamanında anlaşma sırasında sunulan en yüksek AMQP çerçeve boyutunu denetler. Varsayılan değer 65.536 bayttır.
* **[MessagingFactorySettings.AmqpTransportSettings.BatChflushınterval](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.batchflushinterval?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_BatchFlushInterval)**: aktarımlar batchable ise, bu değer deklerin gönderilmesi için maksimum gecikme sayısını belirler. Varsayılan olarak Gönderenler/alıcılar tarafından devralınır. Bireysel gönderici/alıcı varsayılan değer olan 20 milisaniyeyi geçersiz kılabilir.
* **[Messagingfactorysettings. AmqpTransportSettings. UseSslStreamSecurity](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.usesslstreamsecurity?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_UseSslStreamSecurity)**: AMQP BAĞLANTıLARıNıN bir TLS bağlantısı üzerinden oluşturulup yüklenmediğini denetler. Varsayılan değer **true**'dur.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmeye hazır mısınız? Aşağıdaki bağlantıları ziyaret edin:

* [Service Bus AMQP 'ye Genel Bakış]
* [AMQP 1.0 protokol kılavuzu]

[Create a Service Bus namespace using the Azure portal]: service-bus-create-namespace-portal.md
[DataContractSerializer]: https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azureservicebus-4.0.0
[Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory.acceptmessagesession?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactory_AcceptMessageSession
[OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout
[NuGet]: https://nuget.org/packages/WindowsAzure.ServiceBus/
[Azure portal]: https://portal.azure.com
[Service Bus AMQP 'ye Genel Bakış]: service-bus-amqp-overview.md
[AMQP 1.0 protokol kılavuzu]: service-bus-amqp-protocol-guide.md

