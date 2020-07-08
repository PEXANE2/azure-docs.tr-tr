---
title: Azure Event Grid ölçümleri görüntüleme ve uyarıları ayarlama
description: Bu makalede, Azure Event Grid konu ve Aboneliklerle ilgili ölçümleri görüntülemek ve bunlar üzerinde uyarı oluşturmak için Azure portal nasıl kullanılacağı açıklanır.
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 06/16/2020
ms.author: spelluru
ms.openlocfilehash: 75311675ae24f4836ed8f1adb8a7d9802b4d7f0d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85390610"
---
# <a name="monitor-event-grid-message-delivery"></a>İleti teslimini izleme Event Grid 
Bu makalede, Event Grid konular ve abonelikler için ölçümleri görmek ve bunlar üzerinde uyarı oluşturmak üzere portalın nasıl kullanılacağı açıklanır. 

## <a name="metrics"></a>Ölçümler

Portal, olay iletilerinin teslim durumunun ölçümlerini görüntüler.

Konular için bazı ölçümler aşağıda verilmiştir:

* **Yayımlama başarılı**: olay konuya başarıyla gönderildi ve 2xx yanıtıyla işlendi.
* **Yayımlama başarısız oldu**: konuya gönderilen olay, ancak bir hata koduyla reddedildi.
* **Eşleşmeyen**: olay, konuya başarıyla yayımlandı, ancak olay aboneliğiyle eşleşmedi. Olay bırakıldı.

Abonelikler için bazı ölçümler aşağıda verilmiştir:

* **Teslim başarılı**: etkinlik, aboneliğin uç noktasına başarıyla teslim edildi ve 2xx yanıtı aldı.
* **Teslim başarısız oldu**: hizmet teslim etmeye her seferinde ve olay işleyicisi başarılı 2xx Kodu döndürmezse, **teslim başarısız** sayacı artırılır. Aynı olayı birden çok kez teslim etmeye ve başarısız olursa, her başarısızlık için **teslim başarısız** sayacı artırılır.
* **Vadesi geçen olaylar**: olay teslim edilmemiş ve tüm yeniden deneme girişimleri gönderildi. Olay bırakıldı.
* **Eşleşen olaylar**: konudaki olay, olay aboneliği tarafından eşleşti.

    > [!NOTE]
    > Ölçümlerin tam listesi için bkz. [Azure Event Grid tarafından desteklenen ölçümler](metrics.md).

## <a name="view-custom-topic-metrics"></a>Özel konu ölçümlerini görüntüleme

Özel bir konu yayımladıysanız, onun ölçümlerini görüntüleyebilirsiniz. 

