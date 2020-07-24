---
title: Yinelenen ileti algılamayı Azure Service Bus | Microsoft Docs
description: Bu makalede Azure Service Bus iletilerinde yinelenenleri nasıl Algılayabileceğiniz açıklanır. Yinelenen ileti yoksayılabilir ve bırakılabilir.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: dbca1b4b4f894d35835e7d37e0b4e742a2d3b917
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87083897"
---
# <a name="duplicate-detection"></a>Yineleme algılama

Bir uygulama, bir ileti gönderdikten hemen sonra önemli bir hata nedeniyle başarısız olursa ve yeniden başlatılan uygulama örneği, önceki ileti tesliminin gerçekleşmediği görüldüğü takdirde, sonraki bir gönderme aynı iletinin iki kez sistemde görünmesine neden olur.

Ayrıca, istemci veya ağ düzeyindeki bir hata daha önce gerçekleşmesi ve gönderilmiş bir iletinin sıraya teslim edilmesiyle, bildirim istemciye başarıyla döndürülmediği için de mümkündür. Bu senaryo, istemciyi gönderme işleminin sonucu hakkında şüpheli halde bırakır.

Yinelenen algılama, gönderenin aynı iletiyi yeniden göndermesini sağlayarak bu durumlardan yararlanır ve kuyruk veya konu, yinelenen kopyaları atar.

Yinelenen saptamayı etkinleştirmek, belirli bir zaman penceresi sırasında bir sıraya veya konuya gönderilen tüm iletilerin uygulama denetimli *MessageID* ' i izlemeye devam eder. Zaman penceresi sırasında günlüğe kaydedilen *MessageID* ile yeni bir ileti gönderilirse, ileti kabul edildi olarak bildirilir (gönderme işlemi başarılı olur), ancak yeni gönderilen ileti anında yoksayılır ve bırakılır. İletinin *MessageID* dışında başka hiçbir bölümü göz önünde bulundurulmamasıdır.

Yalnızca *uygulamanın, bir* hata oluştuğunda öngörülebilir olarak yeniden oluşturulduğu bir iş süreci bağlamına erişmesini sağladığından, tanımlayıcının uygulama denetimi gereklidir.

Bazı uygulama bağlamını işleme sürecinde birden fazla iletinin gönderildiği bir iş süreci için, *MessageID* , bir satın alma siparişi numarası gibi uygulama düzeyi bağlam tanımlayıcısının bir bileşimi olabilir ve örneğin, **12345.2017/ödeme**gibi ileti konusu olabilir.

*MessageID* her zaman bir GUID olabilir, ancak tanımlayıcıyı iş işlemine tutturma, yinelenen algılama özelliğini etkin bir şekilde kullanmak için istenen tahmin edilebilir yinelenebilirlik verir.

> [!NOTE]
> Yinelenen algılama etkinse ve oturum KIMLIĞI ya da bölüm anahtarı ayarlanmamışsa, bölüm anahtarı olarak ileti KIMLIĞI kullanılır. İleti KIMLIĞI de ayarlanmamışsa, .NET ve AMQP kitaplıkları ileti için otomatik olarak bir ileti KIMLIĞI oluşturur. Daha fazla bilgi için bkz. [bölüm anahtarlarının kullanımı](service-bus-partitioning.md#use-of-partition-keys).

## <a name="enable-duplicate-detection"></a>Yinelenen algılamayı etkinleştir

Portalda, özellik varsayılan olarak kapalı olan **yinelenen saptamayı etkinleştir** onay kutusu ile varlık oluşturma sırasında açıktır. Yeni konu başlıkları oluşturma ayarı eşdeğerdir.

![Yinelenen saptamayı etkinleştir seçeneği belirlenmiş ve kırmızı renkle anahatlı bir sıra oluştur iletişim kutusunun ekran görüntüsü.][1]

> [!IMPORTANT]
> Sıra oluşturulduktan sonra yinelenen saptamayı etkinleştiremez/devre dışı bırakabilirsiniz. Bunu yalnızca kuyruğu oluşturma sırasında yapabilirsiniz. 

Programlama yoluyla, bayrağı Full Framework .NET API 'sindeki [Queuedescription. requiresDuplicateDetection](/dotnet/api/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection#Microsoft_ServiceBus_Messaging_QueueDescription_RequiresDuplicateDetection) özelliği ile ayarlarsınız. Azure Resource Manager API 'SI ile, değeri [Queueproperties. requiresDuplicateDetection](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) özelliği ile ayarlanır.

Yinelenen algılama zaman geçmişi, kuyruklar ve konular için en fazla yedi gün olacak şekilde 30 saniye olur. Bu ayarı, Azure portal sıra ve konu özellikleri penceresinde değiştirebilirsiniz.

![Özellikler ayarı vurgulanmış Service Bus özelliğinin ekran görüntüsü ve kırmızı renkle yinelenen algılama geçmişi seçeneği.][2]

Programlama yoluyla, tam .NET Framework API 'SI ile [Queuedescription. DuplicateDetectionHistoryTimeWindow](/dotnet/api/microsoft.servicebus.messaging.queuedescription.duplicatedetectionhistorytimewindow#Microsoft_ServiceBus_Messaging_QueueDescription_DuplicateDetectionHistoryTimeWindow) özelliğini kullanarak ileti kimliklerinin saklanacağı yinelenen saptama penceresinin boyutunu yapılandırabilirsiniz. Azure Resource Manager API 'SI ile, değeri [Queueproperties. duplicateDetectionHistoryTimeWindow](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) özelliği ile ayarlanır.

Tüm kayıtlı ileti kimliklerinin yeni gönderilen ileti tanımlayıcısına karşı eşleşmesi gerektiğinden, yinelenen saptamayı etkinleştirmek ve pencerenin boyutu doğrudan sırayı (ve konu) verimini etkiler.

Pencerenin küçük tutulması, daha az ileti kimliği tutulması ve eşleştirilmesi gerektiği ve verimlilik daha az etkilenildiği anlamına gelir. Yinelenen saptamayı gerektiren yüksek aktarım hızı varlıkları için, pencereyi mümkün olduğunca küçük tutmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)

İstemci kodunun daha önceki bir *MessageID* ile bir iletiyi yeniden gönderemediği senaryolarda, güvenli bir şekilde işlenebilen iletiler tasarlamak önemlidir. [Eşkuvvetlilik hakkında bu blog gönderisi](https://particular.net/blog/what-does-idempotent-mean) , bunun nasıl yapılacağı hakkında çeşitli teknikler açıklamaktadır.

[1]: ./media/duplicate-detection/create-queue.png
[2]: ./media/duplicate-detection/queue-prop.png
