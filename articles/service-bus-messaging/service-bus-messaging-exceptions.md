---
title: Azure Service Bus mesajlaşma özel durumları | Microsoft Docs
description: Bu makale, özel durum oluştuğunda gerçekleştirilecek Azure Service Bus mesajlaşma özel durumlarının ve önerilen eylemlerin bir listesini sağlar.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 3c648cdce87b26e6258ff8bc25506ca2ebd3bbd9
ms.sourcegitcommit: d8b8768d62672e9c287a04f2578383d0eb857950
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88064613"
---
# <a name="service-bus-messaging-exceptions"></a>Service Bus mesajlaşma özel durumları
Bu makalede, .NET Framework API 'Leri tarafından oluşturulan .NET özel durumları listelenmektedir. 

## <a name="exception-categories"></a>Özel durum kategorileri
Mesajlaşma API 'Leri, aşağıdaki kategorilere ayrılan özel durumlar oluşturur ve bunları gidermeyi denemek için uygulayabileceğiniz ilgili eyleme sahip olabilir. Bir özel durumun anlamı ve nedenleri, mesajlaşma varlığının türüne göre farklılık gösterebilir:

1. Kullanıcı kodlama hatası ([System. ArgumentException](/dotnet/api/system.argumentexception?view=netcore-3.1), [System. InvalidOperationException](/dotnet/api/system.invalidoperationexception?view=netcore-3.1), [System. Operationolaydexception](/dotnet/api/system.operationcanceledexception?view=netcore-3.1), [System. Runtime. Serialization. SerializationException](/dotnet/api/system.runtime.serialization.serializationexception?view=netcore-3.1)). Genel eylem: devam etmeden önce kodu gidermeyi deneyin.
2. Kurulum/yapılandırma hatası ([Microsoft. ServiceBus. Messaging. MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.servicebus.messagingentitynotfoundexception), [System. UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception?view=netcore-3.1). Genel eylem: yapılandırmanızı gözden geçirin ve gerekirse değiştirin.
3. Geçici özel durumlar ([Microsoft. ServiceBus. Messaging. MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft. ServiceBus. Messaging. serverbusyıexception](/dotnet/api/microsoft.azure.servicebus.serverbusyexception), [Microsoft. ServiceBus. Messaging. MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception)). Genel eylem: işlemi yeniden deneyin veya kullanıcılara bildirin. `RetryPolicy`İstemci SDK 'sının sınıfı, yeniden denemeleri otomatik olarak işleyecek şekilde yapılandırılabilir. Daha fazla bilgi için bkz. [yeniden deneme Kılavuzu](/azure/architecture/best-practices/retry-service-specific#service-bus).
4. Diğer özel durumlar ([System. Transactions. TransactionException](/dotnet/api/system.transactions.transactionexception?view=netcore-3.1), [System. TimeoutException](/dotnet/api/system.timeoutexception?view=netcore-3.1), [Microsoft. ServiceBus. Messaging. MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception), [Microsoft. ServiceBus. Messaging. sessionlocklostexception](/dotnet/api/microsoft.azure.servicebus.sessionlocklostexception)). Genel eylem: özel durum türüne özgü; Aşağıdaki bölümdeki tabloya bakın: 

## <a name="exception-types"></a>Özel durum türleri
Aşağıdaki tabloda mesajlaşma özel durum türleri ve nedenleri ve gerçekleştirebileceğiniz notlar önerilir.

| **Özel durum türü** | **Açıklama/neden/örnekler** | **Önerilen eylem** | **Otomatik/anında yeniden denemeye göz atma** |
| --- | --- | --- | --- |
| [TimeoutException](/dotnet/api/system.timeoutexception?view=netcore-3.1) |Sunucu, belirtilen süre içinde, [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings)tarafından denetlenen istenen işleme yanıt vermedi. Sunucu istenen işlemi tamamlamış olabilir. Ağ veya diğer altyapı gecikmelerinden dolayı bu durum oluşabilir. |Tutarlılık için sistem durumunu kontrol edin ve gerekirse yeniden deneyin. Bkz. [zaman aşımı özel durumları](#timeoutexception). |Yeniden deneme bazı durumlarda yardımcı olabilirler; koda yeniden deneme mantığı ekleyin. |
| [InvalidOperationException](/dotnet/api/system.invalidoperationexception?view=netcore-3.1) |İstenen Kullanıcı işlemine sunucu veya hizmet içinde izin verilmiyor. Ayrıntılar için özel durum iletisine bakın. Örneğin, [tamamlanmış ()](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) Ileti [receiveanddelete](/dotnet/api/microsoft.azure.servicebus.receivemode) modunda alınmışsa bu özel durumu oluşturur. |Kodu ve belgeleri denetleyin. İstenen işlemin geçerli olduğundan emin olun. |Yeniden deneme yardım etmez. |
| [OperationCanceledException](/dotnet/api/system.operationcanceledexception?view=netcore-3.1) |Zaten kapatılmış, durdurulmuş veya atılmış bir nesne üzerinde bir işlemi çağırmak için bir girişimde bulunuldu. Nadir durumlarda, çevresel işlem zaten atıldı. |Kodu denetleyin ve çıkarılan bir nesne üzerinde işlemleri çağırmadığına emin olun. |Yeniden deneme yardım etmez. |
| [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception?view=netcore-3.1) |[TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) nesnesi belirteç alamadı, belirteç geçersiz veya belirteç işlemi yapmak için gereken talepleri içermiyor. |Belirteç sağlayıcısının doğru değerlerle oluşturulduğundan emin olun. Access Control Service yapılandırmasını denetleyin. |Yeniden deneme bazı durumlarda yardımcı olabilirler; koda yeniden deneme mantığı ekleyin. |
| [ArgumentException](/dotnet/api/system.argumentexception?view=netcore-3.1)<br /> [ArgumentNullException](/dotnet/api/system.argumentnullexception?view=netcore-3.1)<br />[ArgumentOutOfRangeException](/dotnet/api/system.argumentoutofrangeexception?view=netcore-3.1) |Yönteme sağlanan bir veya daha fazla bağımsız değişken geçersiz.<br /> [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) için sağlanan URI yol kesimi (ler) içeriyor.<br /> [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) için sağlanan URI şeması geçersiz. <br />Özellik değeri 32 KB 'den büyük. |Çağıran kodu denetleyin ve bağımsız değişkenlerin doğru olduğundan emin olun. |Yeniden deneme yardım etmez. |
| [MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.servicebus.messagingentitynotfoundexception) |İşlemle ilişkili varlık yok veya silinmiş. |Varlığın mevcut olduğundan emin olun. |Yeniden deneme yardım etmez. |
| [MessageNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagenotfoundexception) |Belirli bir sıra numarasına sahip bir ileti alma girişimi. Bu ileti bulunamadı. |İletinin zaten alınmadığından emin olun. İletinin kaldırılmış olup olmadığını görmek için sahipsiz sırayı kontrol edin. |Yeniden deneme yardım etmez. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) |İstemci Service Bus bir bağlantı kuramıyor. |Sağlanan ana bilgisayar adının doğru olduğundan ve konağın erişilebilir olduğundan emin olun. |Yeniden dene, aralıklı bağlantı sorunları olup olmadığı konusunda yardımcı olabilir. |
| [ServerBusyException](/dotnet/api/microsoft.azure.servicebus.serverbusyexception) |Hizmet şu anda isteği işleyemiyor. |İstemci bir süre bekleyip işlemi yeniden deneyin. |İstemci belirli bir aralıktan sonra yeniden deneyebilir. Yeniden deneme farklı bir özel durumla sonuçlanırsa, bu özel durumun yeniden deneme davranışını denetleyin. |
| [MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception) |Aşağıdaki durumlarda oluşabilecek genel mesajlaşma özel durumu:<p>Farklı bir varlık türüne (örneğin, bir konu) ait olan bir ad veya yol kullanarak bir [Queueclient](/dotnet/api/microsoft.azure.servicebus.queueclient) oluşturmak için girişimde bulunuldu.</p><p>256 KB 'den büyük bir ileti göndermek için bir girişimde bulunuldu. </p>Sunucu veya hizmet, isteğin işlenmesi sırasında bir hatayla karşılaştı. Ayrıntılar için özel durum iletisine bakın. Genellikle geçici bir özel durumdur.</p><p>Varlık kısıtlandığından istek sonlandırıldı. Hata kodu: 50001, 50002, 50008. </p> | Kodu denetleyin ve ileti gövdesi için yalnızca serileştirilebilir nesnelerin kullanıldığından emin olun (veya özel bir seri hale getirici kullanın). <p>Özelliklerin desteklenen değer türleri için belgeleri denetleyin ve yalnızca desteklenen türleri kullanın.</p><p> [Isgeçici](/dotnet/api/microsoft.servicebus.messaging.messagingexception) özelliğini denetleyin. **Doğru**ise işlemi yeniden deneyebilirsiniz. </p>| Özel durum azaltmasından kaynaklanıyorsa, birkaç saniye bekleyip işlemi yeniden deneyin. Yeniden deneme davranışı tanımsızdır ve diğer senaryolarda yardımcı olmayabilir.|
| [Messagingentityalreadyvartsexception](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) |Bu hizmet ad alanındaki başka bir varlık tarafından zaten kullanılan bir ada sahip bir varlık oluşturma girişimi. |Mevcut varlığı silin veya oluşturulacak varlık için farklı bir ad seçin. |Yeniden deneme yardım etmez. |
| [QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) |Mesajlaşma varlığı izin verilen en büyük boyuta ulaştı veya bir ad alanına yönelik bağlantı sayısı üst sınırı aşıldı. |Varlıktan veya onun alt sıraları üzerinden ileti alarak varlıkta alan oluşturun. Bkz. [QuotaExceededException](#quotaexceededexception). |Yeniden deneme, iletilerin bu sırada kaldırılıp kaldırılmadığı konusunda yardımcı olabilir. |
| [RuleActionException](/dotnet/api/microsoft.servicebus.messaging.ruleactionexception) |Service Bus geçersiz bir kural eylemi oluşturmayı denerseniz bu özel durumu döndürür. Service Bus, söz konusu ileti için kural eylemi işlenirken bir hata oluşursa, bu özel durumu bir hatalı iletiye iliştirir. |Kural eyleminin doğruluğunu denetleyin. |Yeniden deneme yardım etmez. |
| [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) |Service Bus geçersiz bir filtre oluşturmayı denerseniz bu özel durumu döndürür. Service Bus bu özel durumu, bu ileti için filtre işlenirken bir hata oluştuysa bir iletiye iliştirir. |Filtreyi doğruluk açısından denetleyin. |Yeniden deneme yardım etmez. |
| [SessionCannotBeLockedException](/dotnet/api/microsoft.servicebus.messaging.sessioncannotbelockedexception) |Belirli bir oturum KIMLIĞIYLE oturumu kabul etme girişimi, ancak oturum şu anda başka bir istemci tarafından kilitlenmiş. |Oturumun kilidinin başka istemciler tarafından açık olduğundan emin olun. |Oturumun geçici olarak yayınlanmışsa, yeniden deneme yardımcı olabilir. |
| [TransactionSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.transactionsizeexceededexception) |İşlemin parçası çok fazla sayıda işlem. |Bu işlemin parçası olan işlem sayısını azaltın. |Yeniden deneme yardım etmez. |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.azure.servicebus.messagingentitydisabledexception) |Devre dışı bırakılmış bir varlık üzerinde çalışma zamanı işlemi isteği. |Varlığı etkinleştirin. |Yeniden deneme, varlığın geçici bir şekilde etkinleştirilmiş olup olmadığı konusunda yardımcı olabilir. |
| [NoMatchingSubscriptionException](/dotnet/api/microsoft.servicebus.messaging.nomatchingsubscriptionexception) |Service Bus, ön filtreleme etkin olan ve filtrelerin hiçbirinde eşleşmeyen bir konuya ileti gönderirseniz bu özel durumu döndürür. |En az bir filtrenin eşleştiğinden emin olun. |Yeniden deneme yardım etmez. |
| [Iletiizeexceededexception](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |İleti yükü 256 KB sınırını aşıyor. 256-KB sınırı, sistem özelliklerini ve tüm .NET ek yükünü içerebilen toplam ileti boyutudur. |İleti yükünün boyutunu küçültün, sonra işlemi yeniden deneyin. |Yeniden deneme yardım etmez. |
| [TransactionException](/dotnet/api/system.transactions.transactionexception?view=netcore-3.1) |Çevresel işlem (*Transaction. Current*) geçersiz. Tamamlanmış veya durdurulmuş olabilir. İç özel durum, ek bilgi sağlayabilir. | |Yeniden deneme yardım etmez. |
| [TransactionInDoubtException](/dotnet/api/system.transactions.transactionindoubtexception?view=netcore-3.1) |Şüpheli bir işlem üzerinde bir işlem yapılmaya çalışıldı veya işlemi yürütmek için bir girişimde bulunuldu ve işlem şüpheli olur. |İşlem zaten kaydedilmiş olabileceğinden, uygulamanız bu özel durumu (özel bir durum olarak) işlemelidir. |- |

## <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception), belirli bir varlık için belirlenen kotanın aşıldığını gösterir.

### <a name="queues-and-topics"></a>Kuyruklar ve konular
Kuyruklar ve konular için genellikle kuyruğun boyutudur. Hata iletisi özelliği, aşağıdaki örnekte olduğu gibi daha ayrıntılı bilgiler içerir:

```Output
Microsoft.ServiceBus.Messaging.QuotaExceededException
Message: The maximum entity size has been reached or exceeded for Topic: 'xxx-xxx-xxx'. 
    Size of entity in bytes:1073742326, Max entity size in bytes:
1073741824..TrackingId:xxxxxxxxxxxxxxxxxxxxxxxxxx, TimeStamp:3/15/2013 7:50:18 AM
```

İleti, konunun boyut sınırını aştığını, bu durumda 1 GB (varsayılan boyut sınırı) olduğunu belirtir. 

### <a name="namespaces"></a>Ad Alanları

Ad alanları için [QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) , bir uygulamanın bir ad alanına yönelik en fazla bağlantı sayısını aştığını gösterebilir. Örnek:

```Output
Microsoft.ServiceBus.Messaging.QuotaExceededException: ConnectionsQuotaExceeded for namespace xxx.
<tracking-id-guid>_G12 ---> 
System.ServiceModel.FaultException`1[System.ServiceModel.ExceptionDetail]: 
ConnectionsQuotaExceeded for namespace xxx.
```

### <a name="common-causes"></a>Yaygın nedenler
Bu hatanın yaygın iki nedeni vardır: atılacak ileti sırası ve çalışır olmayan ileti alıcıları.

1. **[Atılacak ileti sırası](service-bus-dead-letter-queues.md)** Bir okuyucu iletileri tamamlamayacak ve kilit sona erdiğinde iletiler kuyruğa/konuya döndürülür. Okuyucu, [Brokeredmessage. tamamlanmıştır](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.complete)öğesini aramasını önleyen bir özel durumla karşılaştığında gerçekleşebilir. Bir ileti 10 kez okunduktan sonra varsayılan olarak atılacak ileti kuyruğuna gider. Bu davranış, [Queuedescription. MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount) özelliği tarafından denetlenir ve varsayılan değeri 10 ' dur. İletiler, atılacak ileti kuyruğunda yer aldığı sırada yer kaplar.
   
    Bu sorunu çözmek için, başka bir kuyruktan yaptığınız gibi atılacak ileti sırasından iletileri okuyun ve doldurun. Atılacak ileti sırası yolunu biçimlendirmeye yardımcı olması için [FormatDeadLetterPath](/dotnet/api/microsoft.azure.servicebus.entitynamehelper.formatdeadletterpath) yöntemini kullanabilirsiniz.
2. **Alıcı durdu**. Alıcı bir kuyruktan veya abonelikten ileti almayı durdurdu. Bunu belirlemenin yolu, iletilerin tam dökümünü gösteren [Queuedescription. MessageCountDetails](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails) özelliğine bakabilmenizdir. [Activemessagecount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.activemessagecount) özelliği yüksek veya daha fazla olursa iletiler yazıldığı kadar hızlı okunmazlar.

## <a name="timeoutexception"></a>TimeoutException
[TimeoutException](/dotnet/api/system.timeoutexception?view=netcore-3.1) , Kullanıcı tarafından başlatılan bir işlemin işlem zaman aşımından daha uzun sürdüğünü gösterir. 

[ServicePointManager. DefaultConnectionLimit](/dotnet/api/system.net.servicepointmanager.defaultconnectionlimit?view=netcore-3.1) özelliğinin değerini denetlemeniz gerekir, çünkü bu sınıra ulaşarak bir [TimeoutException](/dotnet/api/system.timeoutexception?view=netcore-3.1)de neden olabilir.

### <a name="queues-and-topics"></a>Kuyruklar ve konular
Kuyruklar ve konular için, zaman aşımı, bağlantı dizesinin parçası olarak ya da [Servicebusconnectionstringbuilder](/dotnet/api/microsoft.azure.servicebus.servicebusconnectionstringbuilder)aracılığıyla [Messagingfactorysettings. OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings) özelliğinde belirtilir. Hata iletisinin kendisi farklılık gösterebilir, ancak her zaman geçerli işlem için belirtilen zaman aşımı değerini içerir. 

## <a name="messagelocklostexception"></a>MessageLockLostException

### <a name="cause"></a>Nedeni

**Messagelocklostexception** , [PeekLock](message-transfers-locks-settlement.md#peeklock) alma modu kullanılarak bir ileti alındığında ve istemci tarafından tutulan kilit hizmet tarafında sona erdiğinde oluşur.

Bir iletideki kilit çeşitli nedenlerden dolayı sona ermeyebilir- 

  * Kilit zamanlayıcının, istemci uygulaması tarafından yenilenmeden önce süresi doldu.
  * İstemci uygulaması kilidi aldı, kalıcı bir depoya kaydetti ve sonra yeniden başlatıldı. Yeniden başlatıldıktan sonra istemci uygulaması, esnek iletilere bakarak bunları tamamlamayı denedi.

### <a name="resolution"></a>Çözüm

Bir **Messagelocklostexception**olayında, istemci uygulaması artık iletiyi işleyemez. İstemci uygulaması isteğe bağlı olarak analiz için özel durumu günlüğe kaydetmeyi göz önünde bulundurmayabilir, ancak istemcinin iletiyi *atmalıdır* .

İleti üzerindeki kilidin süresi sona erdiğinden, kuyruğa (veya aboneliğe) geri döner ve alma işlemi çağıran bir sonraki istemci uygulaması tarafından işlenebilir.

**Maxdeliverycount** aşılırsa Ileti, **DeadLetterQueue**öğesine taşınabilir.

## <a name="sessionlocklostexception"></a>SessionLockLostException

### <a name="cause"></a>Nedeni

**Sessionlocklostexception** , bir oturum kabul edildiğinde ve istemci tarafından tutulan kilit hizmet tarafında sona erdiğinde oluşur.

Bir oturumdaki kilit, çeşitli nedenlerle sona ermeyebilir- 

  * Kilit zamanlayıcının, istemci uygulaması tarafından yenilenmeden önce süresi doldu.
  * İstemci uygulaması kilidi aldı, kalıcı bir depoya kaydetti ve sonra yeniden başlatıldı. Yeniden başlatıldıktan sonra istemci uygulaması, esnek oturumlara bakıyordu ve bu oturumlardaki iletileri işlemeye çalışmış olur.

### <a name="resolution"></a>Çözüm

**Sessionlocklostexception**durumunda istemci uygulaması artık oturumdaki iletileri işlemez. İstemci uygulaması, analiz için özel durumu günlüğe kaydetmeyi göz önünde bulundurmayabilir, ancak istemcinin iletiyi *atmalıdır* .

Oturumdaki kilit sona erdiğinden, kuyruğa (veya aboneliğe) geri döner ve oturumu kabul eden bir sonraki istemci uygulaması tarafından kilitlenebilir. Oturum kilidi, belirli bir zamanda tek bir istemci uygulaması tarafından tutulduğundan, sıralı işleme garanti edilir.

## <a name="socketexception"></a>SocketException

### <a name="cause"></a>Nedeni

Aşağıdaki durumlarda bir **SocketException** oluşturulur-
   * Bir bağlantı girişimi başarısız olduğunda, ana bilgisayar belirtilen süreden sonra düzgün şekilde yanıt vermediğinden (TCP hata kodu 10060).
   * Bağlı konak yanıt vermediği için bağlantı kurulamadı.
   * İleti işlenirken bir hata oluştu veya zaman aşımı uzak ana bilgisayar tarafından aşıldı.
   * Temel alınan ağ kaynak sorunu.

### <a name="resolution"></a>Çözüm

**SocketException** hataları, UYGULAMALARı barındıran VM 'nin adı `<mynamespace>.servicebus.windows.net` ilgili IP adresine dönüştüremeyeceğini gösterir. 

Aşağıdaki komutun IP adresine eşlemede başarılı olup olmadığını denetleyin.

```Powershell
PS C:\> nslookup <mynamespace>.servicebus.windows.net
```

aşağıda gösterildiği gibi bir çıktı sağlaması gerekir

```bash
Name:    <cloudappinstance>.cloudapp.net
Address:  XX.XX.XXX.240
Aliases:  <mynamespace>.servicebus.windows.net
```

Yukarıdaki ad bir IP ve ad alanı diğer adı olarak **çözümlenmezse** , ağ yöneticisinin daha fazla araştırılacağını kontrol edin. Ad çözümlemesi, genellikle müşteri ağındaki bir DNS sunucusu aracılığıyla yapılır. DNS çözümlemesi Azure DNS tarafından yapılabiliyorsanız lütfen Azure desteğine başvurun.

Ad çözümlemesi **beklendiği gibi çalışıyorsa**, Azure Service Bus bağlantılara bu adreste izin [verilip verilmeyeceğini denetleyin](service-bus-troubleshooting-guide.md#connectivity-certificate-or-timeout-issues)


## <a name="messagingexception"></a>MessagingException

### <a name="cause"></a>Nedeni

**Messagingexception** çeşitli nedenlerle oluşturulabilecek genel bir istisnadır. Bazı nedenlerden bazıları aşağıda listelenmiştir.

   * Bir **Konu** veya **abonelik**üzerinde bir **queueclient** oluşturmak için bir girişimde bulunuldu.
   * Gönderilen iletinin boyutu verilen katmanın sınırından daha büyük. Service Bus [kotaları ve limitleri](service-bus-quotas.md)hakkında daha fazla bilgi edinin.
   * Belirli veri düzlemi isteği (gönderme, alma, tamamlanma, bırakma) azaltma nedeniyle sonlandırıldı.
   * Hizmet yükseltmeleri ve yeniden başlatmaları nedeniyle oluşan geçici sorunlar.

> [!NOTE]
> Yukarıdaki özel durumlar listesi ayrıntılı değildir.

### <a name="resolution"></a>Çözüm

Çözümleme adımları, **Messagingexception** 'ın oluşturulmasına neden olan ne olduğuna bağlıdır.

   * **Geçici sorunlar** Için ( ***ısgeçici*** 'in ***true***olarak ayarlandığı) veya **azaltma sorunları**için işlemi yeniden denemek sorunu çözebilir. SDK 'daki varsayılan yeniden deneme ilkesi bu için yararlanılabilir olabilir.
   * Diğer sorunlar için, özel durum içindeki Ayrıntılar sorunu gösterir ve çözümleme adımları da aynı şekilde anlaşılamıyor.

## <a name="next-steps"></a>Sonraki adımlar
.NET API başvurusu Service Bus için, bkz. [Azure .NET API başvurusu](/dotnet/api/overview/azure/service-bus).
Sorun giderme ipuçları için bkz. [sorun giderme kılavuzu](service-bus-troubleshooting-guide.md)