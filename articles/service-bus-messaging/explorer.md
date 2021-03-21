---
title: Service Bus üzerinde veri işlemleri gerçekleştirmek için Azure Service Bus Gezginini Kullanma (Önizleme)
description: Bu makalede, Azure Service Bus verilerine erişmek için portal tabanlı Azure Service Bus Gezginini kullanma hakkında bilgi sağlanır.
ms.topic: conceptual
ms.date: 01/20/2020
ms.openlocfilehash: 0b5274c492a1dfb2523c52d7aea2b7ebf8eae675
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101738965"
---
# <a name="use-service-bus-explorer-to-perform-data-operations-on-service-bus-preview"></a>Service Bus üzerinde veri işlemleri gerçekleştirmek için Service Bus Gezginini Kullanma (Önizleme)

## <a name="overview"></a>Genel Bakış

Azure Service Bus, gönderen ve alıcı istemci uygulamalarının kendi iş mantığını tanıdık noktadan noktaya (kuyruk) ve Publish-Subscribe (konu-abonelik) semantiğini kullanarak ayırmalarına olanak sağlar.

Azure Service Bus ad alanı üzerinde gerçekleştirilen işlemler iki tür 
   * Yönetim Işlemleri-Service Bus ad alanını, kuyrukları, konuları ve abonelikleri oluşturun, güncelleştirin, silin.
   * Veri Işlemleri-sıralara, konuya ve aboneliklerden Ileti gönderin ve Iletileri alın.

Azure Service Bus Gezgini, yönetim işlemlerinin ötesinde, abonelik, konu başlıkları ve aboneliklerde (ve atılacak bazı alt Azure portal varlıklarda) veri işlemlerini (gönder, al, Gözat) desteklemek için Portal işlevselliğini genişletir.

