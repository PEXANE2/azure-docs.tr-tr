---
title: İleti oturumlarını Azure Service Bus | Microsoft Docs
description: Bu makalede, sınırsız sayıda ilişkili ileti dizisinin Birleşik ve sıralı işlenmesini sağlamak üzere oturumların nasıl kullanılacağı açıklanmaktadır.
ms.topic: article
ms.date: 04/12/2021
ms.openlocfilehash: c9a1c4fdccbbc8b38805e23d4895448959126f10
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2021
ms.locfileid: "107308492"
---
# <a name="message-sessions"></a>İleti oturumları
Microsoft Azure Service Bus oturumlar, sınırsız sayıda ilgili ileti dizisinin birleşme ve sıralı işlenmesini sağlar. Oturumlar **ilk içinde, ilk çıkar (FIFO)** ve **istek-yanıt** desenlerinde kullanılabilir. Bu makalede, Service Bus kullanırken bu desenleri uygulamak için oturumların nasıl kullanılacağı gösterilmektedir. 

> [!NOTE]
> Service Bus temel katmanı oturumları desteklemez. Standart ve Premium katmanlar oturumları destekler. Bu katmanlar arasındaki farklar için bkz. [Service Bus fiyatlandırması](https://azure.microsoft.com/pricing/details/service-bus/).

## <a name="first-in-first-out-fifo-pattern"></a>İlk ın, ilk çıkar (FıFO) deseninin
Service Bus bir FıFO garantisi sağlamak için oturumları kullanın. Service Bus, iletiler arasındaki ilişkinin doğası hakkında değildir ve ayrıca bir ileti sırasının nerede başlatıldığını veya bittiğini belirlemek için belirli bir model tanımlamaz.

Her gönderici, oturum **kimliği** özelliğini oturum için benzersiz olan uygulama tanımlı bazı tanımlayıcılarla ayarlayarak bir konuya veya kuyruğa ileti gönderirken bir oturum oluşturabilir. AMQP 1,0 protokol düzeyinde, bu değer *Grup Kimliği* özelliğine eşlenir.

Oturum kullanan sıralarda veya aboneliklerde oturum KIMLIĞI olan en az bir ileti olduğunda oturumlar mevcut duruma gelir. Bir oturum varsa, oturumun süresinin dolması veya kaybolması durumunda için tanımlı bir zaman veya API yoktur. Teorik olarak, bugün bir oturum için bir ileti alınabilir, bir yılda bir sonraki ileti ve oturum KIMLIĞI eşleşiyorsa, oturum Service Bus perspektifinden aynı olur.

Bununla birlikte, genellikle bir uygulama, ilgili bir ileti kümesinin başladığı ve bittiği yerde açık bir kavram içerir. Service Bus belirli bir kural yapmaz. Örneğin, uygulamanız ilk ileti için **etiket** özelliğini **Başlangıç** olarak, ara iletiler için **içerik** ve son iletinin **sona erdirmek** için ayarlayabilir. İçerik iletilerinin göreli konumu, *SequenceNumber* **Başlangıç** iletisinden Delta *SequenceNumber* geçerli ileti olarak hesaplanabilir.

Azure Resource Manager aracılığıyla ya da portalda bayrağını ayarlayarak ya da abonelik veya abonelikte [RequiresSession](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) özelliğini ayarlayarak özelliği etkinleştirebilirsiniz. İlgili API işlemlerini kullanmayı denemeden önce bu gereklidir.

Portalda, aşağıdaki örneklerde gösterildiği gibi bir varlık (kuyruk veya abonelik) oluştururken oturumları etkinleştirebilirsiniz. 

:::image type="content" source="./media/message-sessions/queue-sessions.png" alt-text="Kuyruğu sıra oluşturma sırasında etkinleştirin":::

:::image type="content" source="./media/message-sessions/subscription-sessions.png" alt-text="Abonelik oluşturma sırasında oturumu etkinleştir":::


> [!IMPORTANT]
> Bir kuyrukta veya abonelikte Oturumlar etkinleştirildiğinde, istemci uygulamalar ***artık*** normal iletileri gönderemez/alamaz. Tüm iletilerin bir oturumun parçası olarak gönderilmesi (oturum kimliği ayarlanarak) ve oturum kabul edildiğinde alınması gerekir.

Kuyruk ve abonelik istemcilerinde oturum için API 'Ler mevcuttur. Oturumların ve iletilerin ne zaman alındığını denetleyen ve alma döngüsünü yönetmenin karmaşıklığını gizleyen işleyici tabanlı bir model olan bir zorunlu model vardır. 

Örnekler için, [sonraki adımlar](#next-steps) bölümündeki bağlantıları kullanın. 

### <a name="session-features"></a>Oturum özellikleri

Oturumlar, sıralı teslimi korurken ve garanti edilirken, araya eklemeli ileti akışlarının eşzamanlı olarak birden çok kullanımını sağlar.

![Oturumlar özelliğinin sıralı teslimi nasıl koruyagösteren bir diyagram.][1]

Oturum alıcısı, oturum kabul eden bir istemci tarafından oluşturulur. Oturum bir istemci tarafından kabul edildiğinde ve tutulduğunda, istemci kuyruktaki veya abonelikteki oturum **kimliği** olan tüm iletilerde dışlamalı bir kilit tutar. Ayrıca, daha sonra gelen **oturum kimliği** ile tüm iletilerde dışlamalı kilitleri da tutar.

Kilit, alıcı üzerinde yakın ilgili yöntemleri çağırdığınızda veya kilidin süresi dolarsa serbest bırakılır. Alıcıda de kilitleri yenilemek için yöntemler vardır. Bunun yerine, otomatik kilit yenileme özelliğini kullanarak kilidi yenilemek istediğiniz zaman süresini belirleyebilirsiniz. Oturum kilidi, bir dosya üzerinde özel bir kilit gibi değerlendirilmelidir, yani uygulamanın artık ihtiyaç duyulmadığında oturumu kapatması ve/veya başka ileti beklememesidir.

Kuyruktan birden çok eşzamanlı alıcı geldiğinde, belirli bir oturuma ait iletiler, o oturum için kilidi Şu anda tutan belirli bir alıcıya dağıtılır. Bu işlemle, bir kuyruk veya abonelikte araya eklemeli bir ileti akışı, farklı alıcılar için düzgün şekilde devre dışı bırakılır ve kilit Yönetimi hizmet tarafı Service Bus içinde olduğundan, bu alıcılar farklı istemci makinelerde da kullanılabilir.

Önceki çizimde üç eşzamanlı oturum alıcısı gösterilmektedir. = 4 olan bir oturumda `SessionId` etkin, sahip olmayan bir istemci yoktur ve bu belirli oturumdan hiçbir ileti teslim edimediği anlamına gelir. Bir oturum, alt sıra gibi birçok şekilde davranır.

Oturum alıcısı tarafından tutulan oturum kilidi, *Peek-kilit* kapatma modu tarafından kullanılan ileti kilitleri için bir şemsiye. Bir oturumda yalnızca bir alıcıda kilit olabilir. Bir alıcı birçok kol içi ileti içerebilir, ancak iletiler sırayla alınacaktır. Bir iletiyi terk etmek, bir sonraki alma işlemiyle aynı iletinin yeniden sunulmasını sağlar.

### <a name="message-session-state"></a>İleti oturumu durumu

İş akışları yüksek ölçekli, yüksek kullanılabilirliğe sahip bulut sistemlerinde işlendiğinde, belirli bir oturumla ilişkili iş akışı işleyicisinin beklenmedik hatalardan kurtulabilmeleri ve çalışmanın başladığı farklı bir işlem veya makine üzerinde kısmen tamamlanmış çalışmayı sürdürdirebilmeleri gerekir.

Oturum durumu özelliği, bu oturuma göre kaydedilen işleme durumunun, oturum yeni bir işlemci tarafından edinildiği zaman anında kullanılabilir hale gelmesi için, aracı içindeki bir ileti oturumunun uygulama tanımlı ek açıklamasına olanak sağlar.

Service Bus perspektifinden ileti oturumu durumu, Service Bus standart için 256 KB ve Service Bus Premium için 1 MB olan bir ileti boyutunun verilerini tutan donuk bir ikili nesnedir. Bir oturuma göre işleme durumu oturum durumu içinde tutulabilir veya oturum durumu, bu tür bilgileri tutan bir depolama konumunu veya veritabanı kaydını işaret edebilir.

Oturum durumunu yönetme yöntemleri, SetState ve GetState, oturum alıcısı nesnesinde bulunabilir. Daha önce hiç oturum durumu olmayan bir oturum, GetState için null bir başvuru döndürür. Daha önce ayarlanan oturum durumu, alıcının SetState yöntemine null geçirerek temizlenemez.

Oturum durumu, bir oturumdaki tüm iletiler tüketilse bile temizlenmeyen ( **null** döndüren) sürece kalır.

Bir kuyrukta veya abonelikte tutulan oturum durumu, söz konusu varlığın depolama kotasına doğru sayılır. Uygulama bir oturumla bittiğinde, bu nedenle uygulamanın, dış yönetim maliyetinden kaçınmak için korunan durumunu temizlemesi önerilir.

### <a name="impact-of-delivery-count"></a>Teslimat sayısının etkisi

Oturum bağlamındaki ileti başına teslim sayısı tanımı, oturum yokluğunda tanımdan biraz farklılık gösterir. Burada, teslim sayısı arttırılır.

| Senaryo | İletinin teslim sayısı arttırılır |
|----------|---------------------------------------------|
| Oturum kabul edildi, ancak oturum kilidinin süresi dolduğunda (zaman aşımı nedeniyle) | Yes |
| Oturum kabul edildi, oturumdaki iletiler (kilitli olsalar bile) tamamlanmaz ve oturum kapalı | Hayır |
| Oturum kabul edildi, iletiler tamamlandı, sonra oturum açık olarak kapalı | Yok (Standart akışdır. Buradan iletiler oturumdan kaldırılır) |

## <a name="request-response-pattern"></a>İstek-yanıt deseninin
[İstek-yanıt deseninin](https://www.enterpriseintegrationpatterns.com/patterns/messaging/RequestReply.html) , gönderen uygulamanın bir istek göndermesini sağlayan ve alıcının gönderen uygulamasına doğru bir yanıt gönderebilmesi için bir yol sağlayan iyi şekilde belirlenmiş bir tümleştirme deseninin olması önerilir. Bu model genellikle uygulamanın yanıt gönderebilmesi için kısa süreli bir kuyruk veya konuya ihtiyaç duyuyor. Bu senaryoda, oturumlar benzer anlambilimi olan basit bir alternatif çözüm sağlar. 

Birden çok uygulama, kendi isteklerini tek bir istek kuyruğuna gönderebilir ve belirli bir üst bilgi parametresi, gönderen uygulamasını benzersiz şekilde tanımlamak üzere ayarlanır. Alıcı uygulaması kuyruktaki istekleri işleyebilir ve oturum etkinleştirme kuyruğuna yanıt gönderebilir ve oturum KIMLIĞINI gönderenin istek iletisine gönderdiği benzersiz tanımlayıcıya ayarlar. İsteği gönderen uygulama daha sonra belirli oturum KIMLIĞINDE iletiler alabilir ve yanıtları doğru şekilde işleyebilir.

> [!NOTE]
> İlk istekleri gönderen uygulamanın oturum KIMLIĞINI bilmesi ve yanıtı beklediği oturumun kilitli olması için oturumu kabul etmesi için kullanması gerekir. Uygulamanın örneğini bir oturum kimliği olarak benzersiz bir şekilde tanımlayan GUID kullanmak iyi bir fikirdir. Yanıtların belirli alıcılar tarafından kilitlenmek ve işlenmek üzere kullanılabilir olmasını sağlamak için, bir oturum işleyicisi veya kuyruğun oturum alıcısında bir zaman aşımı belirtilmelidir.

## <a name="next-steps"></a>Sonraki adımlar

- [.NET için Azure. Messaging. ServiceBus örnekleri](/samples/azure/azure-sdk-for-net/azuremessagingservicebus-samples/)
- [Java için Azure Service Bus istemci kitaplığı-örnekler](/samples/azure/azure-sdk-for-java/servicebus-samples/)
- [Python için Azure Service Bus istemci kitaplığı-örnekler](/samples/azure/azure-sdk-for-python/servicebus-samples/)
- [JavaScript için Azure Service Bus istemci kitaplığı-örnekler](/samples/azure/azure-sdk-for-js/service-bus-javascript/)
- [TypeScript için Azure Service Bus istemci kitaplığı-örnekler](/samples/azure/azure-sdk-for-js/service-bus-typescript/)
- [.Net Için Microsoft. Azure. ServiceBus örnekleri](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/) (oturumlar ve sessionState örnekleri)  

Service Bus mesajlaşma hakkında daha fazla bilgi edinmek için bkz. [Service Bus kuyruklar, konular ve abonelikler](service-bus-queues-topics-subscriptions.md).

[1]: ./media/message-sessions/sessions.png
