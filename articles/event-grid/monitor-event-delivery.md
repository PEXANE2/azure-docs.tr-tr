---
title: İleti teslimini izleme Azure Event Grid
description: Bu makalede, Azure Event Grid iletilerinin teslim durumunu görmek için Azure portal nasıl kullanılacağı açıklanır.
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 16587feaca65aa21836d9be1c44e00faa0f4f8d8
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76722144"
---
# <a name="monitor-event-grid-message-delivery"></a>İleti teslimini izleme Event Grid 

Bu makalede, olay teslimlerin durumunu görmek için Portal 'ın nasıl kullanılacağı açıklanır.

Event Grid dayanıklı teslim sağlar. Her bir abonelik için her iletiyi en az bir kez sunar. Olaylar, her aboneliğin kayıtlı Web kancasına hemen gönderilir. Web kancası ilk teslim girişiminin 60 saniye içinde bir olayın alındığını kabul etmezse, olayın yeniden denenmesini Event Grid.

Olay teslimi ve yeniden deneme hakkında bilgi için [Event Grid iletiyi teslim ve yeniden deneme](delivery-and-retry.md).

## <a name="delivery-metrics"></a>Teslim ölçümleri

Portal, olay iletilerinin teslim durumunun ölçümlerini görüntüler.

Konular için ölçümler şunlardır:

* **Yayımlama başarılı**: olay konuya başarıyla gönderildi ve 2xx yanıtıyla işlendi.
* **Yayımlama başarısız oldu**: konuya gönderilen olay, ancak bir hata koduyla reddedildi.
* **Eşleşmeyen**: olay, konuya başarıyla yayımlandı, ancak olay aboneliğiyle eşleşmedi. Olay bırakıldı.

Abonelikler için ölçümler şunlardır:

* **Teslim başarılı**: etkinlik, aboneliğin uç noktasına başarıyla teslim edildi ve 2xx yanıtı aldı.
* **Teslim başarısız oldu**: aboneliğin uç noktasına gönderilen olay, ancak 4xx veya 5xx yanıtı aldı.
* **Vadesi geçen olaylar**: olay teslim edilmemiş ve tüm yeniden deneme girişimleri gönderildi. Olay bırakıldı.
* **Eşleşen olaylar**: konudaki olay, olay aboneliği tarafından eşleşti.

## <a name="event-subscription-status"></a>Olay aboneliği durumu

Bir olay aboneliğine yönelik ölçümleri görmek için, abonelik türüne göre veya belirli bir kaynak için abonelikler ile arama yapabilirsiniz.

Olay aboneliği türüne göre arama yapmak için **tüm hizmetler**' i seçin.

![Tüm hizmetleri seçin](./media/monitor-event-delivery/all-services.png)

**Olay Kılavuzu** araması yapın ve kullanılabilir seçeneklerden **abonelikler Event Grid** seçin.

![Olay abonelikleri ara](./media/monitor-event-delivery/search-and-select.png)

Olay türüne, aboneliğe ve konuma göre filtreleyin. Görüntülenecek abonelik için **ölçümleri** seçin.

![Olay aboneliklerini filtrele](./media/monitor-event-delivery/filter-events.png)

Olay konusu ve abonelik için ölçümleri görüntüleyin.

![Olay ölçümlerini görüntüle](./media/monitor-event-delivery/subscription-metrics.png)

Belirli bir kaynakla ilgili ölçümleri bulmak için bu kaynağı seçin. Ardından, **Olaylar**' ı seçin.

![Bir kaynak için olayları seçin](./media/monitor-event-delivery/select-events.png)

Bu kaynakla ilgili abonelikler için ölçümleri görürsünüz.

## <a name="custom-event-status"></a>Özel olay durumu

Özel bir konu yayımladıysanız, onun ölçümlerini görüntüleyebilirsiniz. Konunun kaynak grubunu seçin ve konuyu seçin.

![Özel konu seçme](./media/monitor-event-delivery/select-custom-topic.png)

Özel olay konusu için ölçümleri görüntüleyin.

![Olay ölçümlerini görüntüle](./media/monitor-event-delivery/custom-topic-metrics.png)

## <a name="set-alerts"></a>Uyarılar ayarlama

Özel konular ve olay etki alanları için konu başlığı ve etki alanı düzeyi ölçümlerinde uyarılar ayarlayabilirsiniz. İçin genel bakış dikey penceresinde, uyarı kurallarını görüntülemek, yönetmek ve oluşturmak için sol taraftaki kaynak menüsünde **Uyarılar** ' ı seçin. [Azure Izleyici uyarıları hakkında daha fazla bilgi edinin](../azure-monitor/platform/alerts-overview.md)

![Olay ölçümlerini görüntüle](./media/monitor-event-delivery/select-alerts.png)

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimi ve yeniden deneme hakkında bilgi için [Event Grid iletiyi teslim ve yeniden deneme](delivery-and-retry.md).
* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Event Grid ile hızla çalışmaya başlamak için bkz: [Azure Event Grid ile özel olaylar oluşturma ve yönlendirme](custom-event-quickstart.md).
