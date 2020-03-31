---
title: Sorun Giderme Azure IoT Hub hatası 403002 IoTHubQuotaExceeded
description: Hata 403002 IoTHubQuotaExceeded nasıl düzeltilir anlayın
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 8312c3267e826088f34b3bffe1520703eec00bdc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76961120"
---
# <a name="403002-iothubquotaexceeded"></a>403002 IoTHubQuotaExceeded

Bu **makalede, 403002 IoTHubQuotaExceeded** hatalarının nedenleri ve çözümleri açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

IoT Hub tüm istekleri hata **403002 IoTHubQuotaExceeded**ile başarısız . Azure portalında, IoT hub aygıt listesi yüklenmez.

## <a name="cause"></a>Nedeni

IoT hub'ı için günlük ileti kotası aşıldı. 

## <a name="solution"></a>Çözüm

[IoT hub'ındaki birim sayısını yükseltin veya artırın](iot-hub-upgrade.md) veya günlük kotanın yenilenmesi için bir sonraki UTC gününü bekleyin.

## <a name="next-steps"></a>Sonraki adımlar

* İkiz sorgular ve doğrudan yöntemler gibi operasyonların kotaya doğru nasıl sayıldığını anlamak için [Bkz.](iot-hub-devguide-pricing.md#charges-per-operation)
* Günlük kota kullanımı için izleme ayarlamak için, kullanılan *iletilerin*metrik Toplam sayısı ile bir uyarı ayarlayın. Adım adım talimatlar için [bkz.](tutorial-use-metrics-and-diags.md#set-up-metrics)