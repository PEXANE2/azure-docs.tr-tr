---
title: Azure Servis Veri Servisi ileti oturumları | Microsoft Dokümanlar
description: Bu makalede, ilgili iletilerin sınırsız dizilerinin ortak ve sıralı şekilde işlenmesini etkinleştirmek için oturumların nasıl kullanılacağı açıklanmaktadır.
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
ms.openlocfilehash: 1e22641e9d4f9959c26cd2043ea2acd7e260e0f0
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81314049"
---
# <a name="message-sessions"></a>İleti oturumları
Microsoft Azure Hizmet Veri Servisi oturumları, ilgili iletilerin sınırsız dizilerinin ortak ve sıralı şekilde işlenmesini sağlar. Oturumlar ilk olarak, ilk çıkışta (FIFO) ve istek yanıt kalıplarında kullanılabilir. Bu makalede, Hizmet Veri Servisi kullanırken bu desenleri uygulamak için oturumları nasıl kullanılacağını gösterir. 

## <a name="first-in-first-out-fifo-pattern"></a>İlk çıkan, ilk çıkan (FIFO) desen
Servis Veri Yollarında FIFO garantisi sağlamak için oturumları kullanın. Hizmet Veri Servisi iletiler arasındaki ilişkinin doğası hakkında açıklayıcı değildir ve ileti dizisinin nerede başlayıp nerede sona erdiğini belirlemek için belirli bir model tanımlamaz.

