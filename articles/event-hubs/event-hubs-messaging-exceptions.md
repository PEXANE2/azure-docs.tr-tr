---
title: Sorun giderme kılavuzu-Azure Event Hubs | Microsoft Docs
description: Bu makalede, Azure Event Hubs Mesajlaşma özel durumları ve Önerilen Eylemler listesini sağlar.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 01/16/2020
ms.author: shvija
ms.openlocfilehash: 26056e9b52ea319856505db837c67dc68b2f4aa6
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76157296"
---
# <a name="troubleshooting-guide-for-azure-event-hubs"></a>Azure Event Hubs için sorun giderme kılavuzu
Bu makalede, Event Hubs .NET Framework API 'Leri tarafından oluşturulan bazı .NET özel durumları ve sorun giderme sorunları için başka ipuçları sunulmaktadır. 

## <a name="event-hubs-messaging-exceptions---net"></a>Event Hubs mesajlaşma özel durumları-.NET
Bu bölümde, .NET Framework API 'Leri tarafından oluşturulan .NET özel durumları listelenmektedir. 

### <a name="exception-categories"></a>Özel durum kategorisi

Event Hubs .NET API 'Leri, çözümü gidermeye çalışmak için uygulayabileceğiniz ilgili eylemle birlikte, aşağıdaki kategorilere ayrılan özel durumlar oluşturur.

