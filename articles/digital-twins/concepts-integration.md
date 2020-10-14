---
title: Diğer hizmetlerle tümleştirme
titleSuffix: Azure Digital Twins
description: Azure dijital TWINS dağıtımı sırasında giriş ve çıkış gereksinimlerini anlayın.
author: baanders
ms.author: baanders
ms.date: 3/16/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 4fcad5798d421939717a6beae01ae9930cfcb117
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92015030"
---
# <a name="integrate-azure-digital-twins-with-other-services"></a>Azure dijital TWINS 'i diğer hizmetlerle tümleştirme

Azure dijital TWINS, genellikle verilerinizi çeşitli yollarla kullanan esnek ve bağlı çözümler oluşturmak için diğer hizmetlerle birlikte kullanılır.

Azure dijital TWINS, [**olay yollarını**](concepts-route-events.md)kullanarak telemetri ve bildirim teslim etmek için kullanılan [IoT Hub](../iot-hub/about-iot-hub.md) veya [Logic Apps](../logic-apps/logic-apps-overview.md)gibi yukarı akış hizmetlerinden veri alabilir. 

Azure dijital TWINS Ayrıca verileri [Azure haritalar](../azure-maps/about-azure-maps.md) ve [Time Series Insights](../time-series-insights/time-series-insights-update-overview.md), depolama, iş akışı tümleştirmesi, analiz ve daha fazlası gibi aşağı akış hizmetlerine yönlendirebilir. 

## <a name="data-ingress"></a>Veri girişi

Azure dijital TWINS;[IoT Hub](../iot-hub/about-iot-hub.md), [Logic Apps](../logic-apps/logic-apps-overview.md), kendi özel hizmetiniz ve daha fazlasını içeren herhangi bir hizmetten gelen verilerle ve etkinliklerle yönetilebilir. Bu sayede ortamınızdaki fiziksel cihazlardan telemetri toplayabilir ve Bulutta Azure dijital TWINS grafiğini kullanarak bu verileri işleyebilirsiniz.

Azure Digital TWINS, arka planda yerleşik IoT Hub sahip olmak yerine, hizmetle birlikte kullanmak için "kendi kendinize getirme" IoT Hub sağlar. Şu anda üretimde mevcut olan bir IoT Hub kullanabilir veya bu amaçla kullanılmak üzere yeni bir tane dağıtabilirsiniz. Bu, IoT Hub tüm cihaz yönetimi özelliklerine tam erişim sağlar.

Herhangi bir kaynaktaki verileri Azure dijital TWINS 'e almak için bir [**Azure işlevi**](../azure-functions/functions-overview.md)kullanın. Bu model hakkında daha fazla bilgi edinin [*IoT Hub nasıl yapılır: alma telemetriyle*](how-to-ingest-iot-hub-data.md)ilgili daha fazla bilgi edinin veya Azure dijital TWINS [*öğreticisinde kendiniz deneyin: uçtan uca bir çözümü bağlama*](tutorial-end-to-end.md). 

Ayrıca [*bkz. nasıl yapılır: Logic Apps Ile tümleştirme Ile*](how-to-integrate-logic-apps.md)Azure dijital twıns 'i Logic Apps tetikleyicisine bağlama hakkında bilgi edinebilirsiniz.

## <a name="data-egress-services"></a>Veri çıkış Hizmetleri

Azure dijital TWINS, bağlı **uç noktalara**veri gönderebilir. Desteklenen uç noktalar şu olabilir:
* [Olay Hub’ı](../event-hubs/event-hubs-about.md)
* [Event Grid](../event-grid/overview.md)
* [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md)

Uç noktalar, yönetim API 'Leri veya Azure portal kullanılarak Azure dijital TWINS 'e eklenir. [*Nasıl yapılır: uç noktaları ve yolları yönetme*](how-to-manage-routes-apis-cli.md)bölümünde Azure dijital TWINS 'e bir uç nokta iliştirme hakkında daha fazla bilgi edinin.

Verilerinizi [Azure depolama](../storage/common/storage-introduction.md), [azure Maps](../azure-maps/about-azure-maps.md)veya [Time Series Insights](../time-series-insights/time-series-insights-update-overview.md)gibi son olarak yönlendirmek isteyebileceğiniz birçok farklı hizmet vardır. Verilerinizi bunlar gibi hizmetlere göndermek için, hedef hizmeti bir uç noktaya bağlayın.

Örneğin, Azure haritalar 'ı da kullanıyorsanız ve Azure dijital TWINS [ikizi Graf](concepts-twins-graph.md)ile konum ilişkilendirmek istiyorsanız, dağıtımınızdaki tüm hizmetler arasında iletişim kurmak için Event Grid Ile Azure işlevleri 'ni kullanabilirsiniz. Bu konuda daha fazla bilgi edinmek [ *Için nasıl yapılır: Azure haritalar giriş eşlemesini güncelleştirmek Için Azure dijital TWINS kullanma*](how-to-integrate-maps.md)

Ayrıca, [*nasıl yapılır: Time Series Insights Ile tümleştirme*](how-to-integrate-time-series-insights.md)' de Time Series Insights benzer bir şekilde verileri nasıl yönlendirebileceğinizi öğrenebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Dış hizmetlere yönelik uç noktalar ve yönlendirme olayları hakkında daha fazla bilgi edinin:
* [*Kavramlar: Azure dijital TWINS olaylarını yönlendirme*](concepts-route-events.md)

IoT Hub verileri almak için Azure dijital TWINS ayarlama hakkında bilgi alın:
* [*Nasıl yapılır: IoT Hub 'dan alma telemetrisi*](how-to-ingest-iot-hub-data.md)