> [!NOTE]
> Bu makalede, Azure portal bulunan Azure Service Bus gezgin 'in işlevleri vurgulanmıştır.
>
> Azure Service Bus Explorer aracı, topluluğa ait olan OSS aracı [Service Bus gezgin](https://github.com/paolosalvatori/ServiceBusExplorer) ***değildir*** .
>

## <a name="prerequisites"></a>Önkoşullar

Service Bus Explorer aracını kullanmak için aşağıdaki görevleri gerçekleştirmeniz gerekir: 

- Azure Service Bus ad alanı sağlayın.
- İşlevselliği sınamak için bir abonelik içeren bir veya bir konuya ileti gönderecek veya bir konuyla ilgili bir kuyruk oluşturun. Kuyruklar, konular ve abonelikler oluşturmayı öğrenmek için aşağıdaki makalelere bakın: 
    - [Hızlı başlangıç-kuyruklar oluşturma](service-bus-quickstart-portal.md)
    - [Hızlı başlangıç-konu oluşturma](service-bus-quickstart-topics-subscriptions-portal.md)
- Ad alanındaki bu rollerden birinin üyesi olduğunuzdan emin olun: 
    - [Service Bus veri sahibi](../role-based-access-control/built-in-roles.md#azure-service-bus-data-owner) 
    - [Katkıda Bulunan](../role-based-access-control/built-in-roles.md#contributor) 
    - [Sahibi](../role-based-access-control/built-in-roles.md#owner)


## <a name="using-the-service-bus-explorer"></a>Service Bus Gezginini Kullanma

Azure Service Bus Gezginini kullanmak için, gönderme, göz atma ve alma işlemlerini gerçekleştirmek istediğiniz Service Bus ad alanına gitmeniz gerekir.

Bir sıraya karşı işlemler gerçekleştirmek istiyorsanız, gezinti menüsünden **' kuyruklar '** seçeneğini belirleyin. Bir konuya (ve ilgili aboneliklerine) yönelik işlemler gerçekleştirmek istiyorsanız **konuları** seçin. 

:::image type="content" source="./media/service-bus-explorer/queue-topics-left-navigation.png" alt-text="Varlık seçimi":::

**' Kuyruklar '** veya **' konular '** seçeneğini belirledikten sonra, belirli kuyruğu veya konuyu seçin.

Sol gezinti menüsünden **' Service Bus Gezgini 'ni (Önizleme) '** seçin

:::image type="content" source="./media/service-bus-explorer/left-navigation-menu-selected.png" alt-text="SB Gezgini sol gezinti menüsü":::

### <a name="sending-a-message-to-a-queue-or-topic"></a>Kuyruğa veya konuya ileti gönderme

Bir **kuyruğa** veya **konuya** bir Ileti göndermek için Service Bus Gezgini ' nde **_Gönder_** sekmesine tıklayın.

Burada bir ileti oluşturmak için 

1. **Içerik türünü** ' text/plain ', ' Application/XML ' veya ' Application/JSON ' olacak şekilde seçin.
2. İleti **içeriğini** ekleyin. Daha önce ayarlanan **Içerik türü** ile eşleştiğinden emin olun.
3. **Gelişmiş özellikleri** ayarlama (isteğe bağlı)-Bu, bağıntı kimliği, ileti kimliği, etiket, ReplyTo, yaşam SÜRESI (TTL) ve zamanlanan sıraya alma süresi (zamanlanan iletiler için) içerir.
4. **Özel özellikleri** ayarlama-bir sözlük anahtarına göre ayarlanmış herhangi bir Kullanıcı özelliği olabilir.

İleti oluşturulduktan sonra Gönder ' i ziyaret edin.

:::image type="content" source="./media/service-bus-explorer/send-experience.png" alt-text="Ileti oluştur":::

Gönderme işlemi başarıyla tamamlandığında, 

* Sıraya gönderiyorsanız, **etkin iletiler** ölçüm sayacı artacaktır.

    :::image type="content" source="./media/service-bus-explorer/queue-after-send-metrics.png" alt-text="Queueaftersendölçümleri":::

* Konuya gönderim yaparsanız, **etkin iletiler** ölçüm sayacı Iletinin yönlendirildiği abonelikte artacaktır.

    :::image type="content" source="./media/service-bus-explorer/topic-after-send-metrics.png" alt-text="Topicaftersendölçümlerini":::

### <a name="receiving-a-message-from-a-queue"></a>Kuyruktan ileti alma

Service Bus Gezgini 'ndeki Receive işlevi aynı anda tek bir ileti almaya izin verir. Alma işlemi, **Receiveanddelete** modu kullanılarak gerçekleştirilir.

> [!IMPORTANT]
> Service Bus gezgin tarafından gerçekleştirilen alma işleminin ***bozucu bir alım*** olduğunu unutmayın, yani ileti, Service Bus Gezgini aracında görüntülendiğinde kuyruktan kaldırılır.
>
> Sıradan kaldırmadan iletilere gözatmak için, göz ***atma*** işlevini kullanmayı göz önünde bulundurun.
>

Kuyruktan (veya sahipsiz alt sıradan) ileti almak için 

1. Service Bus Gezgini 'ndeki ***Al*** sekmesine tıklayın.
2. **Etkin mesajlar** veya alma Için **atılacak iletiler** olup olmadığını görmek için ölçümleri denetleyin.

    :::image type="content" source="./media/service-bus-explorer/queue-after-send-metrics.png" alt-text="Queueaftersendölçümleri":::

3. * Queue _ veya _ *_Deadletter_** alt **sırası** arasında seçim yapın.

    :::image type="content" source="./media/service-bus-explorer/queue-or-deadletter.png" alt-text="QueueOrDeadletter":::

4. ' Al ve Sil ' işlemini onaylamak için ***Al** _ düğmesine ve ardından _ *_Yes_** ' e tıklayın.


Alma işlemi başarılı olduğunda ileti ayrıntıları kılavuzda aşağıda gösterildiği gibi görüntülenir. Ayrıntılarını göstermek için kılavuzdan iletiyi seçebilirsiniz.

:::image type="content" source="./media/service-bus-explorer/receive-message-from-queue-2.png" alt-text="Kuyrukta seçili bir ileti için görüntülenen ileti ayrıntılarıyla birlikte Azure Service Bus Gezgini 'ndeki kuyruklar penceresinin ekran görüntüsü.":::


### <a name="peeking-a-message-from-a-queue"></a>Kuyruktaki bir iletiye göz atma

Göz atma işleviyle, Service Bus gezginini kullanarak bir kuyruktaki en üstteki 32 iletileri veya sahipsiz sırayı görüntüleyebilirsiniz.

1. Kuyruktaki iletiye gözatmak için Service Bus Gezgini ' nde ***Peek*** sekmesine tıklayın.

    :::image type="content" source="./media/service-bus-explorer/peek-tab-selected.png" alt-text="PeekTab":::

2. Gözatmak üzere **etkin mesajlar** veya **atılacak iletiler** olup olmadığını görmek için ölçümleri denetleyin.

    :::image type="content" source="./media/service-bus-explorer/queue-after-send-metrics.png" alt-text="Queueaftersendölçümleri":::

3. Sonra * Queue _ veya _ *_Deadletter_** alt **sırası** arasında seçim yapın.

    :::image type="content" source="./media/service-bus-explorer/queue-or-deadletter.png" alt-text="QueueOrDeadletter":::

4. ***Göz at*** düğmesine tıklayın. 

Göz atma işlemi tamamlandıktan sonra, en fazla 32 ileti, kılavuzda aşağıda gösterildiği gibi görünecektir. Belirli bir iletinin ayrıntılarını görüntülemek için kılavuzdan seçin. 

:::image type="content" source="./media/service-bus-explorer/peek-message-from-queue-2.png" alt-text="PeekMessageFromQueue":::

> [!NOTE]
>
> Peek bir bozucu işlem **olmadığından ileti kuyruktan** kaldırılmaz.
>

### <a name="receiving-a-message-from-a-subscription"></a>Abonelikten ileti alma

Bir kuyrukla tıpkı, ***alma*** işlemi bir abonelik (veya sahipsiz varlık) için de gerçekleştirilebilir. Ancak, bir abonelik konu bağlamı içinde bulunduğundan alma işlemi, belirli bir konu için Service Bus Gezgini 'ne gidilerek gerçekleştirilir.

> [!IMPORTANT]
> Service Bus gezgin tarafından gerçekleştirilen alma işleminin ***bozucu bir alım*** olduğunu unutmayın, yani ileti, Service Bus Gezgini aracında görüntülendiğinde kuyruktan kaldırılır.
>
> Sıradan kaldırmadan iletilere gözatmak için, göz ***atma*** işlevini kullanmayı göz önünde bulundurun.
>

1. ***Al** _ sekmesine tıklayın ve açılan seçicideki belirli _ *_aboneliği_** öğesini seçin.

    :::image type="content" source="./media/service-bus-explorer/receive-subscription-tab-selected.png" alt-text="ReceiveTabSelected":::

2. ***Subscription** _ veya _ *_deadmektup_** alt varlık arasında seçim yapın.

    :::image type="content" source="./media/service-bus-explorer/subscription-or-deadletter.png" alt-text="Abonelik Orden büyük harf":::

3. ' Al ve Sil ' işlemini onaylamak için ***Al** _ düğmesine ve ardından _ *_Yes_** ' e tıklayın.

Alma işlemi başarılı olduğunda, alınan ileti aşağıda gösterildiği gibi kılavuzda görüntülenecektir. İleti ayrıntılarını görüntülemek için iletiye tıklayın.

:::image type="content" source="./media/service-bus-explorer/receive-message-from-subscription.png" alt-text="Azure Service Bus Gezgini 'ndeki al sekmesinin, alınmış etkin bir ileti için ileti ayrıntılarıyla görüntülenen ekran görüntüsü.":::

### <a name="peeking-a-message-from-a-subscription"></a>Bir abonelikten bir iletiye göz atma

Bir abonelikteki iletilere veya sahipsiz alt varlığına gözatmak için, abonelik üzerinde ***göz atma*** işlevi de kullanılabilir.

1. ***Peek** _ sekmesine tıklayın ve açılır seçicideki belirli _ *_abonelik_** öğesini seçin.

    :::image type="content" source="./media/service-bus-explorer/peek-subscription-tab-selected.png" alt-text="PeekTabSelected":::

2. ***Subscription** _ veya _ *_deadmektup_** alt varlığı arasında seçim yapın.

    :::image type="content" source="./media/service-bus-explorer/subscription-or-deadletter.png" alt-text="Abonelik Orden büyük harf":::

3. ***Göz at*** düğmesine tıklayın.

Göz atma işlemi tamamlandıktan sonra, en fazla 32 ileti, kılavuzda aşağıda gösterildiği gibi görünecektir. Belirli bir iletinin ayrıntılarını görüntülemek için kılavuzdan seçin. 

:::image type="content" source="./media/service-bus-explorer/peek-message-from-subscription.png" alt-text="PeekMessageFromSubscription":::

> [!NOTE]
>
> Peek bir bozucu işlem **olmadığından ileti kuyruktan** kaldırılmaz.
>

## <a name="next-steps"></a>Sonraki Adımlar

   * Service Bus [kuyrukları](service-bus-queues-topics-subscriptions.md#queues) ve [konuları](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) hakkında daha fazla bilgi edinin
   * [Azure Portal aracılığıyla Service Bus kuyrukları](service-bus-quickstart-portal.md) oluşturma hakkında daha fazla bilgi edinin
   * [Azure Portal aracılığıyla Service Bus konuları ve abonelikleri](service-bus-quickstart-topics-subscriptions-portal.md) oluşturma hakkında daha fazla bilgi edinin