---
title: Azure Service Bus için sorun giderme kılavuzu | Microsoft Docs
description: Service Bus mesajlaşma özel durumları ve önerilen eylemler listesi.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 3d8526fe-6e47-4119-9f3e-c56d916a98f9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/17/2020
ms.author: aschhab
ms.openlocfilehash: 20d9fc141fa19a5c6d01f33c2a5398ca84497b9f
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76309974"
---
# <a name="troubleshooting-guide-for-azure-service-bus"></a>Azure Service Bus için sorun giderme kılavuzu
Bu makalede, Service Bus .NET Framework API 'Leri tarafından oluşturulan bazı .NET özel durumları ve sorun giderme sorunları için başka ipuçları sunulmaktadır. 

## <a name="service-bus-messaging-exceptions"></a>Hizmet Veri Yolu mesajlaşma özel durumları
Bu bölümde, .NET Framework API 'Leri tarafından oluşturulan .NET özel durumları listelenmektedir. 

### <a name="exception-categories"></a>Özel durum kategorisi
Mesajlaşma API 'Leri, aşağıdaki kategorilere ayrılan özel durumlar oluşturur ve bunları gidermeyi denemek için uygulayabileceğiniz ilgili eyleme sahip olabilir. Bir özel durumun anlamı ve nedenleri, mesajlaşma varlığının türüne göre farklılık gösterebilir:

