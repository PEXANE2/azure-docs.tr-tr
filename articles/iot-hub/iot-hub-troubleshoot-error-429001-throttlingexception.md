---
title: Azure IoT Hub sorunlarını giderme hatası 429001
description: 429001 hatasını nasıl düzelteceğinizi öğrenin
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: d2f12a6982886eeaa375151c5b8a73acc573aab9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "92545370"
---
# <a name="429001-throttlingexception"></a>429001 ThrottlingException

Bu makalede, **429001 azaltıcı özel durum** hatalarına yönelik nedenler ve çözümler açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

IoT Hub istekleriniz **429001** hatasını vererek başarısız olur.

## <a name="cause"></a>Nedeni

İstenen işlem için IoT Hub [azaltma sınırları](./iot-hub-devguide-quotas-throttling.md) aşıldı.

## <a name="solution"></a>Çözüm

*Telemetri iletisi gönderme denemeleri* ölçüsünü yukarıda belirtilen sınırlara göre karşılaştırarak azaltma sınırına ulaşıp ulaşmayacağını denetleyin. Ayrıca, *azaltma hatası* ölçümünün sayısını da denetleyebilirsiniz. Bu ölçümler hakkında daha fazla bilgi için bkz. [cihaz telemetri ölçümleri](monitor-iot-hub-reference.md#device-telemetry-metrics). IoT Hub 'ınızı izlemenize yardımcı olacak ölçümlerin nasıl kullanılacağı hakkında bilgi için bkz. [monitor IoT Hub](monitor-iot-hub.md).

IoT Hub, yalnızca sınır ihlal edildiğinde çok uzun süre dolduktan sonra 429 kısıtlar Lingexception döndürür. Bu işlem, IoT Hub 'ınız veri bloğu trafiği alırsa iletilerinizin bırakılmaması için yapılır. IoT Hub bu süre boyunca iletileri işlem kısıtlama hızında işlemeye devam eder ve kapsamda çok fazla trafik olması halinde bu hız düşük olabilir. Daha fazla bilgi için bkz. [IoT Hub trafiği şekillendirme](./iot-hub-devguide-quotas-throttling.md#traffic-shaping).

## <a name="next-steps"></a>Sonraki adımlar

Kota veya azaltma [limitleriyle çalışıyorsanız IoT Hub ölçeğini ölçeklendirin](./iot-hub-scaling.md) .