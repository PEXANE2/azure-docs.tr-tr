---
title: Azure Servis Veri Servisi - mesajlaşma özel durumları | Microsoft Dokümanlar
description: Bu makalede, Azure Hizmet Veri Servisi mesajlaşma özel durumlarının bir listesi ve özel durum oluştuğunda alınması önerilen eylemler yer almaktadır.
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
ms.date: 03/23/2020
ms.author: aschhab
ms.openlocfilehash: d04902a8d53397b7e7d9712a1c75ce44cc7aa7ad
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80880797"
---
# <a name="service-bus-messaging-exceptions"></a>Servis Veri Servisi mesajlaşma istisnaları
Bu makalede, .NET Framework API'leri tarafından oluşturulan .NET özel durumları listelenir. 

## <a name="exception-categories"></a>Özel durum kategorileri
İleti API'leri, bunları düzeltmek için yapabileceğiniz ilişkili eylemle birlikte aşağıdaki kategorilere düşebilecek özel durumlar oluşturur. Bir özel durum anlamı ve nedenleri ileti varlık türüne bağlı olarak değişebilir:

1. Kullanıcı kodlama hatası ([System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [System.Runtime.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx)). Genel eylem: devam etmeden önce kodu düzeltmeye çalışın.
2. Kurulum/yapılandırma hatası ([Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.servicebus.messagingentitynotfoundexception), [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Genel eylem: yapılandırmanızı gözden geçirin ve gerekirse değiştirin.
3. Geçici özel durumlar ([Microsoft.ServiceBus.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.azure.servicebus.serverbusyexception), [Microsoft.ServiceBus.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception)). Genel eylem: işlemi yeniden deneyin veya kullanıcıları bilgilendirin. İstemci `RetryPolicy` SDK'daki sınıf, yeniden denemeleri otomatik olarak işletecek şekilde yapılandırılabilir. Daha fazla bilgi için [yeniden deneyin kılavuzuna](/azure/architecture/best-practices/retry-service-specific#service-bus)bakın.
4. Diğer istisnalar ([System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx), [Microsoft.ServiceBus.Messaging.MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception), [Microsoft.ServiceBus.Messaging.SessionLockLostException](/dotnet/api/microsoft.azure.servicebus.sessionlocklostexception)). Genel eylem: özel durum türüne özgü; aşağıdaki bölümdeki tabloya bakın: 

## <a name="exception-types"></a>Özel durum türleri
Aşağıdaki tabloda ileti özel durum türleri ve bunların nedenleri listelenecek ve notlar yapabileceğiniz eylemi önerilmektedir.

| **Özel Durum Türü** | **Açıklama/Neden/Örnekler** | **Önerilen Eylem** | **Otomatik/anında yeniden deneme notu** |
| --- | --- | --- | --- |
| [Timeoutexception](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |Sunucu, [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings)tarafından denetlenen belirtilen süre içinde istenen işlemi yanıt vermedi. Sunucu istenen işlemi tamamlamış olabilir. Bu ağ veya diğer altyapı gecikmeleri nedeniyle olabilir. |Tutarlılık ve gerekirse yeniden deneyin için sistem durumunu denetleyin. [Bkz. Zaman Arası özel durumları.](#timeoutexception) |Yeniden deneme bazı durumlarda yardımcı olabilir; koda yeniden deneme mantığı ekleyin. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |İstenen kullanıcı işlemine sunucu veya hizmet içinde izin verilmez. Ayrıntılar için özel durum iletisi bakın. Örneğin, Ileti [ReceiveAndDelete](/dotnet/api/microsoft.azure.servicebus.receivemode) modunda alındıysa, [Complete()](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) bu özel durumu oluşturur. |Kodu ve belgeleri kontrol edin. İstenen işlemin geçerli olduğundan emin olun. |Yeniden denemenin bir faydası yok. |
| [Operationcanceledexception](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Zaten kapatılmış, iptal edilmiş veya elden çıkarılmış bir nesne üzerinde bir işlem çağırma girişiminde bulunulması. Nadir durumlarda, ortam işlemi zaten bertaraf edilir. |Kodu denetleyin ve elden çıkarılan bir nesne üzerinde işlem başlatmadığından emin olun. |Yeniden denemenin bir faydası yok. |
| [Unauthorizedaccessexception](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |[TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) nesnesi bir belirteç elde edemedi, belirteç geçersiz veya belirteç işlemi yapmak için gerekli iddiaları içermez. |Belirteç sağlayıcısının doğru değerlerle oluşturulduğundan emin olun. Access Denetim Hizmeti'nin yapılandırmasını denetleyin. |Yeniden deneme bazı durumlarda yardımcı olabilir; koda yeniden deneme mantığı ekleyin. |
| [Argumentexception](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [Argumentnullexception](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[Argumentoutofrangeexception](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Yönteme verilen bir veya daha fazla bağımsız değişken geçersizdir.<br /> [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [Create'e](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) sağlanan URI yol segmenti(ler) içerir.<br /> [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [Create'e](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) verilen URI şeması geçersizdir. <br />Özellik değeri 32 KB'den büyüktür. |Arama kodunu denetleyin ve bağımsız değişkenlerin doğru olduğundan emin olun. |Yeniden denemenin bir faydası yok. |
| [MesajlaşmaEntityNotFoundException](/dotnet/api/microsoft.azure.servicebus.messagingentitynotfoundexception) |İşlemle ilişkili varlık yok veya silindi. |Varlığın var olduğundan emin olun. |Yeniden denemenin bir faydası yok. |
| [MessageNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagenotfoundexception) |Belirli bir sıra numarasına sahip bir ileti almaya çalış. Bu ileti bulunamadı. |İletinin zaten alınmadığından emin olun. İletinin deadletter edilip edilmedini görmek için deadletter sırasını kontrol edin. |Yeniden denemenin bir faydası yok. |
| [MesajlaşmaİletişimÖzel Durum](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) |İstemci Servis Veri Servisi ile bağlantı kuramaz. |Sağlanan ana bilgisayar adının doğru olduğundan ve ana bilgisayara ulaşıladığından emin olun. |Aralıklı bağlantı sorunları varsa yeniden deneme yardımcı olabilir. |
| [ServerBusyException](/dotnet/api/microsoft.azure.servicebus.serverbusyexception) |Hizmet şu anda isteği işleyemez. |İstemci bir süre bekleyebilir ve işlemi yeniden deneyebilir. |İstemci belirli bir süre sonra yeniden deneyebilir. Yeniden deneme farklı bir özel durumla sonuçlanırsa, bu özel durum la ilgili yeniden deneme davranışını denetleyin. |
| [MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception) |İletiyle ilişkili kilit belirteci süresi doldu veya kilit belirteci bulunamadı. |İletiyi atın. |Yeniden denemenin bir faydası yok. |
| [SessionLockLostException](/dotnet/api/microsoft.azure.servicebus.sessionlocklostexception) |Bu oturumla ilişkili kilit kaybolur. |[İleti Oturumu](/dotnet/api/microsoft.servicebus.messaging.messagesession) nesnesini iptal edin. |Yeniden denemenin bir faydası yok. |
| [MesajlaşmaÖzel Durum](/dotnet/api/microsoft.servicebus.messaging.messagingexception) |Aşağıdaki durumlarda atılabilecek genel ileti özel durumu:<p>Farklı bir varlık türüne (örneğin, bir konu) ait bir ad veya yol kullanarak bir [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) oluşturma girişimi yapılır.</p><p>256 KB'den daha büyük bir ileti gönderme girişiminde bulunuldu. </p>Sunucu veya hizmet, isteğin işlenmesi sırasında bir hatayla karşılaştı. Ayrıntılar için özel durum iletisi bakın. Genelde geçici bir istisnadır.</p><p>Varlık daraltıldığı için istek sonlandırıldı. Hata kodu: 50001, 50002, 50008. </p> | Kodu denetleyin ve ileti gövdesi için yalnızca serileştirilebilir nesnelerin kullanıldığından (veya özel bir serializer kullandığından) emin olun. <p>Özelliklerin desteklenen değer türleri için belgeleri denetleyin ve yalnızca desteklenen türleri kullanın.</p><p> [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception) özelliğini kontrol edin. Eğer **doğruysa,** işlemi yeniden deneyebilirsiniz. </p>| Özel durum azaltma nedeniyle ise, birkaç saniye bekleyin ve işlemi yeniden deneyin. Yeniden deneme davranışı tanımsızdır ve diğer senaryolarda yardımcı olmayabilir.|
| [MesajlaşmaEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) |Bu hizmet ad alanında başka bir varlık tarafından zaten kullanılan bir ada sahip bir varlık oluşturmaya çalış. |Varolan varlığı silin veya oluşturulacak varlık için farklı bir ad seçin. |Yeniden denemenin bir faydası yok. |
| [Quotaexceededexception](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) |İleti varlığı izin verilen en yüksek boyutuna ulaştı veya bir ad alanına en fazla bağlantı sayısı aşıldı. |Varlıktan veya alt sıralarından iletiler alarak varlıkta alan oluşturun. Bkz. [QuotaExceededException](#quotaexceededexception). |Bu arada iletiler kaldırıldıysa yeniden deneyin yardımcı olabilir. |
| [RuleActionException](/dotnet/api/microsoft.servicebus.messaging.ruleactionexception) |Geçersiz bir kural eylemi oluşturmaya çalışırsanız, Hizmet Veri Servisi bu özel durumu döndürür. Hizmet Veri Servisi, bu iletinin kural eylemini işlerken bir hata oluşursa, bu özel durumu ölü harfli iletiye bağlar. |Doğruluk için kural eylemini denetleyin. |Yeniden denemenin bir faydası yok. |
| [FiltreÖzel Durumu](/dotnet/api/microsoft.servicebus.messaging.filterexception) |Geçersiz bir filtre oluşturmaya çalışırsanız, Hizmet Veri Servisi bu özel durumu döndürür. Hizmet Veri Servisi, bu iletinin filtresini işlerken bir hata oluştuysa, bu özel durumu ölü harfli iletiye bağlar. |Doğruluk için filtreyi kontrol edin. |Yeniden denemenin bir faydası yok. |
| [OturumolamazKilitliÖzel Durum](/dotnet/api/microsoft.servicebus.messaging.sessioncannotbelockedexception) |Belirli bir oturum kimliğine sahip bir oturumu kabul etmeye çalışın, ancak oturum şu anda başka bir istemci tarafından kilitlenir. |Oturumun diğer istemciler tarafından kilitlendirildikinden emin olun. |Oturum geçici olarak serbest bırakılmışsa yeniden deneme yardımcı olabilir. |
| [İşlemBoyutAşıldıİst](/dotnet/api/microsoft.servicebus.messaging.transactionsizeexceededexception) |Çok fazla işlem işlemin bir parçasıdır. |Bu işlemin parçası olan işlem sayısını azaltın. |Yeniden denemenin bir faydası yok. |
| [MesajlaşmaEntityDisabledException](/dotnet/api/microsoft.azure.servicebus.messagingentitydisabledexception) |Devre dışı bırakılmış bir varlık üzerinde çalışma zamanı çalışması isteği. |Varlığı etkinleştirin. |Varlık geçici olarak etkinleştirilmişse yeniden deneme yardımcı olabilir. |
| [NoMatchingSubscriptionException](/dotnet/api/microsoft.servicebus.messaging.nomatchingsubscriptionexception) |Ön filtreleme etkin leştirilmiş ve filtrelerin hiçbiri eşleşmemiş bir konuya ileti gönderirseniz, Hizmet Veri Servisi bu özel durumu döndürür. |En az bir filtrenin eşleştiğinden emin olun. |Yeniden denemenin bir faydası yok. |
| [MesajBoyutuAşıldıÖzel Durum](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |İleti yükü 256-KB sınırını aşıyor. 256-KB sınırı, sistem özelliklerini ve herhangi bir .NET genel havasını içerebilen toplam ileti boyutudur. |İleti yükünün boyutunu küçültün ve işlemi yeniden deneyin. |Yeniden denemenin bir faydası yok. |
| [Transactionexception](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx) |Ortam hareketi *(Transaction.Current)* geçersizdir. Tamamlanmış veya iptal edilmiş olabilir. İç özel durum ek bilgi sağlayabilir. | |Yeniden denemenin bir faydası yok. |
| [Transactionındoubtexception](https://msdn.microsoft.com/library/system.transactions.transactionindoubtexception.aspx) |Şüpheli bir işlem üzerinde işlem denenir veya hareketi gerçekleştirme girişiminde bulunulr ve işlem şüpheye düşer. |Uygulamanız bu özel durumu (özel bir durum olarak) işlemeli, çünkü işlem zaten işlenmiş olabilir. |- |

## <a name="quotaexceededexception"></a>Quotaexceededexception
[QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception), belirli bir varlık için belirlenen kotanın aşıldığını gösterir.

### <a name="queues-and-topics"></a>Kuyruklar ve konular
Kuyruklar ve konular için genellikle sıranın boyutu. Hata iletisi özelliği, aşağıdaki örnekte olduğu gibi daha fazla ayrıntı içerir:

```Output
Microsoft.ServiceBus.Messaging.QuotaExceededException
Message: The maximum entity size has been reached or exceeded for Topic: 'xxx-xxx-xxx'. 
    Size of entity in bytes:1073742326, Max entity size in bytes:
1073741824..TrackingId:xxxxxxxxxxxxxxxxxxxxxxxxxx, TimeStamp:3/15/2013 7:50:18 AM
```

İleti, bu durumda 1 GB (varsayılan boyut sınırı) olarak, konunun boyut sınırını aştığını belirtir. 

### <a name="namespaces"></a>Ad Alanları

Ad alanları [için, QuotaExceededException,](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) bir uygulamanın ad alanına en fazla bağlantı sayısını aştığını gösterebilir. Örneğin:

```Output
Microsoft.ServiceBus.Messaging.QuotaExceededException: ConnectionsQuotaExceeded for namespace xxx.
<tracking-id-guid>_G12 ---> 
System.ServiceModel.FaultException`1[System.ServiceModel.ExceptionDetail]: 
ConnectionsQuotaExceeded for namespace xxx.
```

### <a name="common-causes"></a>Olası nedenler
Bu hatanın iki yaygın nedeni vardır: ölü harf sırası ve çalışmayan ileti alıcıları.

1. **[Ölü harf sırası](service-bus-dead-letter-queues.md)** Okuyucu iletileri tamamlayamamaktadır ve kilit süresi dolduğunda iletiler sıraya/konuya döndürülür. Okuyucu [BrokeredMessage.Complete'i](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.complete)aramasını engelleyen bir özel durumla karşılaşırsa gerçekleşebilir. Bir ileti 10 kez okunduktan sonra, varsayılan olarak ölü harf sırasına taşınır. Bu davranış [QueueDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount) özelliği tarafından denetlenir ve varsayılan değeri 10'dur. İletiler ölü harf kuyruğunda biriktikçe yer kaplarlar.
   
    Sorunu gidermek için, diğer kuyruktaki gibi ölü harf kuyruğundaki iletileri okuyun ve tamamlayın. Ölü harf sıra yolunu biçimlendirmeye yardımcı olmak için [FormatDeadLetterPath](/dotnet/api/microsoft.azure.servicebus.entitynamehelper.formatdeadletterpath) yöntemini kullanabilirsiniz.
2. **Alıcı durdu.** Alıcı, bir kuyruktan veya abonelikten ileti almayı durdurdu. Bunu tanımlamanın yolu, iletilerin tam dökümünü gösteren [QueueDescription.MessageCountDetails](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails) özelliğine bakmaktır. [ActiveMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.activemessagecount) özelliği yüksekveya büyüyorsa, iletiler yazıldığı kadar hızlı okunmuyor demektir.

## <a name="timeoutexception"></a>Timeoutexception
[TimeoutException,](https://msdn.microsoft.com/library/system.timeoutexception.aspx) kullanıcı tarafından başlatılan bir işlemin işlem zaman aşımından daha uzun sürdüğünü gösterir. 

[ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) özelliğinin değerini kontrol etmelisiniz, çünkü bu sınıra vurmak bir [TimeoutException'a](https://msdn.microsoft.com/library/system.timeoutexception.aspx)da neden olabilir.

### <a name="queues-and-topics"></a>Kuyruklar ve konular
Kuyruklar ve konular için zaman aşımı, bağlantı dizesinin bir parçası olarak veya [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.azure.servicebus.servicebusconnectionstringbuilder)aracılığıyla [MessagingFactorySettings.OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings) özelliğinde belirtilir. Hata iletisinin kendisi değişebilir, ancak her zaman geçerli işlem için belirtilen zaman adedini içerir. 

## <a name="next-steps"></a>Sonraki adımlar
Tam Hizmet Veri Mes .NET API başvurusu için [Azure .NET API başvurusuna](/dotnet/api/overview/azure/service-bus)bakın.
Sorun giderme ipuçları için [Sorun Giderme kılavuzuna](service-bus-troubleshooting-guide.md) bakın