1. Kullanıcı kodlama hatası ([System. ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System. InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System. Operationolaydexception](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [System. Runtime. Serialization. SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx)). Genel eylem: devam etmeden önce kod düzeltmeye çalışın.
2. Kurulum/yapılandırma hatası ([Microsoft. ServiceBus. Messaging. MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.servicebus.messagingentitynotfoundexception), [System. UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Genel eylem: yapılandırmanızı inceleyin ve gerekirse değiştirin.
3. Geçici özel durumlar ([Microsoft. ServiceBus. Messaging. MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft. ServiceBus. Messaging. serverbusyıexception](/dotnet/api/microsoft.azure.servicebus.serverbusyexception), [Microsoft. ServiceBus. Messaging. MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception)). Genel eylem: işlemi yeniden deneyin veya kullanıcılara bildirin. İstemci SDK 'sindeki `RetryPolicy` sınıfı, yeniden denemeleri otomatik olarak işleyecek şekilde yapılandırılabilir. Daha fazla bilgi için bkz. [yeniden deneme Kılavuzu](/azure/architecture/best-practices/retry-service-specific#service-bus).
4. Diğer özel durumlar ([System. Transactions. TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System. TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx), [Microsoft. ServiceBus. Messaging. MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception), [Microsoft. ServiceBus. Messaging. sessionlocklostexception](/dotnet/api/microsoft.azure.servicebus.sessionlocklostexception)). Genel eylem: özel durum türüne özgü; Aşağıdaki bölümdeki tabloya bakın: 

### <a name="exception-types"></a>Özel durum türleri
Aşağıdaki tabloda, Mesajlaşma özel durum türlerini ve nedenler ve notları önerilen eylem uygulayabileceğiniz listeler.

| **Özel durum türü** | **Açıklama/neden/örnekler** | **Önerilen eylem** | **Otomatik/anında yeniden denemeye göz atma** |
| --- | --- | --- | --- |
| [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |Sunucu, belirtilen süre içinde, [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings)tarafından denetlenen istenen işleme yanıt vermedi. Sunucu istenen işlemi tamamlanmamış olabilir. Ağ veya diğer altyapı gecikmelerinden dolayı bu durum oluşabilir. |Tutarlılık için sistem durumunu denetleyin ve gerekirse yeniden deneyin. Bkz. [zaman aşımı özel durumları](#timeoutexception). |Bazı durumlarda yeniden başlatma yardımcı; yeniden deneme mantığı için kod ekleyin. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |İstenen Kullanıcı işlemine sunucu veya hizmet içinde izin verilmiyor. Özel durum iletisi ayrıntıları için bkz. Örneğin, [tamamlanmış ()](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) Ileti [receiveanddelete](/dotnet/api/microsoft.azure.servicebus.receivemode) modunda alınmışsa bu özel durumu oluşturur. |Kod ve belgelere bakın. İstenen işlem geçerli olduğundan emin olun. |Yeniden deneme yardım etmez. |
| [OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Zaten kapalı, durduruldu veya atılmış bir nesne üzerinde bir işlem çağırmak için bir deneme yapılır. Nadiren de olsa, ortam işlem zaten atıldı. |Kodu denetleyin ve çıkarılan bir nesne üzerinde işlemleri çağırmadığına emin olun. |Yeniden deneme yardım etmez. |
| [UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |[TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) nesnesi belirteç alamadı, belirteç geçersiz veya belirteç işlemi yapmak için gereken talepleri içermiyor. |Belirteç sağlayıcısı, doğru değerlerle oluşturulduğundan emin olun. Access Control Service yapılandırmasını denetleyin. |Bazı durumlarda yeniden başlatma yardımcı; yeniden deneme mantığı için kod ekleyin. |
| [ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [ArgumentNullException](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[Üretiliyor](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Yöntemine sağlanan bir veya daha fazla bağımsız değişken geçersiz.<br /> Sağlanan URI [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [Oluştur](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) yolu segment(s) içerir.<br /> Sağlanan URI düzeni [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [Oluştur](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) geçersiz. <br />Özellik değeri, 32 KB'den daha büyük. |Çağıran kod denetleyin ve bağımsız değişkenlerin doğru olduğundan emin olun. |Yeniden deneme yardım etmez. |
| [MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.servicebus.messagingentitynotfoundexception) |İşlemle ilişkili varlık yok veya silinmiş. |Varlığın mevcut olduğundan emin olun. |Yeniden deneme yardım etmez. |
| [MessageNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagenotfoundexception) |Belirli bir sıra numarasına sahip bir ileti alma girişimi. Bu ileti bulunamadı. |İletinin zaten alınmadığından emin olun. İletinin kaldırılmış olup olmadığını görmek için sahipsiz sırayı kontrol edin. |Yeniden deneme yardım etmez. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) |İstemci Service Bus bir bağlantı kuramıyor. |Sağlanan ana bilgisayar adının doğru olduğundan ve konağın erişilebilir olduğundan emin olun. |Aralıklı bağlantı sorunu yoksa, yeniden deneme yardımcı olabilir. |
| [ServerBusyException](/dotnet/api/microsoft.azure.servicebus.serverbusyexception) |Hizmet şu anda isteği işleyemiyor. |İstemci bir süre bekleyin. ardından işlemi yeniden deneyin. |İstemci, belirli bir süre sonra yeniden deneyebilir. Farklı bir özel durum yeniden oluşur, o özel durumu yeniden deneme davranışını kontrol edin. |
| [MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception) |İletiyle ilişkili kilit belirtecinin süresi doldu veya kilit belirteci bulunamadı. |İletiyi atın. |Yeniden deneme yardım etmez. |
| [SessionLockLostException](/dotnet/api/microsoft.azure.servicebus.sessionlocklostexception) |Bu oturumla ilişkili kilit kayboldu. |[Messagesession](/dotnet/api/microsoft.servicebus.messaging.messagesession) nesnesini iptal edin. |Yeniden deneme yardım etmez. |
| [Istransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception) |Aşağıdaki durumlarda oluşabilecek genel mesajlaşma özel durumu:<br /> Farklı bir varlık türüne (örneğin, bir konu) ait olan bir ad veya yol kullanarak bir [Queueclient](/dotnet/api/microsoft.azure.servicebus.queueclient) oluşturmak için girişimde bulunuldu.<br />  256 KB'den daha büyük bir ileti göndermek için bir deneme yapılır. Sunucu veya hizmet isteğinin işlenmesi sırasında bir hatayla karşılaştı. Özel durum iletisi ayrıntıları için bkz. Genellikle geçici bir özel durumdur. |Kodunu kontrol edin ve yalnızca serileştirilebilir nesneler ileti gövdesi için kullanıldığından emin olun (veya özel bir serileştirici kullanın). Özelliklerin desteklenen değer türleri ve yalnızca desteklenen kullanım türleri için belgelere bakın. Denetleme [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception) özelliği. **Doğru**ise işlemi yeniden deneyebilirsiniz. |Yeniden deneme davranışı tanımsızdır ve yardımcı. |
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) |Bu hizmet ad alanındaki başka bir varlık tarafından zaten kullanılan bir ada sahip bir varlık oluşturmaya çalışır. |Var olan bir varlığa silin veya oluşturulacak varlığın farklı bir ad seçin. |Yeniden deneme yardım etmez. |
| [QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) |Mesajlaşma varlığı izin verilen en büyük boyuta ulaştı veya bir ad alanına yönelik bağlantı sayısı üst sınırı aşıldı. |Alanı varlık içinde varlık veya onun alt iletilerini alma oluşturun. Bkz. [QuotaExceededException](#quotaexceededexception). |İletiler sırada kaldırdıysanız, yeniden deneme yardımcı olabilir. |
| [RuleActionException](/dotnet/api/microsoft.servicebus.messaging.ruleactionexception) |Service Bus geçersiz bir kural eylemi oluşturmayı denerseniz bu özel durumu döndürür. Service Bus, söz konusu ileti için kural eylemi işlenirken bir hata oluşursa, bu özel durumu bir hatalı iletiye iliştirir. |Kural eyleminin doğruluğunu denetleyin. |Yeniden deneme yardım etmez. |
| [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) |Service Bus geçersiz bir filtre oluşturmayı denerseniz bu özel durumu döndürür. Service Bus bu özel durumu, bu ileti için filtre işlenirken bir hata oluştuysa bir iletiye iliştirir. |Filtreyi doğruluk açısından denetleyin. |Yeniden deneme yardım etmez. |
| [SessionCannotBeLockedException](/dotnet/api/microsoft.servicebus.messaging.sessioncannotbelockedexception) |Belirli bir oturum KIMLIĞIYLE oturumu kabul etme girişimi, ancak oturum şu anda başka bir istemci tarafından kilitlenmiş. |Oturumun kilidinin başka istemciler tarafından açık olduğundan emin olun. |Oturumun geçici olarak yayınlanmışsa, yeniden deneme yardımcı olabilir. |
| [TransactionSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.transactionsizeexceededexception) |İşlemin parçası çok fazla sayıda işlem. |Bu işlemin parçası olan işlem sayısını azaltın. |Yeniden deneme yardım etmez. |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.azure.servicebus.messagingentitydisabledexception) |Bir çalışma zamanı işlemi devre dışı bırakılmış bir varlık için isteği. |Varlık etkinleştirin. |Bu arada varlık etkinleştirdiyseniz, yeniden deneme yardımcı olabilir. |
| [NoMatchingSubscriptionException](/dotnet/api/microsoft.servicebus.messaging.nomatchingsubscriptionexception) |Service Bus, ön filtreleme etkin olan ve filtrelerin hiçbirinde eşleşmeyen bir konuya ileti gönderirseniz bu özel durumu döndürür. |En az bir filtrenin eşleştiğinden emin olun. |Yeniden deneme yardım etmez. |
| [Iletiizeexceededexception](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |İleti yükü 256 KB sınırını aşıyor. 256-KB sınırı, sistem özelliklerini ve tüm .NET ek yükünü içerebilen toplam ileti boyutudur. |İleti yükü azaltın, ardından işlemi yeniden deneyin. |Yeniden deneme yardım etmez. |
| [TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx) |Çevresel işlem (*Transaction. Current*) geçersiz. Tamamlanmış veya durdurulmuş olabilir. İç özel durum, ek bilgi sağlayabilir. | |Yeniden deneme yardım etmez. |
| [TransactionInDoubtException](https://msdn.microsoft.com/library/system.transactions.transactionindoubtexception.aspx) |Şüpheli bir işlem üzerinde bir işlem yapılmaya çalışıldı veya işlemi yürütmek için bir girişimde bulunuldu ve işlem şüpheli olur. |İşlem zaten kaydedilmiş olabileceğinden, uygulamanız bu özel durumu (özel bir durum olarak) işlemelidir. |- |

### <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception), belirli bir varlık için belirlenen kotanın aşıldığını gösterir.

#### <a name="queues-and-topics"></a>Kuyruklar ve konular
Kuyruklar ve konular için genellikle kuyruğun boyutudur. Hata iletisi özelliği, aşağıdaki örnekte olduğu gibi daha ayrıntılı bilgiler içerir:

```Output
Microsoft.ServiceBus.Messaging.QuotaExceededException
Message: The maximum entity size has been reached or exceeded for Topic: ‘xxx-xxx-xxx’. 
    Size of entity in bytes:1073742326, Max entity size in bytes:
1073741824..TrackingId:xxxxxxxxxxxxxxxxxxxxxxxxxx, TimeStamp:3/15/2013 7:50:18 AM
```

İleti, konunun boyut sınırını aştığını, bu durumda 1 GB (varsayılan boyut sınırı) olduğunu belirtir. 

#### <a name="namespaces"></a>Ad Alanları

Ad alanları için [QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) , bir uygulamanın bir ad alanına yönelik en fazla bağlantı sayısını aştığını gösterebilir. Örneğin:

```Output
Microsoft.ServiceBus.Messaging.QuotaExceededException: ConnectionsQuotaExceeded for namespace xxx.
<tracking-id-guid>_G12 ---> 
System.ServiceModel.FaultException`1[System.ServiceModel.ExceptionDetail]: 
ConnectionsQuotaExceeded for namespace xxx.
```

#### <a name="common-causes"></a>Olası nedenler
Bu hatanın yaygın iki nedeni vardır: atılacak ileti sırası ve çalışır olmayan ileti alıcıları.

1. **[Atılacak ileti sırası](service-bus-dead-letter-queues.md)** Bir okuyucu iletileri tamamlamayacak ve kilit sona erdiğinde iletiler kuyruğa/konuya döndürülür. Okuyucu, [Brokeredmessage. tamamlanmıştır](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.complete)öğesini aramasını önleyen bir özel durumla karşılaştığında gerçekleşebilir. Bir ileti 10 kez okunduktan sonra varsayılan olarak atılacak ileti kuyruğuna gider. Bu davranış, [Queuedescription. MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount) özelliği tarafından denetlenir ve varsayılan değeri 10 ' dur. İletiler, atılacak ileti kuyruğunda yer aldığı sırada yer kaplar.
   
    Bu sorunu çözmek için, başka bir kuyruktan yaptığınız gibi atılacak ileti sırasından iletileri okuyun ve doldurun. Atılacak ileti sırası yolunu biçimlendirmeye yardımcı olması için [FormatDeadLetterPath](/dotnet/api/microsoft.azure.servicebus.entitynamehelper.formatdeadletterpath) yöntemini kullanabilirsiniz.
2. **Alıcı durdu**. Alıcı bir kuyruktan veya abonelikten ileti almayı durdurdu. Bunu belirlemenin yolu, iletilerin tam dökümünü gösteren [Queuedescription. MessageCountDetails](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails) özelliğine bakabilmenizdir. [Activemessagecount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.activemessagecount) özelliği yüksek veya daha fazla olursa iletiler yazıldığı kadar hızlı okunmazlar.

### <a name="timeoutexception"></a>TimeoutException
A [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) kullanıcı tarafından başlatılan bir işlemin işlemi zaman aşımı süresinden daha uzun sürdüğünü gösterir. 

[ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) özelliğinin değerini denetlemeniz gerekir, çünkü bu sınıra ulaşarak bir [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx)de neden olabilir.

#### <a name="queues-and-topics"></a>Kuyruklar ve konular
Kuyruklar ve konular için, zaman aşımı, bağlantı dizesinin parçası olarak ya da [Servicebusconnectionstringbuilder](/dotnet/api/microsoft.azure.servicebus.servicebusconnectionstringbuilder)aracılığıyla [Messagingfactorysettings. OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings) özelliğinde belirtilir. Hata iletisi farklılık gösterebilir, ancak her zaman geçerli işlem için belirtilen zaman aşımı değeri içerir. 

## <a name="connectivity-certificate-or-timeout-issues"></a>Bağlantı, sertifika veya zaman aşımı sorunları
Aşağıdaki adımlar *. servicebus.windows.net altındaki tüm hizmetlerde bağlantı/sertifika/zaman aşımı sorunlarını gidermenize yardımcı olabilir. 

- Veya [wget](https://www.gnu.org/software/wget/) `https://<yournamespace>.servicebus.windows.net/`gidin. IP filtrelemesi veya sanal ağ ya da sertifika zinciri sorunları olup olmadığını denetlemeye yardımcı olur (Java SDK kullanırken en yaygın olarak).

    Başarılı bir ileti örneği:
    
    ```xml
    <feed xmlns="http://www.w3.org/2005/Atom"><title type="text">Publicly Listed Services</title><subtitle type="text">This is the list of publicly-listed services currently available.</subtitle><id>uuid:27fcd1e2-3a99-44b1-8f1e-3e92b52f0171;id=30</id><updated>2019-12-27T13:11:47Z</updated><generator>Service Bus 1.1</generator></feed>
    ```
    
    Hata iletisi örneği:

    ```json
    <Error>
        <Code>400</Code>
        <Detail>
            Bad Request. To know more visit https://aka.ms/sbResourceMgrExceptions. . TrackingId:b786d4d1-cbaf-47a8-a3d1-be689cda2a98_G22, SystemTracker:NoSystemTracker, Timestamp:2019-12-27T13:12:40
        </Detail>
    </Error>
    ```
- Güvenlik duvarında herhangi bir bağlantı noktasının engellenip engellenmediğini denetlemek için aşağıdaki komutu çalıştırın. Kullanılan bağlantı noktaları 443 (HTTPS), 5671 (AMQP) ve 9354 (net mesajlaşma/SBMP). Kullandığınız kitaplığa bağlı olarak diğer bağlantı noktaları da kullanılır. 5671 bağlantı noktasının engellenip engellenmeyeceğini kontrol eden örnek komut aşağıda verilmiştir. 

    ```powershell
    tnc <yournamespacename>.servicebus.windows.net -port 5671
    ```

    Linux üzerinde:

    ```shell
    telnet <yournamespacename>.servicebus.windows.net 5671
    ```
- Aralıklı bağlantı sorunları olduğunda, bırakılan herhangi bir paket olup olmadığını denetlemek için aşağıdaki komutu çalıştırın. Bu komut, hizmeti ile her 1 saniyede 25 farklı TCP bağlantısı kurmaya çalışacaktır. Sonra, kaç tane başarılı/başarısız olduğunu denetleyebilir ve ayrıca TCP bağlantı gecikmesini de görebilirsiniz. `psping` aracını [buradan](/sysinternals/downloads/psping)indirebilirsiniz.

    ```shell
    .\psping.exe -n 25 -i 1 -q <yournamespace>.servicebus.windows.net:5671 -nobanner     
    ```
    `tnc`, `ping`vb. gibi diğer araçları kullanıyorsanız eşdeğer komutları kullanabilirsiniz. 
- Önceki adımlar [Wireshark](https://www.wireshark.org/)gibi araçları kullanarak yardımcı değilse ve analiz yoksa bir ağ izlemesi elde edin. Gerekirse [Microsoft desteği](https://support.microsoft.com/) başvurun. 


## <a name="next-steps"></a>Sonraki adımlar

.NET API başvurusu Service Bus için, bkz. [Azure .NET API başvurusu](/dotnet/api/overview/azure/service-bus).

[Service Bus](https://azure.microsoft.com/services/service-bus/)hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Service Bus mesajlaşma hizmetine genel bakış](service-bus-messaging-overview.md)
* [Service Bus mimarisi](service-bus-architecture.md)

