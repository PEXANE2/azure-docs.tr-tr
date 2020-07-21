---
title: İleti oturumlarını Azure Service Bus | Microsoft Docs
description: Bu makalede, sınırsız sayıda ilişkili ileti dizisinin Birleşik ve sıralı işlenmesini sağlamak üzere oturumların nasıl kullanılacağı açıklanmaktadır.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 05efc550e119186a2925c13d3fcfed11bec17251
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86511305"
---
# <a name="message-sessions"></a>İleti oturumları
Microsoft Azure Service Bus oturumlar, sınırsız sayıda ilgili ileti dizisinin birleşme ve sıralı işlenmesini sağlar. Oturumlar **ilk içinde, ilk çıkar (FIFO)** ve **istek-yanıt** desenlerinde kullanılabilir. Bu makalede, Service Bus kullanırken bu desenleri uygulamak için oturumların nasıl kullanılacağı gösterilmektedir. 

> [!NOTE]
> Service Bus temel katmanı oturumları desteklemez. Standart ve Premium katmanlar oturumları destekler. Bu katmanlar arasındaki farklar için bkz. [Service Bus fiyatlandırması](https://azure.microsoft.com/pricing/details/service-bus/).

## <a name="first-in-first-out-fifo-pattern"></a>İlk ın, ilk çıkar (FıFO) deseninin
Service Bus bir FıFO garantisi sağlamak için oturumları kullanın. Service Bus, iletiler arasındaki ilişkinin doğası hakkında değildir ve ayrıca bir ileti sırasının nerede başlatıldığını veya bittiğini belirlemek için belirli bir model tanımlamaz.

Herhangi bir gönderen, [SessionID](/dotnet/api/microsoft.azure.servicebus.message.sessionid#Microsoft_Azure_ServiceBus_Message_SessionId) özelliğini oturum için benzersiz olan uygulama tanımlı bazı tanımlayıcılarla ayarlayarak bir konuya veya kuyruğa ileti gönderirken bir oturum oluşturabilir. AMQP 1,0 protokol düzeyinde, bu değer *Grup Kimliği* özelliğine eşlenir.

Oturum kullanan sıralarda veya aboneliklerde oturum, oturumun [SessionID](/dotnet/api/microsoft.azure.servicebus.message.sessionid#Microsoft_Azure_ServiceBus_Message_SessionId)'sini içeren en az bir ileti olduğunda mevcut olacaktır. Bir oturum varsa, oturumun süresinin dolması veya kaybolması durumunda için tanımlı bir zaman veya API yoktur. Teorik olarak, bugün bir oturum için bir ileti alınabilir, bir yılda bir sonraki ileti ve **SessionID** eşleşiyorsa, oturum Service Bus perspektifinden aynı olur.

Bununla birlikte, genellikle bir uygulama, ilgili bir ileti kümesinin başladığı ve bittiği yerde açık bir kavram içerir. Service Bus belirli bir kural yapmaz.

Bir dosyanın aktarılması için bir diziyi belirleme örneği, ilk iletinin **Başlangıç**olarak, ara iletilerin **içeriğe**ve son Iletinin **sona erdirmek**için **Label** özelliğini ayarlamaya yönelik bir örnektir. İçerik iletilerinin göreli konumu, *SequenceNumber* **Başlangıç** iletisinden Delta *SequenceNumber*geçerli ileti olarak hesaplanabilir.

Service Bus oturum özelliği, C# ve Java API 'Lerinde [Messagesession](/dotnet/api/microsoft.servicebus.messaging.messagesession) biçiminde belirli bir alma işlemini mümkün bir şekilde sunar. Azure Resource Manager aracılığıyla ya da portalda bayrağını ayarlayarak ya da abonelik veya abonelikte [RequiresSession](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) özelliğini ayarlayarak özelliği etkinleştirebilirsiniz. İlgili API işlemlerini kullanmayı denemeden önce bu gereklidir.

Portalda bayrağı aşağıdaki onay kutusuyla ayarlayın:

![Oturumları etkinleştir seçeneği belirlenmiş ve kırmızı renkle özetlenen kuyruk oluştur iletişim kutusunun ekran görüntüsü.][2]

> [!NOTE]
> Bir kuyrukta veya abonelikte Oturumlar etkinleştirildiğinde, istemci uygulamalar ***artık*** normal iletileri gönderemez/alamaz. Tüm iletilerin bir oturumun parçası olarak gönderilmesi (oturum kimliği ayarlanarak) ve oturum alarak alınmalıdır.

Kuyruk ve abonelik istemcilerinde oturum için API 'Ler mevcuttur. Oturum ve mesajların ne zaman alındığını denetleyen ve alma döngüsünü yönetmenin karmaşıklığını gizleyen bir işleyici tabanlı model olan bir zorunlu model vardır. *OnMessage*

### <a name="session-features"></a>Oturum özellikleri

Oturumlar, sıralı teslimi korurken ve garanti edilirken, araya eklemeli ileti akışlarının eşzamanlı olarak birden çok kullanımını sağlar.

![Oturumlar özelliğinin sıralı teslimi nasıl koruyagösteren bir diyagram.][1]

İstemci tarafından oturum kabul eden bir [Messagesession](/dotnet/api/microsoft.servicebus.messaging.messagesession) alıcısı oluşturulur. İstemci, C# ' de [queueclient. AcceptMessageSession](/dotnet/api/microsoft.servicebus.messaging.queueclient.acceptmessagesession#Microsoft_ServiceBus_Messaging_QueueClient_AcceptMessageSession) veya [Queueclient. AcceptMessageSessionAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.acceptmessagesessionasync#Microsoft_ServiceBus_Messaging_QueueClient_AcceptMessageSessionAsync) çağırır. Reaktif geri çağırma modelinde bir oturum işleyicisi kaydeder.

[Messagesession](/dotnet/api/microsoft.servicebus.messaging.messagesession) nesnesi kabul edildiğinde ve bir istemci tarafından tutulurken, bu istemci kuyrukta veya abonelikte var olan [SessionID](/dotnet/api/microsoft.servicebus.messaging.messagesession.sessionid#Microsoft_ServiceBus_Messaging_MessageSession_SessionId) 'ye sahip tüm iletilerde ve ayrıca oturum tutulurken hala gelen **SessionID** 'ye sahip tüm iletilerde özel bir kilit barındırır.

**Close** veya **CloseAsync** çağrıldığında kilit serbest bırakılır veya kilit, uygulamanın kapatma işlemini yapaamadığı durumlarda zaman aşımına uğrar. Oturum kilidi, bir dosya üzerinde özel bir kilit gibi değerlendirilmelidir, yani uygulamanın artık ihtiyaç duyulmadığında oturumu kapatması ve/veya başka ileti beklememesidir.

Kuyruktan birden çok eşzamanlı alıcı geldiğinde, belirli bir oturuma ait iletiler, o oturum için kilidi Şu anda tutan belirli bir alıcıya dağıtılır. Bu işlemle, bir kuyruk veya abonelikte araya eklemeli bir ileti akışı, farklı alıcılar için düzgün şekilde devre dışı bırakılır ve kilit Yönetimi hizmet tarafı Service Bus içinde olduğundan, bu alıcılar farklı istemci makinelerde da kullanılabilir.

Önceki çizimde üç eşzamanlı oturum alıcısı gösterilmektedir. = 4 olan bir oturumda `SessionId` etkin, sahip olmayan bir istemci yoktur ve bu belirli oturumdan hiçbir ileti teslim edimediği anlamına gelir. Bir oturum, alt sıra gibi birçok şekilde davranır.

Oturum alıcısı tarafından tutulan oturum kilidi, *Peek-kilit* kapatma modu tarafından kullanılan ileti kilitleri için bir şemsiye. Bir oturumda yalnızca bir alıcıda kilit olabilir. Bir alıcı birçok kol içi ileti içerebilir, ancak iletiler sırayla alınacaktır. Bir iletiyi terk etmek, bir sonraki alma işlemiyle aynı iletinin yeniden sunulmasını sağlar.

### <a name="message-session-state"></a>İleti oturumu durumu

İş akışları yüksek ölçekli, yüksek kullanılabilirliğe sahip bulut sistemlerinde işlendiğinde, belirli bir oturumla ilişkili iş akışı işleyicisinin beklenmedik hatalardan kurtulabilmeleri ve çalışmanın başladığı farklı bir işlem veya makine üzerinde kısmen tamamlanmış çalışmayı sürdürdirebilmeleri gerekir.

Oturum durumu özelliği, bu oturuma göre kaydedilen işleme durumunun, oturum yeni bir işlemci tarafından edinildiği zaman anında kullanılabilir hale gelmesi için, aracı içindeki bir ileti oturumunun uygulama tanımlı ek açıklamasına olanak sağlar.

Service Bus perspektifinden ileti oturumu durumu, Service Bus standart için 256 KB ve Service Bus Premium için 1 MB olan bir ileti boyutunun verilerini tutan donuk bir ikili nesnedir. Bir oturuma göre işleme durumu oturum durumu içinde tutulabilir veya oturum durumu, bu tür bilgileri tutan bir depolama konumunu veya veritabanı kaydını işaret edebilir.

Oturum durumunu yönetmeye yönelik API 'Ler, [SetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_) ve [GetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate#Microsoft_ServiceBus_Messaging_MessageSession_GetState), hem C# hem de Java API 'lerinde [messagesession](/dotnet/api/microsoft.servicebus.messaging.messagesession) nesnesinde bulunabilir. Daha önce oturum durumu kümesi olmayan bir oturum, **GetState**için **null** bir başvuru döndürür. Daha önce ayarlanan oturum durumunu Temizleme işlemi [setstate (null)](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_)ile yapılır.

Oturum durumu, bir oturumdaki tüm iletiler tüketilse bile temizlenmeyen ( **null**döndüren) sürece kalır.

Bir kuyruktaki veya abonelikteki tüm mevcut oturumlar, Java API 'sinde **Sessionbrowser** yöntemiyle ve .NET Framework Istemcisinde [Queueclient](/dotnet/api/microsoft.servicebus.messaging.queueclient) ve [Subscriptionclient](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient) üzerinde [getmessagesessions](/dotnet/api/microsoft.servicebus.messaging.queueclient.getmessagesessions#Microsoft_ServiceBus_Messaging_QueueClient_GetMessageSessions) ile listelenebilir.

Bir kuyrukta veya abonelikte tutulan oturum durumu, söz konusu varlığın depolama kotasına doğru sayılır. Uygulama bir oturumla bittiğinde, bu nedenle uygulamanın, dış yönetim maliyetinden kaçınmak için korunan durumunu temizlemesi önerilir.

### <a name="impact-of-delivery-count"></a>Teslimat sayısının etkisi

Oturum bağlamındaki ileti başına teslim sayısı tanımı, oturum yokluğunda tanımdan biraz farklılık gösterir. Burada, teslim sayısı arttırılır.

| Senaryo | İletinin teslim sayısı arttırılır |
|----------|---------------------------------------------|
| Oturum kabul edildi, ancak oturum kilidinin süresi dolduğunda (zaman aşımı nedeniyle) | Yes |
| Oturum kabul edildi, oturumdaki iletiler (kilitli olsalar bile) tamamlanmaz ve oturum kapalı | No |
| Oturum kabul edildi, iletiler tamamlandı, sonra oturum açık olarak kapalı | Yok (Standart akışdır. Buradan iletiler oturumdan kaldırılır) |

## <a name="request-response-pattern"></a>İstek-yanıt deseninin
[İstek-yanıt deseninin](https://www.enterpriseintegrationpatterns.com/patterns/messaging/RequestReply.html) , gönderen uygulamanın bir istek göndermesini sağlayan ve alıcının gönderen uygulamasına doğru bir yanıt gönderebilmesi için bir yol sağlayan iyi şekilde belirlenmiş bir tümleştirme deseninin olması önerilir. Bu model genellikle uygulamanın yanıt gönderebilmesi için kısa süreli bir kuyruk veya konuya ihtiyaç duyuyor. Bu senaryoda, oturumlar benzer anlambilimi olan basit bir alternatif çözüm sağlar. 

Birden çok uygulama, kendi isteklerini tek bir istek kuyruğuna gönderebilir ve belirli bir üst bilgi parametresi, gönderen uygulamasını benzersiz şekilde tanımlamak üzere ayarlanır. Alıcı uygulaması kuyruktaki istekleri işleyebilir ve oturum etkinleştirme kuyruğuna yanıt gönderebilir ve oturum KIMLIĞINI gönderenin istek iletisine gönderdiği benzersiz tanımlayıcıya ayarlar. İsteği gönderen uygulama daha sonra belirli oturum KIMLIĞINDE iletiler alabilir ve yanıtları doğru şekilde işleyebilir.

> [!NOTE]
> İlk istekleri gönderen uygulama, oturum KIMLIĞI hakkında bilgi almalıdır ve `SessionClient.AcceptMessageSession(SessionID)` yanıtı beklediği oturumu kilitlemek için kullanır. Uygulamanın örneğini bir oturum kimliği olarak benzersiz bir şekilde tanımlayan GUID kullanmak iyi bir fikirdir. `AcceptMessageSession(timeout)`Yanıtların belirli alıcılar tarafından kilitlenmek ve işlenmek üzere kullanılabilir olduğundan emin olmak için herhangi bir oturum işleyicisi veya kuyrukta olmaması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- Oturum kullanan iletileri işlemek için .NET Framework istemcisini kullanan bir örnek için [Microsoft. Azure. ServiceBus örnekleri](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/Sessions) veya [Microsoft. ServiceBus. Messaging örneklerine](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/Sessions) bakın. 

Service Bus mesajlaşma hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/message-sessions/sessions.png
[2]: ./media/message-sessions/queue-sessions.png