> [!NOTE]
> Servis Veri Servisi'nin temel katmanı oturumları desteklemez. Standart ve premium katmanlar oturumları destekler. Bu katmanlar arasındaki farklar için [Servis Veri Servisi fiyatlandırması'na](https://azure.microsoft.com/pricing/details/service-bus/)bakın.

Herhangi bir gönderen, [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid#Microsoft_Azure_ServiceBus_Message_SessionId) özelliğini oturuma özgü bazı uygulama tanımlı tanımlayıcısına ayarlayarak bir konuya veya kuyruğa ileti gönderirken oturum oluşturabilir. AMQP 1.0 protokol düzeyinde, bu değer *grup kimliği* özelliğiyle eşlenir.

Oturuma duyarlı kuyruklarda veya aboneliklerde, [oturumoturumKimliği](/dotnet/api/microsoft.azure.servicebus.message.sessionid#Microsoft_Azure_ServiceBus_Message_SessionId)ile en az bir ileti olduğunda oturumlar ortaya çıkar. Bir oturum olduğunda, oturumun süresinin dolması veya kaybolduğu zaman için tanımlanmış bir zaman veya API yoktur. Teorik olarak, bugün bir oturum için bir ileti alınabilir, bir sonraki ileti bir yıl içinde ve **SessionId** eşleşirse, oturum Servis Veri Servisi açısından aynıdır.

Genellikle, ancak, bir uygulama ilgili iletiler kümesi başlar ve biter nerede açık bir kavram vardır. Servis Otobüsü belirli bir kural belirlemez.

Bir dosyayı aktarmak için bir sıranın nasıl belirlenececeaçıklaştırılabilen bir örnek, etiket **özelliğini** ilk iletinin **başlatılması**, ara iletilerin **içeriğe**ve son iletinin **sona ermesi**için ayarlamaktır. İçerik iletilerinin göreli **konumu, başlangıç** iletisi *SequenceNumber'tan* geçerli ileti *SequenceNumber*deltası olarak hesaplanabilir.

Service Bus'taki oturum özelliği, C# ve Java API'lerinde [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) şeklinde belirli bir alma işlemini sağlar. Azure Kaynak Yöneticisi aracılığıyla kuyrukta veya abonelikte [gerekli Oturum](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) özelliğini ayarlayarak veya portaldaki bayrağı ayarlayarak özelliği etkinleştirebilirsiniz. İlgili API işlemlerini kullanmaya çalışmadan önce gereklidir.

Portalda, bayrağı aşağıdaki onay kutusuyla ayarlayın:

![][2]

> [!NOTE]
> Oturumlar bir sırada veya abonelikte etkinleştirildiğinde, istemci uygulamaları ***artık*** normal iletigönderemez/alamaz. Tüm iletiler bir oturumun parçası olarak (oturum kimliğini ayarlayarak) gönderilmeli ve oturumu alarak alınmalıdır.

Oturumlar için API'ler sıra ve abonelik istemcilerinde bulunur. Oturumların ve iletilerin ne zaman alındığını denetleyen zorunlu bir model ve Alma döngüsünün yönetiminin karmaşıklığını gizleyen *OnMessage'a*benzer işleyici tabanlı bir model vardır.

### <a name="session-features"></a>Oturum özellikleri

Oturumlar, sıralı teslimatı korurken ve garanti ederken, ara sıraileti akışlarının eşzamanlı olarak de-çoklamasını sağlar.

![][1]

[İletiOturumu](/dotnet/api/microsoft.servicebus.messaging.messagesession) alıcısı, istemcinin bir oturumu kabul eden tarafından oluşturulur. İstemci [QueueClient.AcceptMessageSession](/dotnet/api/microsoft.servicebus.messaging.queueclient.acceptmessagesession#Microsoft_ServiceBus_Messaging_QueueClient_AcceptMessageSession) veya [QueueClient.AcceptMessageSessionAsync'i](/dotnet/api/microsoft.servicebus.messaging.queueclient.acceptmessagesessionasync#Microsoft_ServiceBus_Messaging_QueueClient_AcceptMessageSessionAsync) C#'da çağırır. Reaktif geri arama modelinde, bir oturum işleyicisi kaydeder.

[MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) nesnesi kabul edildiğinde ve istemci tarafından tutulduğunda, bu istemci, söz konusu oturumun [OturumKimliği'nde](/dotnet/api/microsoft.servicebus.messaging.messagesession.sessionid#Microsoft_ServiceBus_Messaging_MessageSession_SessionId) bulunan tüm iletilerde ve ayrıca oturum tutulurken hala gelen **SessionId'li** tüm iletilerde özel bir kilit tutar.

Kilit, **Close** veya **CloseAsync** çağrıldığında veya uygulamanın yakın işlemi yapamadığı durumlarda kilit süresi dolduğunda serbest bırakılır. Oturum kilidi bir dosyaüzerinde özel bir kilit gibi ele alınmalıdır, bu da uygulamanın artık ihtiyacı olmadığı anda oturumu kapatması gerektiği ve/veya başka ileti beklememesi gerektiği anlamına gelir.

Birden çok eşzamanlı alıcı kuyruktan çekildiğinde, belirli bir oturuma ait iletiler, o oturumun kilidini tutan belirli alıcıya gönderilir. Bu işlemle, bir kuyruktaki veya abonelikteki aralar arası ileti akışı, farklı alıcılara temiz bir şekilde çok yönlü olarak giderilir ve kilit yönetimi servis tarafında, Hizmet Veri Servisi'nde gerçekleştiğinden, bu alıcılar da farklı istemci makinelerinde yaşayabilir.

Önceki resimde üç eşzamanlı oturum alıcıları gösterilmektedir. = 4'lü `SessionId` bir Oturum'un etkin ve sahip olunan istemcisi yoktur, bu da bu oturumdan ileti teslim edilmemiştir. Bir oturum alt sıra gibi birçok şekilde davranır.

Oturum alıcısı tarafından tutulan oturum *kilidi, gözetleme kilidi* kapatma modu tarafından kullanılan ileti kilitleri için bir şemsiyedir. Bir alıcıaynı anda "uçuş" iki ileti olamaz, ancak iletiler sırayla işlenmesi gerekir. Yeni bir ileti yalnızca önceki ileti tamamlandığında veya ölü harflerle yazılmışolduğunda elde edilebilir. İletiyi terk etmek, aynı iletinin bir sonraki alma işlemiyle birlikte yeniden sunulmasına neden olur.

### <a name="message-session-state"></a>İleti oturumu durumu

İş akışları yüksek ölçekli, yüksek kullanılabilirlikli bulut sistemlerinde işlendiğinde, belirli bir oturumla ilişkili iş akışı işleyicisinin beklenmeyen hatalardan kurtarabilmesi ve çalışmanın başladığı farklı bir işlem veya makinede kısmen tamamlanmış çalışmaya devam edebilmesi gerekir.

Oturum durumu tesisi, aracı içinde bir ileti oturumunun uygulama tanımlı bir ek açıklamasını sağlar, böylece oturum yeni bir işlemci tarafından satın alınınca bu oturuma göre kaydedilen işlem durumu anında kullanılabilir hale gelir.

Hizmet Veri Servisi açısından bakıldığında, ileti oturumu durumu, Hizmet Veri Hizmeti Veri Servisi Standardı için 256 KB ve Service Bus Premium için 1 MB olan bir iletinin boyutundaki verileri tutabilen opak bir ikili nesnedir. Bir oturuma göre işlem durumu oturum durumu içinde tutulabilir veya oturum durumu, bu tür bilgileri tutan bazı depolama konumuna veya veritabanı kaydına işaret edebilir.

Oturum durumunu, SetState'i ve [GetState'i](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_) yönetmek için API'ler, Hem C# hem de Java API'lerinde [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) nesnesinde bulunabilir. [GetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate#Microsoft_ServiceBus_Messaging_MessageSession_GetState) Daha önce oturum durumu kümesi olmayan bir oturum **GetState**için **null** başvuru döndürür. Önceden ayarlanmış oturum durumunu temizleme [SetState(null)](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_)ile yapılır.

Oturum durumu, oturumdaki tüm iletiler tüketilse bile temizlenmedikçe **(null**returning) sürece kalır.

Bir kuyruk veya abonelikteki tüm varolan oturumlar Java API'sındaki **SessionBrowser** yöntemi ve .NET Framework istemcisinde QueueClient ve [SubscriptionClient'daki](/dotnet/api/microsoft.servicebus.messaging.queueclient) [SubscriptionClient](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient) [GetMessageSessions](/dotnet/api/microsoft.servicebus.messaging.queueclient.getmessagesessions#Microsoft_ServiceBus_Messaging_QueueClient_GetMessageSessions) ile numaralandırılabilir.

Bir sırada veya abonelikte tutulan oturum durumu, o varlığın depolama kotasına doğru sayılır. Uygulama bir oturumla tamamlandığında, dış yönetim maliyetini önlemek için uygulamanın korunan durumunu temizlemesi önerilir.

### <a name="impact-of-delivery-count"></a>Teslimat sayısının etkisi

Oturumlar bağlamında ileti başına teslim sayısının tanımı, oturumların yokluğunda tanımdan biraz değişir. Teslimat sayısının ne zaman arta geldiğini özetleyen bir tablo aşağıda veda edilmiştir.

| Senaryo | İletinin teslim sayısı artımlı mı |
|----------|---------------------------------------------|
| Oturum kabul edilir, ancak oturum kilidi sona erer (zaman aşımı nedeniyle) | Evet |
| Oturum kabul edilir, oturum içindeki iletiler tamamlanmaz (kilitli olsalar bile) ve oturum kapatılır | Hayır |
| Oturum kabul edilir, iletiler tamamlanır ve oturum açıkça kapatılır | N/A (Standart akış. Burada iletiler oturumdan kaldırılır) |

## <a name="request-response-pattern"></a>İstek-yanıt deseni
[İstek yanıtde deseni,](https://www.enterpriseintegrationpatterns.com/patterns/messaging/RequestReply.html) gönderen uygulamanın bir istek göndermesini sağlayan ve alıcının gönderen uygulamaya doğru bir yanıt göndermesi için bir yol sağlayan iyi kurulmuş bir tümleştirme desenidir. Bu desen genellikle kısa ömürlü bir kuyruk veya uygulama yanıtları göndermek için konu gerekir. Bu senaryoda, oturumlar karşılaştırılabilir semantik ile basit bir alternatif çözüm sağlar. 

Birden çok uygulama, gönderen uygulamayı benzersiz olarak tanımlamak için belirli bir üstbilgi parametresi ayarlanmış olan isteklerini tek bir istek kuyruğuna gönderebilir. Alıcı uygulaması, sıraya giren istekleri işleyebilir ve oturumlar etkin leştirilmiş bir sıraya yanıt göndererek oturum kimliğini gönderenin istek iletisinde gönderdiği benzersiz tanımlayıcıya ayarlayabilir. İsteğe bağlı uygulama daha sonra belirli bir oturum kimliğindeki iletileri alabilir ve yanıtları doğru şekilde işleyebilir.

> [!NOTE]
> İlk istekleri gönderen uygulama oturum kimliğini bilmeli ve `SessionClient.AcceptMessageSession(SessionID)` yanıtı beklediği oturumu kilitlemek için kullanılmalıdır. Uygulama örneğini oturum kimliği olarak benzersiz olarak tanımlayan bir GUID kullanmak iyi bir fikirdir. Yanıtların belirli alıcılar tarafından `AcceptMessageSession(timeout)` kilitlenmeve işlenme için kullanılabilir olduğundan emin olmak için oturum işleyicisi veya kuyrukta olmamalıdır.

## <a name="next-steps"></a>Sonraki adımlar

- Oturuma duyarlı iletileri işlemek için .NET Framework istemcisini kullanan bir örnek için [Microsoft.Azure.ServiceBus örnekleriveya](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/Sessions) [Microsoft.ServiceBus.Messaging örneklerine](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/Sessions) bakın. 

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/message-sessions/sessions.png
[2]: ./media/message-sessions/queue-sessions.png
