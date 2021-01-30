---
title: Service Bus üzerinde veri işlemleri gerçekleştirmek için Azure Service Bus Gezginini Kullanma (Önizleme)
description: Bu makalede, Azure Service Bus verilerine erişmek için portal tabanlı Azure Service Bus Gezginini kullanma hakkında bilgi sağlanır.
ms.topic: conceptual
ms.date: 01/20/2020
ms.openlocfilehash: 06bc2ffffc6b467aa730e16599099bc95117dac9
ms.sourcegitcommit: b4e6b2627842a1183fce78bce6c6c7e088d6157b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99092226"
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
> Azure Service Bus gezgin Aracı, topluluğun sahip olduğu OSS aracı [Service Bus gezgin](https://github.com/paolosalvatori/ServiceBusExplorer)**değildir**.
>

## <a name="prerequisites"></a>Önkoşullar

Service Bus Explorer aracını kullanmak için aşağıdaki görevleri gerçekleştirmeniz gerekir: 

- Azure Service Bus ad alanı sağlayın.
- İşlevselliği sınamak için bir abonelik içeren bir veya bir konuya ileti gönderecek veya bir konuyla ilgili bir kuyruk oluşturun. Kuyruklar, konular ve abonelikler oluşturmayı öğrenmek için aşağıdaki makalelere bakın: 
    - [Hızlı başlangıç-kuyruklar oluşturma](service-bus-quickstart-portal.md)
    - [Hızlı başlangıç-konu oluşturma](service-bus-quickstart-topics-subscriptions-portal.md)
- Ad alanındaki bu rollerden birinin üyesi olduğunuzdan emin olun: 
    - [Service Bus veri sahibi](/azure/role-based-access-control/built-in-roles#azure-service-bus-data-owner) 
    - [Katkıda Bulunan](/azure/role-based-access-control/built-in-roles#contributor) 
    - [Sahibi](/azure/role-based-access-control/built-in-roles#owner)


## <a name="using-the-service-bus-explorer"></a>Service Bus Gezginini Kullanma

Azure Service Bus Gezginini kullanmak için, gönderme, göz atma ve alma işlemlerini gerçekleştirmek istediğiniz Service Bus ad alanına gitmeniz gerekir.

Bir sıraya karşı işlem gerçekleştirmek istiyorsanız, gezinti menüsünden _ *' kuyruklar '** seçeneğini belirleyin. Bir konuya (ve ilgili aboneliklerine) yönelik işlemler gerçekleştirmek istiyorsanız **konuları** seçin. 

:::image type="content" source="./media/service-bus-explorer/queue-topics-left-navigation.png"alt-text="Varlık seçimi":::

**' Kuyruklar '** veya **' konular '** seçeneğini belirledikten sonra, belirli kuyruğu veya konuyu seçin.

Sol gezinti menüsünden **' Service Bus Gezgini 'ni (Önizleme) '** seçin

:::image type="content" source="./media/service-bus-explorer/left-navigation-menu-selected.png" alt-text="SB Gezgini sol gezinti menüsü":::

### <a name="sending-a-message-to-a-queue-or-topic"></a>Kuyruğa veya konuya ileti gönderme

Bir **kuyruğa** veya **konuya** bir ileti göndermek için, Service Bus Gezgini ' nde **_Gönder_* _ sekmesine tıklayın.

Burada bir ileti oluşturmak için 

1. _ *Içerik türünü** ' text/plain ', ' Application/XML ' veya ' Application/JSON ' olacak şekilde seçin.
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
> Lütfen Service Bus gezgin tarafından gerçekleştirilen alma işleminin bir **_bozucu alma_* _ olduğunu, yani Ileti Service Bus gezgin aracında görüntülendiğinde kuyruktan kaldırıldığını unutmayın.
>
> Sıradan kaldırmadan iletilere gözatmak için, göz _*_atma_*_ işlevini kullanmayı göz önünde bulundurun.
>

Kuyruktan (veya sahipsiz alt sıradan) ileti almak için 

1. Service Bus Gezgini 'ndeki _*_Al_*_ sekmesine tıklayın.
2. _ *Etkin iletiler** veya alma Için **atılacak ileti** olup olmadığını görmek için ölçümleri denetleyin.

    :::image type="content" source="./media/service-bus-explorer/queue-after-send-metrics.png" alt-text="Queueaftersendölçümleri":::

3. **_Queue_* _ veya _*_sahipsiz_*_ alt sıra arasında seçim yapın.

    :::image type="content" source="./media/service-bus-explorer/queue-or-deadletter.png" alt-text="QueueOrDeadletter":::

4. _*_Al_*_ düğmesine ve ardından ' al ve Sil ' işlemini onaylamak için _*_Evet_*_ ' e tıklayın.


Alma işlemi başarılı olduğunda ileti ayrıntıları kılavuzda aşağıda gösterildiği gibi görüntülenir. Ayrıntılarını göstermek için kılavuzdan iletiyi seçebilirsiniz.

:::image type="content" source="./media/service-bus-explorer/receive-message-from-queue-2.png" alt-text="Kuyrukta seçili bir ileti için görüntülenen ileti ayrıntılarıyla birlikte Azure Service Bus Gezgini 'ndeki kuyruklar penceresinin ekran görüntüsü.":::


### <a name="peeking-a-message-from-a-queue"></a>Kuyruktaki bir iletiye göz atma

Göz atma işleviyle, Service Bus gezginini kullanarak bir kuyruktaki en üstteki 32 iletileri veya sahipsiz sırayı görüntüleyebilirsiniz.

1. Kuyruktaki iletiye gözatmak için Service Bus Gezgini ' nde _*_Peek_*_ sekmesine tıklayın.

    :::image type="content" source="./media/service-bus-explorer/peek-tab-selected.png" alt-text="PeekTab":::

2. Gözatmak üzere _ *etkin iletiler** veya **atılacak ileti** olup olmadığını görmek için ölçümleri denetleyin.

    :::image type="content" source="./media/service-bus-explorer/queue-after-send-metrics.png" alt-text="Queueaftersendölçümleri":::

3. Sonra **_Queue_* _ veya _*_sahipsiz_*_ alt sıra arasında seçim yapın.

    :::image type="content" source="./media/service-bus-explorer/queue-or-deadletter.png" alt-text="QueueOrDeadletter":::

4. _*_Göz at_*_ düğmesine tıklayın. 

Göz atma işlemi tamamlandıktan sonra, en fazla 32 ileti, kılavuzda aşağıda gösterildiği gibi görünecektir. Belirli bir iletinin ayrıntılarını görüntülemek için kılavuzdan seçin. 

:::image type="content" source="./media/service-bus-explorer/peek-message-from-queue-2.png" alt-text="PeekMessageFromQueue":::

> [!NOTE]
>
> Peek bir bozucu işlem olmadığından, from iletisi *kuyruktan kaldırılacak.*
>

### <a name="receiving-a-message-from-a-subscription"></a>Abonelikten ileti alma

Bir kuyrukla tıpkı, **_Receive_* _ işlemi bir abonelik (veya sahipsiz varlık) için de gerçekleştirilebilir. Ancak, bir abonelik konu bağlamı içinde bulunduğundan alma işlemi, belirli bir konu için Service Bus Gezgini 'ne gidilerek gerçekleştirilir.

> [!IMPORTANT]
> Service Bus gezgin tarafından gerçekleştirilen alma işleminin _*_bozucu bir alım_*_ olduğunu unutmayın, yani ileti, Service Bus Gezgini aracında görüntülendiğinde kuyruktan kaldırılır.
>
> Sıradan kaldırmadan iletilere gözatmak için, göz _*_atma_*_ işlevini kullanmayı göz önünde bulundurun.
>

1. _*_Al_*_ sekmesine tıklayın ve açılan seçicideki belirli bir _*_aboneliği_*_ seçin.

    :::image type="content" source="./media/service-bus-explorer/receive-subscription-tab-selected.png" alt-text="ReceiveTabSelected":::

2. _*_Abonelik_*_ veya _*_sahipsiz_*_ alt varlık arasında seçim yapın.

    :::image type="content" source="./media/service-bus-explorer/subscription-or-deadletter.png" alt-text="Abonelik Orden büyük harf":::

3. _*_Al_*_ düğmesine ve ardından ' al ve Sil ' işlemini onaylamak için _*_Evet_*_ ' e tıklayın.

Alma işlemi başarılı olduğunda, alınan ileti aşağıda gösterildiği gibi kılavuzda görüntülenecektir. İleti ayrıntılarını görüntülemek için iletiye tıklayın.

:::image type="content" source="./media/service-bus-explorer/receive-message-from-subscription.png" alt-text="Azure Service Bus Gezgini 'ndeki al sekmesinin, alınmış etkin bir ileti için ileti ayrıntılarıyla görüntülenen ekran görüntüsü.":::

### <a name="peeking-a-message-from-a-subscription"></a>Bir abonelikten bir iletiye göz atma

Bir abonelikteki iletilere veya sahipsiz alt varlığına gözatmak için, abonelik üzerinde _*_göz atma_*_ işlevi de kullanılabilir.

1. _*_Göz at_*_ sekmesine tıklayın ve açılan seçicideki belirli bir _*_aboneliği_*_ seçin.

    :::image type="content" source="./media/service-bus-explorer/peek-subscription-tab-selected.png" alt-text="PeekTabSelected":::

2. _*_Abonelik_*_ veya _*_sahipsiz_*_ alt varlık arasında seçim yapın.

    :::image type="content" source="./media/service-bus-explorer/subscription-or-deadletter.png" alt-text="Abonelik Orden büyük harf":::

3. _*_Göz at_*_ düğmesine tıklayın.

Göz atma işlemi tamamlandıktan sonra, en fazla 32 ileti, kılavuzda aşağıda gösterildiği gibi görünecektir. Belirli bir iletinin ayrıntılarını görüntülemek için kılavuzdan seçin. 

:::image type="content" source="./media/service-bus-explorer/peek-message-from-subscription.png" alt-text="PeekMessageFromSubscription":::

> [!NOTE]
>
> Peek bir bozucu işlem olmadığından, from iletisi *kuyruktan kaldırılacak.*
>

## <a name="next-steps"></a>Sonraki Adımlar

   * Service Bus [kuyrukları](service-bus-queues-topics-subscriptions.md#queues) ve [konuları](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) hakkında daha fazla bilgi edinin
   * [Azure Portal aracılığıyla Service Bus kuyrukları](service-bus-quickstart-portal.md) oluşturma hakkında daha fazla bilgi edinin
   * [Azure Portal aracılığıyla Service Bus konuları ve abonelikleri](service-bus-quickstart-topics-subscriptions-portal.md) oluşturma hakkında daha fazla bilgi edinin
