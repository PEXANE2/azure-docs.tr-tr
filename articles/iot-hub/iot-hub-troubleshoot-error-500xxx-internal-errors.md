---
title: Azure IoT Hub 500xxx İç hataları giderme
description: 500xxx İç hataların nasıl düzeltilebildiğini anlama
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 7f3f5177e084693c45bed1088a4e1d091be100ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271050"
---
# <a name="500xxx-internal-errors"></a>500xxx Internal errors

Bu **makalede, 500xxx İç hataların**nedenleri ve çözümleri açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

IoT Hub'a isteğiniz 500 ve/veya bir tür "sunucu hatası" ile başlayan bir hatayla başarısız olur. Bazı olasılıklar şunlardır:

* **500001 ServerError**: IoT Hub sunucu tarafında bir sorunla karşılaştı.

* **500008 GenericTimeout**: IoT Hub zamanlamadan önce bağlantı isteğini tamamlayamadı.

* **ServiceUnavailable (hata kodu yok)**: IoT Hub bir iç hatayla karşılaştı.

* **InternalServerError (hata kodu yok)**: IoT Hub bir iç hatayla karşılaştı.

## <a name="cause"></a>Nedeni

500xxx hata yanıtı için bir dizi neden olabilir. Her durumda, sorun büyük olasılıkla geçicidir. IoT Hub ekibi [SLA'yı](https://azure.microsoft.com/support/legal/sla/iot-hub/)korumak için çok çalışırken, IoT Hub düğümlerinin küçük alt kümeleri bazen geçici hatalarla karşılaşabilir. Aygıtınız sorun olan bir düğüme bağlanmaya çalıştığında, bu hatayı alırsınız.

## <a name="solution"></a>Çözüm

500xxx hatalarını azaltmak için aygıttan yeniden deneme yapın. [Yeniden denemeleri otomatik olarak yönetmek](./iot-hub-reliability-features-in-sdks.md#connection-and-retry)için Azure [IoT SDK'larının](./iot-hub-devguide-sdks.md)en son sürümünü kullandığınızdan emin olun. Geçici hata işleme ve yeniden denemeler üzerinde en iyi uygulama için Geçici [hata işleme](https://docs.microsoft.com/azure/architecture/best-practices/transient-faults)bölümüne bakın.  Sorun devam ederse, IoT Hub'ın bilinen bir sorunu olup olmadığını görmek için [Kaynak Durumu](./iot-hub-monitor-resource-health.md#use-azure-resource-health) ve [Azure Durumu'nu](https://status.azure.com/) denetleyin. [Ayrıca manuel failover özelliğini](./tutorial-manual-failover.md)kullanabilirsiniz. Bilinen bir sorun yoksa ve sorun devam ederse, daha fazla araştırma için [desteğe başvurun.](https://azure.microsoft.com/support/options/)
