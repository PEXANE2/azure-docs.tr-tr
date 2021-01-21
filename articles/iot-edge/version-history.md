---
title: IoT Edge sürümü gezintisi ve geçmişi-Azure IoT Edge
description: En son sürümlerde yeni özellikler ve yetenekler hakkında bilgi içeren IoT Edge yenilikleri öğrenin.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/08/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: e7b85f3e01a1c9a9c354599cfed8359b4a56d549
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98631463"
---
# <a name="azure-iot-edge-versions-and-release-notes"></a>Azure IoT Edge sürümleri ve sürüm notları

Azure IoT Edge, GitHub üzerinde barındırılan açık kaynaklı IoT Edge projesinden oluşturulan bir üründür. Tüm yeni yayınlar [Azure IoT Edge projesinde](https://github.com/Azure/azure-iotedge)kullanılabilir hale getirilir. Katkıları ve hata raporlarını [Açık kaynaklı IoT Edge projesi](https://github.com/Azure/iotedge)üzerinde yapabilirsiniz.

## <a name="documented-versions"></a>Belgelenmiş sürümler

Bu sitedeki IoT Edge belge, ürünün iki farklı sürümünde sunulmaktadır. böylece, IoT Edge ortamınız için geçerli olan içeriği seçebilmenizi sağlayabilirsiniz. Şu anda desteklenen iki sürüm şunlardır:

* **IoT Edge 1.0.10** , genel kullanıma sunulan en son sürüm aracılığıyla tüm özellikleri ve özellikleri içerir: [1.0.10](https://github.com/Azure/azure-iotedge/releases/tag/1.0.10).
* **IoT Edge 1,2 (Önizleme)** en son önizleme sürümündeki özellikler ve yetenekler için ek içerik içerir: [1,2-RC1](https://github.com/Azure/azure-iotedge/releases/tag/1.2.0-rc1)
  * IoT Edge 1,2 Önizleme sürümündedir, sürüm adayı sürümlerini yüklemeniz gerekir. Daha fazla bilgi için bkz. [çevrimdışı veya belirli sürümü yükleme](how-to-install-iot-edge.md?tabs=linux#offline-or-specific-version-installation-optional).

## <a name="version-history"></a>Sürüm geçmişi

Bu tabloda, IoT Edge paket sürümleri için son sürüm geçmişi ve her bir sürüm için yapılan önemli belgelerde bulunan belge güncelleştirmeleri sağlanmaktadır.

| Sürüm notları ve varlıkları | Tür | Tarih | Önemli Noktalar |
| ------------------------ | ---- | ---- | ---------- |
| [1,2-RC1](https://github.com/Azure/azure-iotedge/releases/tag/1.2.0-rc1) | Önizleme | Kasım 2020 | [Ağ geçitlerinin arkasındaki Cihazları IoT Edge](how-to-connect-downstream-iot-edge-device.md?view=iotedge-2020-11&preserve-view=true)<br>[IoT Edge MQTT Aracısı](how-to-publish-subscribe.md?view=iotedge-2020-11&preserve-view=true) |
| [1.0.10](https://github.com/Azure/azure-iotedge/releases/tag/1.0.10) | Dengeli | Ekim 2020 | [Uploadsupportdemeti doğrudan yöntemi](how-to-retrieve-iot-edge-logs.md#upload-support-bundle-diagnostics)<br>[Çalışma zamanı ölçümlerini karşıya yükle](how-to-access-built-in-metrics.md)<br>[Yönlendirme önceliği ve yaşam süresi](module-composition.md#priority-and-time-to-live)<br>[Modül başlangıç sırası](module-composition.md#configure-modules)<br>[X. 509.440 el ile sağlama](how-to-register-device.md) |
| [1.0.9](https://github.com/Azure/azure-iotedge/releases/tag/1.0.9) | Dengeli | Mart 2020 | [DPS ile X. 509.440 otomatik sağlama](how-to-auto-provision-x509-certs.md)<br>[RestartModule Direct yöntemi](how-to-edgeagent-direct-method.md#restart-module)<br>[support-demeti komutu](troubleshoot.md#gather-debug-information-with-support-bundle-command) |

## <a name="next-steps"></a>Sonraki adımlar

* [Tüm Azure IoT Edge sürümlerini görüntüle](https://github.com/Azure/azure-iotedge/releases)
* [Geri bildirim forumundaki özellik isteklerini oluşturun veya gözden geçirin](https://feedback.azure.com/forums/907045-azure-iot-edge)