1. Kodlama hatası kullanıcı: [System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [ System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx). Genel eylem: devam etmeden önce kod düzeltmeye çalışın.
2. Kurulumu/yapılandırma hatası: [Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception), [Microsoft.Azure.EventHubs.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception), [ System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Genel eylem: yapılandırmanızı inceleyin ve gerekirse değiştirin.
3. Geçici özel durumlar: [Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](#serverbusyexception), [ Microsoft.Azure.EventHubs.ServerBusyException](#serverbusyexception), [Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception). Genel eylem: işlemi yeniden deneyin veya kullanıcılara bildirin.
4. Diğer özel durumlar: [System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](#timeoutexception), [Microsoft.ServiceBus.Messaging.MessageLockLostException](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception), [Microsoft.ServiceBus.Messaging.SessionLockLostException](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception). Genel eylem: özel durum türü; özel aşağıdaki bölümdeki tabloya bakın. 

### <a name="exception-types"></a>Özel durum türleri
Aşağıdaki tabloda, Mesajlaşma özel durum türlerini ve nedenler ve notları önerilen eylem uygulayabileceğiniz listeler.

| Özel Durum Türü | Neden/açıklama/örnekleri | Önerilen eylem | Otomatik/hemen yeniden deneme sırasında dikkat edin. |
| -------------- | -------------------------- | ---------------- | --------------------------------- |
| [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |Sunucu, belirtilen süre içinde, [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings)tarafından denetlenen istenen işleme yanıt vermedi. Sunucu istenen işlemi tamamlanmamış olabilir. Bu özel durum, ağ veya diğer altyapı gecikmelerinden kaynaklanabilir. |Tutarlılık için sistem durumunu denetleyin ve gerekirse yeniden deneyin.<br /> Bkz: [TimeoutException](#timeoutexception). | Bazı durumlarda yeniden başlatma yardımcı; yeniden deneme mantığı için kod ekleyin. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |İstenen Kullanıcı işlemine sunucu veya hizmet içinde izin verilmiyor. Özel durum iletisi ayrıntıları için bkz. Örneğin, [tam](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) iletisi alındı bu özel durum oluşturur [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) modu. | Kod ve belgelere bakın. İstenen işlem geçerli olduğundan emin olun. | Yeniden deneme yardımcı olmayacaktır. |
| [OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) | Zaten kapalı, durduruldu veya atılmış bir nesne üzerinde bir işlem çağırmak için bir deneme yapılır. Nadiren de olsa, ortam işlem zaten atıldı. | Kodu denetleyin ve çıkarılan bir nesne üzerinde işlemleri çağırmadığına emin olun. | Yeniden deneme yardımcı olmayacaktır. |
| [UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) | [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) nesnesi belirteç alamadı, belirteç geçersiz veya belirteç işlemi yapmak için gereken talepleri içermiyor. | Belirteç sağlayıcısı, doğru değerlerle oluşturulduğundan emin olun. Access Control Service yapılandırmasını denetleyin. | Bazı durumlarda yeniden başlatma yardımcı; yeniden deneme mantığı için kod ekleyin. |
| [ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [ArgumentNullException](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[Üretiliyor](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) | Yöntemine sağlanan bir veya daha fazla bağımsız değişken geçersiz. Sağlanan URI [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [Oluştur](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) yolu segment(s) içerir. Sağlanan URI düzeni [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [Oluştur](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) geçersiz. Özellik değeri, 32 KB'den daha büyük. | Çağıran kod denetleyin ve bağımsız değişkenlerin doğru olduğundan emin olun. | Yeniden deneme yardımcı olmaz. |
| [Microsoft.ServiceBus.Messaging MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception) <br /><br/> [Microsoft.Azure.EventHubs MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception) | İşlemle ilişkili varlık yok veya silinmiş olabilir. | Varlığın mevcut olduğundan emin olun. | Yeniden deneme yardımcı olmaz. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) | İstemci, olay hub'ına bağlantı kuramıyor. |Sağlanan ana bilgisayar adının doğru olduğundan ve konağın erişilebilir olduğundan emin olun. | Aralıklı bağlantı sorunu yoksa, yeniden deneme yardımcı olabilir. |
| [Microsoft. ServiceBus. Messaging Serverbusyıexception](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) <br /> <br/>[Microsoft.Azure.EventHubs ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) | Hizmet şu anda isteğinizi mümkün değil. | İstemci bir süre bekleyin. ardından işlemi yeniden deneyin. <br /> Bkz: [ServerBusyException](#serverbusyexception). | İstemci, belirli bir süre sonra yeniden deneyebilir. Farklı bir özel durum yeniden oluşur, o özel durumu yeniden deneme davranışını kontrol edin. |
| [Istransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception) | Aşağıdaki durumlarda oluşturulan özel durum iletileri genel: oluşturmak için bir girişimde bir [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) adınızı veya ait farklı bir varlık türü için (örneğin, bir konu) yolu. 1 MB 'den büyük bir ileti göndermek için bir girişimde bulunuldu. Sunucu veya hizmet isteğinin işlenmesi sırasında bir hatayla karşılaştı. Özel durum iletisi ayrıntıları için bkz. Bu durum genellikle geçici bir istisnadır. | Kodunu kontrol edin ve yalnızca serileştirilebilir nesneler ileti gövdesi için kullanıldığından emin olun (veya özel bir serileştirici kullanın). Özelliklerin desteklenen değer türleri ve yalnızca desteklenen kullanım türleri için belgelere bakın. Denetleme [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception) özelliği. Eğer öyleyse **true**, işlemi yeniden deneyin. | Yeniden deneme davranışı tanımsızdır ve yardımcı. |
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) | Bu hizmet ad alanındaki başka bir varlık tarafından zaten kullanılan bir ada sahip bir varlık oluşturmaya çalışır. | Var olan bir varlığa silin veya oluşturulacak varlığın farklı bir ad seçin. | Yeniden deneme yardımcı olmaz. |
| [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) | Mesajlaşma varlığı, izin verilen boyut üst sınırına. Bu özel durumun (olan 5) sayısı alıcılar bir tüketici her grubu düzeyinde açılan oluşabilir. | Alanı varlık içinde varlık veya onun alt iletilerini alma oluşturun. <br /> Bkz: [QuotaExceededException](#quotaexceededexception) | İletiler sırada kaldırdıysanız, yeniden deneme yardımcı olabilir. |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.servicebus.messaging.messagingentitydisabledexception) | Bir çalışma zamanı işlemi devre dışı bırakılmış bir varlık için isteği. |Varlık etkinleştirin. | Bu arada varlık etkinleştirdiyseniz, yeniden deneme yardımcı olabilir. |
| [Microsoft.ServiceBus.Messaging MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) <br /><br/> [Microsoft.Azure.EventHubs MessageSizeExceededException](/dotnet/api/microsoft.azure.eventhubs.messagesizeexceededexception) | İleti yükü 1 MB sınırı aşıyor. Bu 1 MB 'lık sınır, sistem özelliklerini ve tüm .NET ek yükünü içerebilen toplam ileti içindir. | İleti yükü azaltın, ardından işlemi yeniden deneyin. |Yeniden deneme yardımcı olmaz. |

### <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception), belirli bir varlık için belirlenen kotanın aşıldığını gösterir.

Bu özel durum sayısı (5) alıcılar bir tüketici her grubu düzeyinde açılan oluşabilir.

#### <a name="event-hubs"></a>Event Hubs
Event Hubs olay hub'ı başına 20 tüketici grubu sınırı vardır. Daha fazla oluşturmaya çalıştığınızda, aldığınız bir [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception). 

### <a name="timeoutexception"></a>TimeoutException
A [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) kullanıcı tarafından başlatılan bir işlemin işlemi zaman aşımı süresinden daha uzun sürdüğünü gösterir. 

Event Hubs için zaman aşımı veya bağlantı dizesinin veya aracılığıyla bir parçası olarak belirtilen [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.servicebus.servicebusconnectionstringbuilder). Hata iletisi farklılık gösterebilir, ancak her zaman geçerli işlem için belirtilen zaman aşımı değeri içerir. 

#### <a name="common-causes"></a>Olası nedenler
Bu hata sık karşılaşılan iki nedeni vardır: yanlış yapılandırma veya bir geçici hizmet hatası.

1. **Yanlış yapılandırma** işlem zaman aşımı işletimsel koşul için çok küçük olabilir. İstemci SDK'sı işlemi zaman aşımı'için varsayılan değer 60 saniyedir. Kodunuzu değer olup olmadığını görmek için çok küçük bir şeye ayarlayın denetleyin. Ağ ve CPU kullanımının koşulu, belirli bir işlemin tamamlanma süresini etkileyebilir, bu nedenle işlem zaman aşımı küçük bir değere ayarlanamaz.
2. **Geçici bir hizmet hatası** bazen Event Hubs hizmeti istekleri işliyor; Örneğin, yüksek trafiği dönemlerinde gecikme. Bu gibi durumlarda, işlemi başarılı olana kadar bir gecikmeden sonra işlemi yeniden deneyebilirsiniz. Aynı işlemi birden çok denemeden sonra yine başarısız olursa ziyaret [Azure hizmet durumu sitesine](https://azure.microsoft.com/status/) herhangi bir bilinen hizmet kesintileri olup olmadığını görmek için.

### <a name="serverbusyexception"></a>ServerBusyException

A [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) veya [Microsoft.Azure.EventHubs.ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) bir sunucu aşırı yüklenmiş olduğunu gösterir. Bu özel durumun iki ilgili hata kodları vardır.

#### <a name="error-code-50002"></a>Hata kodu 50002

Bu hata iki nedenlerden birinden dolayı oluşabilir:

1. Yük, Olay Hub 'ındaki tüm bölümler arasında eşit olarak dağıtılmaz ve bir bölüm yerel üretilen iş birimi sınırlamasını aşmaktadır.
    
    Çözüm: bölüm Dağıtım stratejisi düzeltilmesi veya çalışırken [EventHubClient.Send(eventDataWithOutPartitionKey)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient) yardımcı olabilir.

2. Event Hubs ad alanı yeterli işleme birimi içermiyor (onaylamak için [Azure portal](https://portal.azure.com) Event Hubs ad alanı penceresinde **ölçümler** ekranını kontrol edebilirsiniz). Portal toplanmış (1 dakikalık) bilgileri gösterir, ancak aktarım hızını gerçek zamanlı olarak ölçyoruz; bu nedenle yalnızca bir tahmindir.

    Çözüm: ad alanı üzerinde üretilen iş birimleri artırılması yardımcı olabilir. Portal, bu işlemi de yapabilirsiniz **ölçek** Event Hubs ad alanı ekranın penceresi. Veya, kullanabileceğiniz [otomatik şişme](event-hubs-auto-inflate.md).

#### <a name="error-code-50001"></a>Hata kodu 50001

Bu hata nadiren gerçekleşmesi. Ad alanınız için kod kapsayıcıyı CPU üzerinde düşük – başlamadan önce olay hub'ları yük dengeleyici fazla birkaç saniye ortaya çıkar.

#### <a name="limit-on-calls-to-the-getruntimeinformation-method"></a>GetRuntimeInformation yöntemiyle yapılan çağrıların sınırı
Azure Event Hubs, saniye başına Getruntimeınfo 'ya saniyede en fazla 50 çağrı destekler. Sınıra ulaşıldığında aşağıdakine benzer bir özel durum alabilirsiniz:

```
ExceptionId: 00000000000-00000-0000-a48a-9c908fbe84f6-ServerBusyException: The request was terminated because the namespace 75248:aaa-default-eventhub-ns-prodb2b is being throttled. Error code : 50001. Please wait 10 seconds and try again.
```

## <a name="connectivity-certificate-or-timeout-issues"></a>Bağlantı, sertifika veya zaman aşımı sorunları
Aşağıdaki adımlar *. servicebus.windows.net altındaki tüm hizmetlerde bağlantı/sertifika/zaman aşımı sorunlarını gidermenize yardımcı olabilir. 

- Veya [wget](https://www.gnu.org/software/wget/) `https://sbwagn2.servicebus.windows.net/`gidin. IP filtrelemesi veya sanal ağ ya da sertifika zinciri sorunları olup olmadığını denetlemeye yardımcı olur (Java SDK kullanırken en yaygın olarak).
- Güvenlik duvarında herhangi bir bağlantı noktasının engellenip engellenmediğini denetlemek için aşağıdaki komutu çalıştırın. Kullandığınız kitaplığa bağlı olarak diğer bağlantı noktaları da kullanılır. Örneğin: 443, 5672, 9354.

    ```powershell
    tnc sbwagn2.servicebus.windows.net -port 5671
    ```

    Linux üzerinde:

    ```shell
    telnet sbwagn2.servicebus.windows.net 5671
    ```
- Aralıklı bağlantı sorunları olduğunda, bırakılan herhangi bir paket olup olmadığını denetlemek için aşağıdaki komutu çalıştırın. Bu komut, hizmet ile her 1 saniyede 25 farklı TCP bağlantısı kurmaya çalışır, sonra kaç tane başarılı/başarısız olduğunu denetleyebilir ve TCP bağlantı gecikmesini de görebilirsiniz. `psping` aracını [buradan](/sysinternals/downloads/psping)indirebilirsiniz.

    ```shell
    .\psping.exe -n 25 -i 1 -q yournamespace.servicebus.windows.net:5671 -nobanner     
    ```
    `tnc`, `ping`vb. gibi diğer araçları kullanıyorsanız eşdeğer komutları kullanabilirsiniz. 
- Önceki adımlar yardım edip çözümlamazlarsa veya [Microsoft desteği](https://support.microsoft.com/)ile iletişim kurmazsanız bir ağ izlemesi elde edin. 

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
* [Olay Hub’ı oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
