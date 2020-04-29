---
title: Sorun giderme kılavuzu-Azure Event Hubs | Microsoft Docs
description: Bu makale, Azure Event Hubs mesajlaşma özel durumlarının ve önerilen eylemlerin bir listesini sağlar.
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
ms.openlocfilehash: de5b95bd10bf72f60ba5d63c4b3a799556fcce33
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76309787"
---
# <a name="troubleshooting-guide-for-azure-event-hubs"></a>Azure Event Hubs için sorun giderme kılavuzu
Bu makalede, Event Hubs .NET Framework API 'Leri tarafından oluşturulan bazı .NET özel durumları ve sorun giderme sorunları için başka ipuçları sunulmaktadır. 

## <a name="event-hubs-messaging-exceptions---net"></a>Event Hubs mesajlaşma özel durumları-.NET
Bu bölümde, .NET Framework API 'Leri tarafından oluşturulan .NET özel durumları listelenmektedir. 

### <a name="exception-categories"></a>Özel durum kategorileri

Event Hubs .NET API 'Leri, çözümü gidermeye çalışmak için uygulayabileceğiniz ilgili eylemle birlikte, aşağıdaki kategorilere ayrılan özel durumlar oluşturur.

1. Kullanıcı kodlama hatası: [System. ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System. InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System. Operationolaydexception](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [System. Runtime. Serialization. SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx). Genel eylem: devam etmeden önce kodu gidermeyi deneyin.
2. Kurulum/yapılandırma hatası: [Microsoft. ServiceBus. Messaging. MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception), [Microsoft. Azure. EventHubs. MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception), [System. UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Genel eylem: yapılandırmanızı gözden geçirin ve gerekirse değiştirin.
3. Geçici özel durumlar: [Microsoft. ServiceBus. Messaging. MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft. ServiceBus. Messaging. ServerBusyException](#serverbusyexception), [Microsoft. Azure. Eventhubs. Serverbusyexception](#serverbusyexception), [Microsoft. ServiceBus. Messaging. MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception). Genel eylem: işlemi yeniden deneyin veya kullanıcılara bildirin.
4. Diğer özel durumlar: [System. Transactions. TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System. TimeoutException](#timeoutexception), [Microsoft. ServiceBus. Messaging. MessageLockLostException](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception), [Microsoft. ServiceBus. Messaging. sessionlocklostexception](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception). Genel eylem: özel durum türüne özgü; Aşağıdaki bölümdeki tabloya bakın. 

### <a name="exception-types"></a>Özel durum türleri
Aşağıdaki tabloda mesajlaşma özel durum türleri ve nedenleri ve gerçekleştirebileceğiniz notlar önerilir.

| Özel durum türü | Açıklama/neden/örnekler | Önerilen eylem | Otomatik/anında yeniden denemeye göz atma |
| -------------- | -------------------------- | ---------------- | --------------------------------- |
| [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |Sunucu, belirtilen süre içinde, [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings)tarafından denetlenen istenen işleme yanıt vermedi. Sunucu istenen işlemi tamamlamış olabilir. Bu özel durum, ağ veya diğer altyapı gecikmelerinden kaynaklanabilir. |Tutarlılık için sistem durumunu kontrol edin ve gerekirse yeniden deneyin.<br /> Bkz. [TimeoutException](#timeoutexception). | Yeniden deneme bazı durumlarda yardımcı olabilirler; koda yeniden deneme mantığı ekleyin. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |İstenen Kullanıcı işlemine sunucu veya hizmet içinde izin verilmiyor. Ayrıntılar için özel durum iletisine bakın. Örneğin, ileti [Receiveanddelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) modunda alınmışsa, [tamamlanmış](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) bu özel durumu oluşturur. | Kodu ve belgeleri denetleyin. İstenen işlemin geçerli olduğundan emin olun. | Yeniden deneme yardımcı olmayacaktır. |
| [OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) | Zaten kapatılmış, durdurulmuş veya atılmış bir nesne üzerinde bir işlemi çağırmak için bir girişimde bulunuldu. Nadir durumlarda, çevresel işlem zaten atıldı. | Kodu denetleyin ve çıkarılan bir nesne üzerinde işlemleri çağırmadığına emin olun. | Yeniden deneme yardımcı olmayacaktır. |
| [UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) | [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) nesnesi belirteç alamadı, belirteç geçersiz veya belirteç işlemi yapmak için gereken talepleri içermiyor. | Belirteç sağlayıcısının doğru değerlerle oluşturulduğundan emin olun. Access Control Service yapılandırmasını denetleyin. | Yeniden deneme bazı durumlarda yardımcı olabilirler; koda yeniden deneme mantığı ekleyin. |
| [ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [ArgumentNullException](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[ArgumentOutOfRangeException](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) | Yönteme sağlanan bir veya daha fazla bağımsız değişken geçersiz. [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) için sağlanan URI yol kesimi (ler) içeriyor. [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) için sağlanan URI şeması geçersiz. Özellik değeri 32 KB 'den büyük. | Çağıran kodu denetleyin ve bağımsız değişkenlerin doğru olduğundan emin olun. | Yeniden deneme, yardım etmez. |
| [Microsoft. ServiceBus. Messaging MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception) <br /><br/> [Microsoft. Azure. EventHubs MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception) | İşlemle ilişkili varlık yok veya silinmiş. | Varlığın mevcut olduğundan emin olun. | Yeniden deneme, yardım etmez. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) | İstemci, Olay Hub 'ına bir bağlantı kuramıyor. |Sağlanan ana bilgisayar adının doğru olduğundan ve konağın erişilebilir olduğundan emin olun. | Yeniden dene, aralıklı bağlantı sorunları olup olmadığı konusunda yardımcı olabilir. |
| [Microsoft. ServiceBus. Messaging Serverbusyıexception](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) <br /> <br/>[Microsoft. Azure. EventHubs Serverbusonexception](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) | Hizmet şu anda isteği işleyemiyor. | İstemci bir süre bekleyip işlemi yeniden deneyin. <br /> Bkz. [Serverbusyıexception](#serverbusyexception). | İstemci belirli bir aralıktan sonra yeniden deneyebilir. Yeniden deneme farklı bir özel durumla sonuçlanırsa, bu özel durumun yeniden deneme davranışını denetleyin. |
| [MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception) | Aşağıdaki durumlarda oluşturulabilecek genel mesajlaşma özel durumu: farklı bir varlık türüne (örneğin, bir konu) ait olan bir ad veya yol kullanarak bir [Queueclient](/dotnet/api/microsoft.servicebus.messaging.queueclient) oluşturmak için girişimde bulunuldu. 1 MB 'den büyük bir ileti göndermek için bir girişimde bulunuldu. Sunucu veya hizmet, isteğin işlenmesi sırasında bir hatayla karşılaştı. Ayrıntılar için özel durum iletisine bakın. Bu özel durum genellikle geçici bir özel durumdur. | Kodu denetleyin ve ileti gövdesi için yalnızca serileştirilebilir nesnelerin kullanıldığından emin olun (veya özel bir seri hale getirici kullanın). Özelliklerin desteklenen değer türleri için belgeleri denetleyin ve yalnızca desteklenen türleri kullanın. [Isgeçici](/dotnet/api/microsoft.servicebus.messaging.messagingexception) özelliğini denetleyin. **Doğru**ise işlemi yeniden deneyebilirsiniz. | Yeniden deneme davranışı tanımsız ve yardım olmayabilir. |
| [Messagingentityalreadyvartsexception](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) | Bu hizmet ad alanındaki başka bir varlık tarafından zaten kullanılan bir ada sahip bir varlık oluşturma girişimi. | Mevcut varlığı silin veya oluşturulacak varlık için farklı bir ad seçin. | Yeniden deneme, yardım etmez. |
| [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) | Mesajlaşma varlığı izin verilen en büyük boyuta ulaştı. Bu özel durum, müşteri başına grup düzeyinde en fazla alıcı sayısı (5 ' i) zaten açıldıysa meydana gelebilir. | Varlıktan veya onun alt sıraları üzerinden ileti alarak varlıkta alan oluşturun. <br /> Bkz. [QuotaExceededException](#quotaexceededexception) | Yeniden deneme, iletilerin bu sırada kaldırılıp kaldırılmadığı konusunda yardımcı olabilir. |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.servicebus.messaging.messagingentitydisabledexception) | Devre dışı bırakılmış bir varlık üzerinde çalışma zamanı işlemi isteği. |Varlığı etkinleştirin. | Yeniden deneme, varlığın geçici bir şekilde etkinleştirilmiş olup olmadığı konusunda yardımcı olabilir. |
| [Microsoft. ServiceBus. Messaging Iletiizeexceededexception](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) <br /><br/> [Microsoft. Azure. EventHubs Iletiizeexceededexception](/dotnet/api/microsoft.azure.eventhubs.messagesizeexceededexception) | İleti yükü 1 MB sınırı aşıyor. Bu 1 MB 'lık sınır, sistem özelliklerini ve tüm .NET ek yükünü içerebilen toplam ileti içindir. | İleti yükünün boyutunu küçültün, sonra işlemi yeniden deneyin. |Yeniden deneme, yardım etmez. |

### <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception), belirli bir varlık için belirlenen kotanın aşıldığını gösterir.

Bu özel durum, müşteri başına grup düzeyinde en fazla alıcı sayısı (5) zaten açılırsa meydana gelebilir.

#### <a name="event-hubs"></a>Event Hubs
Event Hubs Olay Hub 'ı başına 20 Tüketici grubu sınırı vardır. Daha fazla oluşturmaya çalıştığınızda bir [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception)alırsınız. 

### <a name="timeoutexception"></a>TimeoutException
[TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) , Kullanıcı tarafından başlatılan bir işlemin işlem zaman aşımından daha uzun sürdüğünü gösterir. 

Event Hubs için zaman aşımı, bağlantı dizesinin parçası olarak veya [Servicebusconnectionstringbuilder](/dotnet/api/microsoft.servicebus.servicebusconnectionstringbuilder)aracılığıyla belirtilir. Hata iletisinin kendisi farklılık gösterebilir, ancak her zaman geçerli işlem için belirtilen zaman aşımı değerini içerir. 

#### <a name="common-causes"></a>Olası nedenler
Bu hatanın iki yaygın nedeni vardır: yanlış yapılandırma veya geçici bir hizmet hatası.

1. **Yanlış yapılandırma** İşlem zaman aşımı, işlemsel koşul için çok küçük olabilir. İstemci SDK 'sında işlem zaman aşımı için varsayılan değer 60 saniyedir. Kodunuzun değerin çok küçük bir değere ayarlanmış olup olmadığını denetleyin. Ağ ve CPU kullanımının koşulu, belirli bir işlemin tamamlanma süresini etkileyebilir, bu nedenle işlem zaman aşımı küçük bir değere ayarlanamaz.
2. **Geçici hizmet hatası** Bazen Event Hubs hizmeti isteklerindeki gecikmelerle karşılaşabilir; Örneğin, yüksek trafik dönemlerinde. Bu gibi durumlarda, işlem başarılı olana kadar işleminizi bir gecikmeden sonra yeniden deneyebilirsiniz. Aynı işlem birden çok denemeden sonra yine de başarısız olursa, bilinen hizmet kesintileri olup olmadığını görmek için [Azure hizmet durumu sitesini](https://azure.microsoft.com/status/) ziyaret edin.

### <a name="serverbusyexception"></a>ServerBusyException

Bir [Microsoft. ServiceBus. Messaging. serverbusonexception](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) veya [Microsoft. Azure. EventHubs. serverbusonexception](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) , bir sunucunun aşırı yüklü olduğunu gösterir. Bu özel durum için iki ilgili hata kodu vardır.

#### <a name="error-code-50002"></a>Hata kodu 50002

Bu hata, iki nedenden biri nedeniyle oluşabilir:

1. Yük, Olay Hub 'ındaki tüm bölümler arasında eşit olarak dağıtılmaz ve bir bölüm yerel üretilen iş birimi sınırlamasını aşmaktadır.
    
    Çözüm: Bölüm dağıtım stratejisini düzeltme ya da [Eventhubclient. Send (eventDataWithOutPartitionKey)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient) yardımcı olabilecek.

2. Event Hubs ad alanı yeterli işleme birimi içermiyor (onaylamak için [Azure portal](https://portal.azure.com) Event Hubs ad alanı penceresinde **ölçümler** ekranını kontrol edebilirsiniz). Portal toplanmış (1 dakikalık) bilgileri gösterir, ancak aktarım hızını gerçek zamanlı olarak ölçyoruz; bu nedenle yalnızca bir tahmindir.

    Çözüm: ad alanındaki üretilen iş birimlerinin artırılması yardımcı olabilir. Bu işlemi portalda Event Hubs ad alanı ekranının **Ölçek** penceresinde yapabilirsiniz. Ya da [Otomatik Şişir](event-hubs-auto-inflate.md)kullanabilirsiniz.

#### <a name="error-code-50001"></a>Hata kodu 50001

Bu hata nadiren gerçekleşmelidir. Ad alanınız için kodu çalıştıran kapsayıcı, Event Hubs yük dengeleyici başlamadan önce birkaç saniyeden daha fazla değil, CPU üzerinde düşük olduğunda gerçekleşir.

#### <a name="limit-on-calls-to-the-getruntimeinformation-method"></a>GetRuntimeInformation yöntemiyle yapılan çağrıların sınırı
Azure Event Hubs, saniye başına Getruntimeınfo 'ya saniyede en fazla 50 çağrı destekler. Sınıra ulaşıldığında aşağıdakine benzer bir özel durum alabilirsiniz:

```
ExceptionId: 00000000000-00000-0000-a48a-9c908fbe84f6-ServerBusyException: The request was terminated because the namespace 75248:aaa-default-eventhub-ns-prodb2b is being throttled. Error code : 50001. Please wait 10 seconds and try again.
```

## <a name="connectivity-certificate-or-timeout-issues"></a>Bağlantı, sertifika veya zaman aşımı sorunları
Aşağıdaki adımlar *. servicebus.windows.net altındaki tüm hizmetlerde bağlantı/sertifika/zaman aşımı sorunlarını gidermenize yardımcı olabilir. 

- Veya [wget](https://www.gnu.org/software/wget/) `https://<yournamespacename>.servicebus.windows.net/`'e gidin. IP filtrelemesi veya sanal ağ ya da sertifika zinciri sorunları olup olmadığını denetlemeye yardımcı olur (Java SDK kullanırken en yaygın olarak).

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
- Güvenlik duvarında herhangi bir bağlantı noktasının engellenip engellenmediğini denetlemek için aşağıdaki komutu çalıştırın. Kullanılan bağlantı noktaları 443 (HTTPS), 5671 (AMQP) ve 9093 (Kafka). Kullandığınız kitaplığa bağlı olarak diğer bağlantı noktaları da kullanılır. 5671 bağlantı noktasının engellenip engellenmeyeceğini kontrol eden örnek komut aşağıda verilmiştir.

    ```powershell
    tnc <yournamespacename>.servicebus.windows.net -port 5671
    ```

    Linux 'ta:

    ```shell
    telnet <yournamespacename>.servicebus.windows.net 5671
    ```
- Aralıklı bağlantı sorunları olduğunda, bırakılan herhangi bir paket olup olmadığını denetlemek için aşağıdaki komutu çalıştırın. Bu komut, hizmeti ile her 1 saniyede 25 farklı TCP bağlantısı kurmaya çalışacaktır. Sonra, kaç tane başarılı/başarısız olduğunu denetleyebilir ve ayrıca TCP bağlantı gecikmesini de görebilirsiniz. `psping` Aracı [buradan](/sysinternals/downloads/psping)indirebilirsiniz.

    ```shell
    .\psping.exe -n 25 -i 1 -q <yournamespacename>.servicebus.windows.net:5671 -nobanner     
    ```
    `tnc`, `ping`Vb. gibi diğer araçları kullanıyorsanız eşdeğer komutları kullanabilirsiniz. 
- Önceki adımlar [Wireshark](https://www.wireshark.org/)gibi araçları kullanarak yardımcı değilse ve analiz yoksa bir ağ izlemesi elde edin. Gerekirse [Microsoft desteği](https://support.microsoft.com/) başvurun. 

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs genel bakış](event-hubs-what-is-event-hubs.md)
* [Olay Hub 'ı oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