1. [Azure Portal](https://portal.azure.com/)oturum açın.
2. Konunun arama çubuğunda **Event Grid konular**yazın ve ardından aşağı açılan listeden **Event Grid konular** ' ı seçin. 

    :::image type="content" source="./media/custom-event-quickstart-portal/select-event-grid-topics.png" alt-text="Event Grid konuları arayın ve seçin":::
3. Konu listesinden özel konu başlığı ' nı seçin. 

    :::image type="content" source="./media/monitor-event-delivery/select-custom-topic.png" alt-text="Özel konu başlığını seçin":::
4. **Event Grid konu** sayfasındaki özel olay konusu için ölçümleri görüntüleyin. Aşağıdaki görüntüde, abonelik vb. kaynak grubunu gösteren **temel** bileşenler bölümü simge durumuna küçültülmüş. 

    :::image type="content" source="./media/monitor-event-delivery/custom-topic-metrics.png" alt-text="Olay ölçümlerini görüntüle":::

**Event Grid konu** sayfasının **ölçümler** sekmesini kullanarak desteklenen ölçümlerle grafik oluşturabilirsiniz.

:::image type="content" source="./media/monitor-event-delivery/topics-metrics-page.png" alt-text="Konu-ölçümler sayfası":::

Ölçümler hakkında daha fazla bilgi edinmek için bkz. [Azure izleyici 'de ölçümler](../azure-monitor/platform/data-platform-metrics.md)

Örneğin, **yayımlanan olaylar** ölçümü için ölçüm grafiğine bakın.

:::image type="content" source="./media/monitor-event-delivery/custom-topic-metrics-example.png" alt-text="Yayımlanan olaylar ölçümü":::


## <a name="view-subscription-metrics"></a>Abonelik ölçümlerini görüntüle
1. Önceki bölümde bulunan adımları izleyerek **Event Grid konu** sayfasına gidin. 
2. Aşağıdaki örnekte gösterildiği gibi alt bölmeden aboneliği seçin. 

    :::image type="content" source="./media/monitor-event-delivery/select-event-subscription.png" alt-text="Olay aboneliği seçin":::    

    Ayrıca, bir olay aboneliğini görmek için Azure portal arama çubuğunda **Event Grid abonelikleri** arayabilir, **konu türü**, **abonelik**ve **konum** ' u seçin. 

    :::image type="content" source="./media/monitor-event-delivery/event-subscriptions-page.png" alt-text="Event Grid abonelikler sayfasından olay aboneliği seçin":::        

    Özel Konular için **konu türü**olarak **Event Grid konular** ' ı seçin. Sistem konuları için Azure kaynağının türünü seçin (örneğin, **depolama hesapları (blob, GPv2)**. 
3. Bir grafikteki aboneliğin ana sayfasında, abonelik ölçümlerine bakın. Son 1 saat, 6 saat, 12 saat, 1 gün, 7 gün veya 30 gün için **genel**, **hata**, **gecikme süresi**ve **atılacak mektup** ölçümlerini görebilirsiniz. 

    :::image type="content" source="./media/monitor-event-delivery/subscription-home-page-metrics.png" alt-text="Abonelik giriş sayfasındaki ölçümler":::    

## <a name="view-system-topic-metrics"></a>Sistem konu ölçümlerini görüntüleme

1. [Azure Portal](https://portal.azure.com/)oturum açın.
2. Konunun arama çubuğunda **Event Grid sistem konuları**yazın ve ardından açılır listeden **Event Grid sistem konuları** ' nı seçin. 

    :::image type="content" source="./media/monitor-event-delivery/search-system-topics.png" alt-text="Event Grid sistem konularını arayın ve seçin":::
3. Konu listesinden sistem konu başlığını seçin. 

    :::image type="content" source="./media/monitor-event-delivery/select-system-topic.png" alt-text="Sistem konusunu seçin":::
4. **Event Grid sistem konusu** sayfasında sistem konusunun ölçümlerini görüntüleyin. Aşağıdaki görüntüde, abonelik vb. kaynak grubunu gösteren **temel** bileşenler bölümü simge durumuna küçültülmüş. 

    :::image type="content" source="./media/monitor-event-delivery/system-topic-overview-metrics.png" alt-text="Genel Bakış sayfasında sistem konu ölçümlerini görüntüleme":::

**Event Grid konu** sayfasının **ölçümler** sekmesini kullanarak desteklenen ölçümlerle grafik oluşturabilirsiniz.

:::image type="content" source="./media/monitor-event-delivery/system-topic-metrics-page.png" alt-text="Sistem konusu-ölçümler sayfası":::

Ölçümler hakkında daha fazla bilgi edinmek için bkz. [Azure izleyici 'de ölçümler](../azure-monitor/platform/data-platform-metrics.md)


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- Ölçümler ve etkinlik günlüğü işlemlerinde uyarı oluşturma hakkında bilgi edinmek için bkz. [uyarıları ayarlama](set-alerts.md).
- Olay teslimi ve yeniden denemeler hakkında daha fazla bilgi için [Event Grid ileti teslimi ve yeniden deneyin](delivery-and-retry.md).
