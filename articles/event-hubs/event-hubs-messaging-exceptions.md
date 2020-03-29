---
title: Sorun giderme kılavuzu - Azure Etkinlik Hub'ları | Microsoft Dokümanlar
description: Bu makalede, Azure Olay Hub'ları ileti özel durumları ve önerilen eylemlerin bir listesini sağlar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76309787"
---
# <a name="troubleshooting-guide-for-azure-event-hubs"></a>Azure Etkinlik Hub'ları için sorun giderme kılavuzu
Bu makalede, Olay Hub'ları .NET Framework API'leri tarafından oluşturulan .NET özel durumları ve sorun giderme sorunları için diğer ipuçları sağlar. 

## <a name="event-hubs-messaging-exceptions---net"></a>Olay Hub'ları mesajlaşma özel durumları - .NET
Bu bölümde .NET Framework API'ler tarafından oluşturulan .NET özel durumları listelenir. 

### <a name="exception-categories"></a>Özel durum kategorileri

Olay Hub'ları .NET API'leri, bunları düzeltmek için yapabileceğiniz ilişkili eylemle birlikte aşağıdaki kategorilere düşebilecek özel durumlar oluşturur.

1. Kullanıcı kodlama hatası: [System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [System.Runtime.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx). Genel eylem: devam etmeden önce kodu düzeltmeye çalışın.
2. Kurulum/yapılandırma hatası: [Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception), [Microsoft.Azure.EventHubs.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception), [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Genel eylem: yapılandırmanızı gözden geçirin ve gerekirse değiştirin.
3. Geçici istisnalar: [Microsoft.ServiceBus.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](#serverbusyexception), [Microsoft.Azure.EventHubs.ServerBusyException](#serverbusyexception), [Microsoft.ServiceBus.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception). Genel eylem: işlemi yeniden deneyin veya kullanıcıları bilgilendirin.
4. Diğer istisnalar: [System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](#timeoutexception), [Microsoft.ServiceBus.Messaging.MessageLockLostException](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception), [Microsoft.ServiceBus.Messaging.SessionLockLostException](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception). Genel eylem: özel durum türüne özgü; aşağıdaki bölümdeki tabloya bakın. 

### <a name="exception-types"></a>Özel durum türleri
Aşağıdaki tabloda ileti özel durum türleri ve bunların nedenleri listelenecek ve notlar yapabileceğiniz eylemi önerilmektedir.

| Özel Durum Türü | Açıklama/Neden/Örnekler | Önerilen Eylem | Otomatik/anında yeniden deneme notu |
| -------------- | -------------------------- | ---------------- | --------------------------------- |
| [Timeoutexception](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |Sunucu, [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings)tarafından denetlenen belirtilen süre içinde istenen işlemi yanıt vermedi. Sunucu istenen işlemi tamamlamış olabilir. Bu özel durum, ağ veya diğer altyapı gecikmeleri nedeniyle gerçekleşebilir. |Tutarlılık ve gerekirse yeniden deneyin için sistem durumunu denetleyin.<br /> [Bkz. TimeoutException](#timeoutexception). | Yeniden deneme bazı durumlarda yardımcı olabilir; koda yeniden deneme mantığı ekleyin. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |İstenen kullanıcı işlemine sunucu veya hizmet içinde izin verilmez. Ayrıntılar için özel durum iletisi bakın. Örneğin, [Ileti](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) modunda alındıysa Complete bu özel durumu oluşturur. | Kodu ve belgeleri kontrol edin. İstenen işlemin geçerli olduğundan emin olun. | Yeniden denemenin bir faydası olmaz. |
| [Operationcanceledexception](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) | Zaten kapatılmış, iptal edilmiş veya elden çıkarılmış bir nesne üzerinde bir işlem çağırma girişiminde bulunulması. Nadir durumlarda, ortam işlemi zaten bertaraf edilir. | Kodu denetleyin ve elden çıkarılan bir nesne üzerinde işlem başlatmadığından emin olun. | Yeniden denemenin bir faydası olmaz. |
| [Unauthorizedaccessexception](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) | [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) nesnesi bir belirteç elde edemedi, belirteç geçersiz veya belirteç işlemi yapmak için gerekli iddiaları içermez. | Belirteç sağlayıcısının doğru değerlerle oluşturulduğundan emin olun. Access Denetim Hizmeti'nin yapılandırmasını denetleyin. | Yeniden deneme bazı durumlarda yardımcı olabilir; koda yeniden deneme mantığı ekleyin. |
| [Argumentexception](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [Argumentnullexception](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[Argumentoutofrangeexception](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) | Yönteme verilen bir veya daha fazla bağımsız değişken geçersizdir. [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [Create'e](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) sağlanan URI yol segmenti(ler) içerir. [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) veya [Create'e](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) verilen URI şeması geçersizdir. Özellik değeri 32 KB'den büyüktür. | Arama kodunu denetleyin ve bağımsız değişkenlerin doğru olduğundan emin olun. | Yeniden denemenin bir faydası olmaz. |
| [Microsoft.ServiceBus.Messaging MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception) <br /><br/> [Microsoft.Azure.EventHubs MesajlaşmaEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception) | İşlemle ilişkili varlık yok veya silindi. | Varlığın var olduğundan emin olun. | Yeniden denemenin bir faydası olmaz. |
| [MesajlaşmaİletişimÖzel Durum](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) | İstemci Olay Hub'ına bağlantı kuramaz. |Sağlanan ana bilgisayar adının doğru olduğundan ve ana bilgisayara ulaşıladığından emin olun. | Aralıklı bağlantı sorunları varsa yeniden deneme yardımcı olabilir. |
| [Microsoft.ServiceBus.Messaging ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) <br /> <br/>[Microsoft.Azure.EventHubs ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) | Hizmet şu anda isteği işleyemez. | İstemci bir süre bekleyebilir ve işlemi yeniden deneyebilir. <br /> Bkz. [ServerBusyException](#serverbusyexception). | İstemci belirli bir süre sonra yeniden deneyebilir. Yeniden deneme farklı bir özel durumla sonuçlanırsa, bu özel durum la ilgili yeniden deneme davranışını denetleyin. |
| [MesajlaşmaÖzel Durum](/dotnet/api/microsoft.servicebus.messaging.messagingexception) | Aşağıdaki durumlarda atılabilecek genel ileti özel durumu: Farklı bir varlık türüne (örneğin, bir konu) ait bir ad veya yol kullanarak [bir QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) oluşturma girişimi yapılır. 1 MB'dan büyük bir ileti gönderme girişiminde bulunularak yapılır. Sunucu veya hizmet, isteğin işlenmesi sırasında bir hatayla karşılaştı. Ayrıntılar için özel durum iletisi bakın. Bu özel durum genellikle geçici bir özel durumdur. | Kodu denetleyin ve ileti gövdesi için yalnızca serileştirilebilir nesnelerin kullanıldığından (veya özel bir serializer kullandığından) emin olun. Özelliklerin desteklenen değer türleri için belgeleri denetleyin ve yalnızca desteklenen türleri kullanın. [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception) özelliğini kontrol edin. **Doğruysa,** işlemi yeniden deneyebilirsiniz. | Yeniden deneme davranışı tanımsızdır ve yardımcı olmayabilir. |
| [MesajlaşmaEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) | Bu hizmet ad alanında başka bir varlık tarafından zaten kullanılan bir ada sahip bir varlık oluşturmaya çalış. | Varolan varlığı silin veya oluşturulacak varlık için farklı bir ad seçin. | Yeniden denemenin bir faydası olmaz. |
| [Quotaexceededexception](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) | İleti varlığı izin verilen maksimum boyutuna ulaştı. Bu özel durum, en fazla alıcı sayısı (5 olan) tüketici başına grup düzeyinde zaten açılmışsa gerçekleşebilir. | Varlıktan veya alt sıralarından iletiler alarak varlıkta alan oluşturun. <br /> Bkz. [QuotaExceededException](#quotaexceededexception) | Bu arada iletiler kaldırıldıysa yeniden deneyin yardımcı olabilir. |
| [MesajlaşmaEntityDisabledException](/dotnet/api/microsoft.servicebus.messaging.messagingentitydisabledexception) | Devre dışı bırakılmış bir varlık üzerinde çalışma zamanı çalışması isteği. |Varlığı etkinleştirin. | Varlık geçici olarak etkinleştirilmişse yeniden deneme yardımcı olabilir. |
| [Microsoft.ServiceBus.Messaging MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) <br /><br/> [Microsoft.Azure.EventHubs MessageSizeExceededException](/dotnet/api/microsoft.azure.eventhubs.messagesizeexceededexception) | İleti yükü 1 MB sınırını aşıyor. Bu 1 MB sınırı, sistem özelliklerini ve herhangi bir .NET genel havai tüm lerini içerebilen toplam ileti içindir. | İleti yükünün boyutunu küçültün ve işlemi yeniden deneyin. |Yeniden denemenin bir faydası olmaz. |

### <a name="quotaexceededexception"></a>Quotaexceededexception
[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception), belirli bir varlık için belirlenen kotanın aşıldığını gösterir.

Bu özel durum, tüketici başına grup düzeyinde en fazla alıcı sayısı (5) zaten açılmışsa gerçekleşebilir.

#### <a name="event-hubs"></a>Event Hubs
Olay Hub'ları, Etkinlik Hub'ı başına 20 tüketici grubu sınırına sahiptir. Daha fazla oluşturmaya çalıştığınızda, bir [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception)alırsınız. 

### <a name="timeoutexception"></a>Timeoutexception
[TimeoutException,](https://msdn.microsoft.com/library/system.timeoutexception.aspx) kullanıcı tarafından başlatılan bir işlemin işlem zaman aşımından daha uzun sürdüğünü gösterir. 

Olay Hub'ları için zaman aşımı bağlantı dizesinin bir parçası olarak veya [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.servicebus.servicebusconnectionstringbuilder)aracılığıyla belirtilir. Hata iletisinin kendisi değişebilir, ancak her zaman geçerli işlem için belirtilen zaman adedini içerir. 

#### <a name="common-causes"></a>Olası nedenler
Bu hatanın iki yaygın nedeni vardır: yanlış yapılandırma veya geçici hizmet hatası.

1. **Yanlış yapılandırma** Operasyon zaman, operasyonel durum için çok küçük olabilir. İstemci SDK'daki işlem zaman anına varsayılan değer 60 saniyedir. Kodunuzun değeri çok küçük bir şeye ayarlı p'lere sahip olup olmadığını kontrol edin. Ağın ve CPU kullanımının durumu, belirli bir işlemin tamamlanması için gereken süreyi etkileyebilir, bu nedenle işlem zaman laması küçük bir değere ayarlanmamalıdır.
2. **Geçici servis hatası** Bazen Olay Hub'ları hizmeti istekleri işlemede gecikmeler yaşayabilir; örneğin, yüksek trafik dönemlerinde. Bu gibi durumlarda, işlem başarılı olana kadar bir gecikmeden sonra işleminizi yeniden deneyebilirsiniz. Aynı işlem birden çok denemeden sonra hala başarısız olursa, bilinen hizmet kesintileri olup olmadığını görmek için [Azure hizmet durum sitesini](https://azure.microsoft.com/status/) ziyaret edin.

### <a name="serverbusyexception"></a>ServerBusyException

[Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) veya [Microsoft.Azure.EventHubs.ServerBusyException,](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) sunucunun aşırı yüklendiğini gösterir. Bu özel durum için iki ilgili hata kodu vardır.

#### <a name="error-code-50002"></a>Hata kodu 50002

Bu hata iki nedenden biri için oluşabilir:

1. Yük olay merkezindeki tüm bölümlere eşit olarak dağıtılmaz ve bir bölüm yerel üretim birimi sınırlamasına isabet eder.
    
    Çözüm: Bölüm dağıtım stratejisini gözden geçirmek veya [EventHubClient.Send'i (eventDataWithOutPartitionKey)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient) denemek yardımcı olabilir.

2. Olay Hub'ları ad alanı yeterli iş birimi ne sahip değildir (onaylamak için [Azure portalındaki](https://portal.azure.com) Olay Hub'ları ad alanı penceresindeki **Ölçümler** ekranını denetleyebilirsiniz). Portal toplu (1 dakika) bilgi gösterir, ancak biz gerçek zamanlı olarak iş sahibi ölçmek - bu yüzden sadece bir tahmin.

    Çözünürlük: Ad alanındaki iş birimi birimlerinin artırılması yardımcı olabilir. Bu işlemi portalda, Olay Hub'ları ad alanı ekranının **Ölçek** penceresinde yapabilirsiniz. Veya [Otomatik şişirme](event-hubs-auto-inflate.md)kullanabilirsiniz.

#### <a name="error-code-50001"></a>Hata kodu 50001

Bu hata nadiren oluşmalıdır. Ad alanınız için kod çalıştıran kapsayıcı CPU'da düşük olduğunda olur – Olay Hub'ları yük dengeleyicisinin başlamasından birkaç saniye den fazla olmamak.

#### <a name="limit-on-calls-to-the-getruntimeinformation-method"></a>GetRuntimeInformation yöntemine yapılan çağrıları sınırlama
Azure Etkinlik Hub'ları, GetRuntimeInfo'ya saniyede 50'ye kadar çağrı yı destekler. Sınıra ulaşıldıktan sonra aşağıdakilere benzer bir özel durum alabilirsiniz:

```
ExceptionId: 00000000000-00000-0000-a48a-9c908fbe84f6-ServerBusyException: The request was terminated because the namespace 75248:aaa-default-eventhub-ns-prodb2b is being throttled. Error code : 50001. Please wait 10 seconds and try again.
```

## <a name="connectivity-certificate-or-timeout-issues"></a>Bağlantı, sertifika veya zaman ara sorunları
Aşağıdaki adımlar, *.servicebus.windows.net altındaki tüm hizmetler için bağlantı/sertifika/zaman alametleri giderme sorununda size yardımcı olabilir. 

- Göz atın veya [wget](https://www.gnu.org/software/wget/) `https://<yournamespacename>.servicebus.windows.net/`. IP filtreleme niz mi, sanal ağ veya sertifika zinciri sorunlarınız olup olmadığını kontrol etmeye yardımcı olur (en yaygın ı java SDK kullanırken).

    Başarılı ileti örneği:
    
    ```xml
    <feed xmlns="http://www.w3.org/2005/Atom"><title type="text">Publicly Listed Services</title><subtitle type="text">This is the list of publicly-listed services currently available.</subtitle><id>uuid:27fcd1e2-3a99-44b1-8f1e-3e92b52f0171;id=30</id><updated>2019-12-27T13:11:47Z</updated><generator>Service Bus 1.1</generator></feed>
    ```
    
    Hata hatası iletisi örneği:

    ```json
    <Error>
        <Code>400</Code>
        <Detail>
            Bad Request. To know more visit https://aka.ms/sbResourceMgrExceptions. . TrackingId:b786d4d1-cbaf-47a8-a3d1-be689cda2a98_G22, SystemTracker:NoSystemTracker, Timestamp:2019-12-27T13:12:40
        </Detail>
    </Error>
    ```
- Güvenlik duvarında herhangi bir bağlantı noktasının engellendiğini kontrol etmek için aşağıdaki komutu çalıştırın. Kullanılan bağlantı noktaları 443 (HTTPS), 5671 (AMQP) ve 9093 (Kafka) 'dır. Kullandığınız kitaplık bağlı olarak, diğer bağlantı noktaları da kullanılır. Burada 5671 bağlantı noktası nın engellenip engellenmediğini kontrol eden örnek komutu verem.

    ```powershell
    tnc <yournamespacename>.servicebus.windows.net -port 5671
    ```

    Linux üzerinde:

    ```shell
    telnet <yournamespacename>.servicebus.windows.net 5671
    ```
- Aralıklı bağlantı sorunları olduğunda, bırakılan paketler olup olmadığını kontrol etmek için aşağıdaki komutu çalıştırın. Bu komut, hizmetle birlikte her 1 saniyede 25 farklı TCP bağlantısı kurmaya çalışır. Ardından, kaç tanesinin başarılı/başarısız olduğunu kontrol edebilir ve Ayrıca TCP bağlantı gecikmesini de görebilirsiniz. Aracı `psping` [buradan](/sysinternals/downloads/psping)indirebilirsiniz.

    ```shell
    .\psping.exe -n 25 -i 1 -q <yournamespacename>.servicebus.windows.net:5671 -nobanner     
    ```
    `tnc`,, `ping`ve benzeri diğer araçları kullanıyorsanız eşdeğer komutları kullanabilirsiniz. 
- Önceki adımlar yardımcı değilse bir ağ izleme edinin ve [Wireshark](https://www.wireshark.org/)gibi araçları kullanarak analiz. Gerekirse [Microsoft Destek'e](https://support.microsoft.com/) başvurun. 

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Olay Hub'larına genel bakış](event-hubs-what-is-event-hubs.md)
* [Etkinlik Merkezi Oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
