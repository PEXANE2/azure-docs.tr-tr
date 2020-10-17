---
title: Azure IoT Hub 500 xxx Iç hatalarında sorun giderme
description: 500 xxx Iç hatalarının nasıl düzeltileceğini anlayın
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: f83ec4d09389d91c2dd427c9840885c0b829eafb
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92150338"
---
# <a name="500xxx-internal-errors"></a>500xxx Internal errors

Bu makalede, **500 xxx iç hatalara**yönelik nedenler ve çözümler açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

IoT Hub isteğiniz, 500 ve/veya bazı "sunucu hatası" sıralaması ile başlayan bir hata ile başarısız olur. Bazı olanaklar şunlardır:

* **500001 Sunucuhatası**: IoT Hub sunucu tarafında bir sorun oluştu.

* **500008 GenericTimeout**: IoT Hub zaman aşımından önce bağlantı isteğini tamamlayamadı.

* **Serviceunavailable (hata kodu yok)**: IoT Hub bir iç hatayla karşılaştı.

* **Internalservererror (hata kodu yok)**: IoT Hub bir iç hatayla karşılaştı.

## <a name="cause"></a>Nedeni

500 xxx hata yanıtının bazı nedenleri olabilir. Her durumda, sorun büyük olasılıkla geçicidir. IoT Hub takım [SLA](https://azure.microsoft.com/support/legal/sla/iot-hub/)'yı sürdürmek için zor çalışırken, IoT Hub düğümlerin küçük alt kümeleri zaman zaman geçici hatalara karşılaşabilir. Cihazınız sorun yaşayan bir düğüme bağlanmaya çalıştığında, bu hatayı alırsınız.

## <a name="solution"></a>Çözüm

500 xxx hata sorunlarını azaltmak için cihazdan yeniden deneme yapın. [Yeniden denemeleri otomatik olarak yönetmek](./iot-hub-reliability-features-in-sdks.md#connection-and-retry)Için [Azure IoT SDK](./iot-hub-devguide-sdks.md)'larının en son sürümünü kullandığınızdan emin olun. Geçici hata işleme ve yeniden denemeler için en iyi yöntem için bkz. [geçici hata işleme](/azure/architecture/best-practices/transient-faults).  Sorun devam ederse, IoT Hub bilinen bir sorun olup olmadığını görmek için [kaynak durumu](./iot-hub-monitor-resource-health.md#use-azure-resource-health) ve [Azure durumunu](https://status.azure.com/) kontrol edin. [El ile yük devretme özelliğini](./tutorial-manual-failover.md)de kullanabilirsiniz. Bilinen bir sorun yoksa ve sorun devam ederse, daha fazla araştırma için [desteğe başvurun](https://azure.microsoft.com/support/options/) .