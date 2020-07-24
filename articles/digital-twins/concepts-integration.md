---
title: Diğer hizmetlerle tümleştirme
titleSuffix: Azure Digital Twins
description: Azure dijital TWINS dağıtımı sırasında giriş ve çıkış gereksinimlerini anlayın.
author: baanders
ms.author: baanders
ms.date: 3/16/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: d82289366d79f44460b96d75781cac4fa50de53a
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87129738"
---
# <a name="integrate-azure-digital-twins-with-other-services"></a>Azure dijital TWINS 'i diğer hizmetlerle tümleştirme

Azure dijital TWINS genellikle diğer hizmetlerle birlikte kullanılır. Azure dijital TWINS, [**olay yollarını**](concepts-route-events.md)kullanarak telemetri ve bildirimler sunmak için kullanılan [IoT Hub](../iot-hub/about-iot-hub.md)gibi yukarı akış hizmetlerinden veri alır. Azure dijital TWINS Ayrıca depolama, iş akışı tümleştirme, analiz ve diğer kullanımlar için aşağı akış hizmetlerine veri yönlendirebilir. 

## <a name="data-ingress"></a>Veri girişi

Azure dijital TWINS, IoT Hub verilerle yönetilebilir. Bu sayede ortamınızdaki fiziksel cihazlardan telemetri toplayabilir ve Bulutta Azure dijital TWINS grafiğini kullanarak bu verileri işleyebilirsiniz.

Azure dijital TWINS 'in yerleşik IoT Hub yok. Şu anda üretimde olan mevcut bir IoT Hub kullanabilir veya yeni bir tane dağıtabilirsiniz. Bu, IoT Hub tüm cihaz yönetimi özelliklerine tam erişim sağlar.

Azure dijital TWINS 'e IoT Hub verileri almak için bir [Azure işlevi](../azure-functions/functions-overview.md)kullanın. Bu model hakkında daha fazla bilgi edinin [*IoT Hub nasıl yapılır: alma telemetriyle*](how-to-ingest-iot-hub-data.md)ilgili daha fazla bilgi edinin veya Azure dijital TWINS [*öğreticisinde kendiniz deneyin: uçtan uca bir çözümü bağlama*](tutorial-end-to-end.md).

## <a name="data-egress-services"></a>Veri çıkış Hizmetleri

Azure dijital TWINS, bağlı **uç noktalara**veri gönderebilir. Desteklenen uç noktalar şu olabilir:
* [Olay Hub’ı](../event-hubs/event-hubs-about.md)
* [Event Grid](../event-grid/overview.md)
* [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md)

Uç noktalar, yönetim API 'Leri veya Azure portal kullanılarak Azure dijital TWINS 'e eklenir. [*Nasıl yapılır: uç noktaları ve yolları yönetme*](how-to-manage-routes.md)bölümünde Azure dijital TWINS 'e bir uç nokta iliştirme hakkında daha fazla bilgi edinin.

Verilerinizi [Azure depolama](../storage/common/storage-introduction.md) veya [Time Series Insights](../time-series-insights/time-series-insights-update-overview.md)gibi son olarak yönlendirmek isteyebileceğiniz birçok farklı hizmet vardır. Verilerinizi bunlar gibi hizmetlere göndermek için, hedef hizmeti bir uç noktaya bağlayın.

Örneğin, [Azure haritalar](../azure-maps/about-azure-maps.md) 'ı da kullanıyorsanız ve Azure dijital TWINS [ikizi Graf](concepts-twins-graph.md)ile konum ilişkilendirmek istiyorsanız, dağıtımınızdaki tüm hizmetler arasında iletişim kurmak Için Event Grid ile Azure işlevleri 'ni kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Dış hizmetlere yönelik uç noktalar ve yönlendirme olayları hakkında daha fazla bilgi edinin:
* [*Kavramlar: Azure dijital TWINS olaylarını yönlendirme*](concepts-route-events.md)

IoT Hub verileri almak için Azure dijital TWINS ayarlama hakkında bilgi alın:
* [*Nasıl yapılır: IoT Hub 'dan alma telemetrisi*](how-to-ingest-iot-hub-data.md)
