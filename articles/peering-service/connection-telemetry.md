---
title: Azure eşleme hizmeti bağlantı telemetrisi
description: Microsoft Azure eşleme hizmeti bağlantı telemetrisi hakkında bilgi edinin
services: peering-service
author: derekolo
ms.service: peering-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/18/2020
ms.author: derekol
ms.openlocfilehash: 0e54b20cc6aefa69226c24f557ae02166dc4b4b6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84872011"
---
# <a name="peering-service-connection-telemetry"></a>Eşleme hizmeti bağlantı telemetrisi

Bağlantı telemetrisi, müşterinin konumu ile Microsoft ağı arasındaki bağlantı için toplanan öngörüleri sağlar. Müşteriler Azure portal bağlantıyı kaydederek Azure eşleme hizmeti bağlantısı için telemetri alabilir. Bu özellik, ağ performansına ilişkin ön ek güvenlik ve öngörüler sağlar.


Bağlantı telemetrisi aşağıdaki kapsamlardan oluşur:

### <a name="latency-measurement"></a>Gecikme süresi ölçümü

 Gecikme süresi, istemciden, eşleme hizmeti içindeki kayıtlı ön ekler için Microsoft Edge PoP 'una ölçülür.

### <a name="route-prefix-monitoring-and-protection"></a>Rota ön eki izleme ve koruma

Yönlendirme yolları, daha sonra olay günlüklerinde yakalanan tüm şüpheli etkinlikler için izlenir. Örneğin, bu faktörlerin bazıları için olay günlükleri oluşturulur:

- Ön ek ele geçirmeleri
- Önek çekme al
- Yol sızıntıları

## <a name="next-steps"></a>Sonraki adımlar

- Eşleme hizmeti bağlantısı hakkında bilgi edinmek için bkz. [eşleme hizmeti bağlantısı](connection.md).
- Bir eşleme hizmeti bağlantısı eklemek için bkz. [eşleme hizmeti modeli ekleme](onboarding-model.md).
- Telemetriyi ölçmek için bkz. [ölçüm bağlantı telemetrisi](measure-connection-telemetry.md).
