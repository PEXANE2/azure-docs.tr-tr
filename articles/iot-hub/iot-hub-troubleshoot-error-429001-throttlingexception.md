---
title: Sorun Giderme Azure IoT Hub hatası 429001 AzaltmaÖzel Durum
description: 429001 ThrottlingException hatasının nasıl düzeltilebildiğini anlayın
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 3095e398d7e5cfe59085144d5bb4e8dc33618064
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76960704"
---
# <a name="429001-throttlingexception"></a>429001 ThrottlingException

Bu **makalede, 429001 ThrottlingException** hatalarının nedenleri ve çözümleri açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

IoT Hub'a istekleriniz **429001 ThrottlingException**hatasıile başarısız olur.

## <a name="cause"></a>Nedeni

İstenen işlem için IoT Hub [azaltma sınırları](./iot-hub-devguide-quotas-throttling.md) aşıldı.

## <a name="solution"></a>Çözüm

*Telemetri iletinizi* karşılaştırarak azaltma sınırına uyup uyup olmadığınızı kontrol edin girişim metriklerini yukarıda belirtilen sınırlara göre karşılaştırın. *Azaltma hataları sayısı* ölçümüne de bakabilirsiniz. IoT Hub için bu ve diğer ölçümler hakkında daha fazla bilgi için [IoT Hub ölçümlerine ve bunların nasıl kullanılacağına](./iot-hub-metrics.md#iot-hub-metrics-and-how-to-use-them)bakın.

IoT Hub, 429 ThrottlingException'ı yalnızca sınır çok uzun bir süre ihlal edildikten sonra döndürür. Bu, IoT hub'ınız patlama trafiği olursa iletilerinizin düşmemesi için yapılır. IoT Hub bu süre boyunca iletileri işlem kısıtlama hızında işlemeye devam eder ve kapsamda çok fazla trafik olması halinde bu hız düşük olabilir. Daha fazla bilgi için bkz. [IoT Hub trafiği şekillendirme](./iot-hub-devguide-quotas-throttling.md#traffic-shaping).

## <a name="next-steps"></a>Sonraki adımlar

Kota veya azaltma sınırlarına koşuyorsanız [IoT Hub'ınızı ölçeklemeyi](./iot-hub-scaling.md) düşünün.