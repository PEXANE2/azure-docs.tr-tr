---
title: Azure Olay Ağı ileti teslimini izleyin
description: Bu makalede, Azure Olay Izgara iletilerinin teslim durumunu görmek için Azure portalının nasıl kullanılacağı açıklanmaktadır.
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 16587feaca65aa21836d9be1c44e00faa0f4f8d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76722144"
---
# <a name="monitor-event-grid-message-delivery"></a>Olay Izgara ileti teslimini izleyin 

Bu makalede, olay teslimatlarının durumunu görmek için portalın nasıl kullanılacağı açıklanmaktadır.

Olay Izgara dayanıklı teslimat sağlar. Her abonelik için her iletiyi en az bir kez iletin. Olaylar hemen her aboneliğin kayıtlı webhook gönderilir. Bir webhook ilk teslimat denemesinden sonraki 60 saniye içinde bir olayın alındığını kabul etmezse, Olay Izgarası etkinliğin teslimini yeniden dener.

Olay teslimi ve yeniden denemeleri hakkında bilgi için, [Olay Grid ileti teslimi ve yeniden deneme.](delivery-and-retry.md)

## <a name="delivery-metrics"></a>Teslimat ölçümleri

Portal, olay iletileri sunma durumuna yönelik ölçümleri görüntüler.

Konular için ölçümler şunlardır:

* **Yayımla Başarılı**: Olay başarıyla konuya gönderildi ve 2xx yanıtı ile işlenir.
* **Yayımlama Başarısız**: Konuya gönderilen ancak bir hata koduyla reddedilen olay.
* **Eşleşmeyen**: Konuya başarıyla yayınlanan ancak bir etkinlik aboneliğiyle eşleşmeyen olay. Olay iptal edildi.

Abonelikler için ölçümler şunlardır:

* **Teslim Başarılı**: Etkinlik başarıyla aboneliğin bitiş noktasına teslim edildi ve 2xx yanıt aldı.
* **Teslim Başarısız :** Olay aboneliğin bitiş noktasına gönderildi, ancak 4xx veya 5xx yanıtı aldı.
* **Süresi Dolan Olaylar**: Olay teslim edilmedi ve tüm yeniden deneme girişimleri gönderildi. Olay iptal edildi.
* **Eşleşen Olaylar**: Konuyla ilgili olay, etkinlik aboneliği ile eşleşti.

## <a name="event-subscription-status"></a>Olay abonelik durumu

Bir etkinlik aboneliğinin ölçümlerini görmek için abonelik türüne veya belirli bir kaynağın aboneliklerine göre arama yapabilirsiniz.

Olay abonelik türüne göre arama yapmak için **Tüm hizmetler'i**seçin.

![Tüm hizmetleri seçin](./media/monitor-event-delivery/all-services.png)

Etkinlik **ızgarasını** arayın ve kullanılabilir seçeneklerden **Olay Izgara Abonelikleri'ni** seçin.

![Etkinlik aboneliklerini ara](./media/monitor-event-delivery/search-and-select.png)

Olay türüne, aboneye ve konuma göre filtre uygulayın. Görüntülemek için abonelik için **Ölçümler'i** seçin.

![Etkinlik aboneliklerini filtreleme](./media/monitor-event-delivery/filter-events.png)

Etkinlik konusu ve abonelik ölçümlerini görüntüleyin.

![Olay ölçümlerini görüntüleme](./media/monitor-event-delivery/subscription-metrics.png)

Belirli bir kaynağın ölçümlerini bulmak için bu kaynağı seçin. Ardından, **Etkinlikler'i**seçin.

![Kaynak için olayları seçme](./media/monitor-event-delivery/select-events.png)

Bu kaynağın aboneliklerinin ölçümlerini görürsünüz.

## <a name="custom-event-status"></a>Özel olay durumu

Özel bir konu yayımladıysanız, bunun ölçümlerini görüntüleyebilirsiniz. Konu için kaynak grubunu seçin ve konuyu seçin.

![Özel konu seçme](./media/monitor-event-delivery/select-custom-topic.png)

Özel etkinlik konusuiçin ölçümleri görüntüleyin.

![Olay ölçümlerini görüntüleme](./media/monitor-event-delivery/custom-topic-metrics.png)

## <a name="set-alerts"></a>Uyarı ayarlama

Özel Konular ve Olay Etki Alanları için konu ve etki alanı düzeyi ölçümleri hakkında uyarılar ayarlayabilirsiniz. Genel bakış bıçağında, uyarı kurallarını görüntülemek, yönetmek ve oluşturmak için soldaki **Uyarılar'ı** seçin kaynak menüsüvardı. [Azure Monitör Uyarıları hakkında daha fazla bilgi edinin](../azure-monitor/platform/alerts-overview.md)

![Olay ölçümlerini görüntüleme](./media/monitor-event-delivery/select-alerts.png)

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimi ve yeniden denemeleri hakkında bilgi için, [Olay Grid ileti teslimi ve yeniden deneme.](delivery-and-retry.md)
* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Olay Ağıt'ı kullanmaya hızla başlamak için [Azure Olay Ağıtı ile özel etkinlikler oluştur ve yönlendir'e](custom-event-quickstart.md)bakın.
