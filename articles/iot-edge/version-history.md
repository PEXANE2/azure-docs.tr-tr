---
title: IoT Edge sürümü gezintisi ve geçmişi-Azure IoT Edge
description: En son sürümlerde yeni özellikler ve yetenekler hakkında bilgi içeren IoT Edge yenilikleri öğrenin.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/01/2021
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 877b488fc888b4ced8165c19bcd5c51b5f982745
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "103492515"
---
# <a name="azure-iot-edge-versions-and-release-notes"></a>Azure IoT Edge sürümleri ve sürüm notları

[!INCLUDE [iot-edge-version-all-supported](../../includes/iot-edge-version-all-supported.md)]

Azure IoT Edge, GitHub üzerinde barındırılan açık kaynaklı IoT Edge projesinden oluşturulan bir üründür. Tüm yeni yayınlar [Azure IoT Edge projesinde](https://github.com/Azure/azure-iotedge)kullanılabilir hale getirilir. Katkıları ve hata raporlarını [Açık kaynaklı IoT Edge projesi](https://github.com/Azure/iotedge)üzerinde yapabilirsiniz.

## <a name="documented-versions"></a>Belgelenmiş sürümler

Bu sitedeki IoT Edge belge, ürünün iki farklı sürümünde sunulmaktadır. böylece, IoT Edge ortamınız için geçerli olan içeriği seçebilmenizi sağlayabilirsiniz. Şu anda desteklenen iki sürüm şunlardır:

* **IoT Edge 1,1 (LTS)** , IoT Edge ilk uzun vadeli destek (LTS) sürümüdür. Bu sürümdeki belgeler, önceki tüm sürümlere ait tüm özellikleri ve yetenekleri 1,1 aracılığıyla ele alır. Bu belge sürümü, 1,1 sürümünün desteklenen ömrü boyunca kararlı olacak ve sonraki sürümlerde yayınlanan yeni özellikleri yansıtmayacaktır. 1,1 sürümü, IoT Edge en son genel kullanıma hazır sürümüdür.
* **IoT Edge 1,2 (Önizleme)** , en son önizleme sürümünde [1,2-RC4](https://github.com/Azure/azure-iotedge/releases/tag/1.2.0-rc4)olan özellikler ve yetenekler için ek içerik içerir.

IoT Edge sürümleri hakkında daha fazla bilgi için bkz. [Azure IoT Edge desteklenen sistemler](support.md).

## <a name="version-history"></a>Sürüm geçmişi

Bu tabloda, IoT Edge paket sürümleri için son sürüm geçmişi ve her bir sürüm için yapılan önemli belgelerde bulunan belge güncelleştirmeleri sağlanmaktadır.

| Sürüm notları ve varlıkları | Tür | Tarih | Önemli Noktalar |
| ------------------------ | ---- | ---- | ---------- |
| [1,2-RC4](https://github.com/Azure/azure-iotedge/releases/tag/1.2.0-rc1) | Önizleme | Mart 2021 | Yeni IoT Edge paketleri yeni yükleme ve yapılandırma adımlarıyla kullanıma sunulmuştur. Daha fazla bilgi için bkz. [güncelleştirme 1,0 veya 1,1 ile 1,2](how-to-update-iot-edge.md#special-case-update-from-10-or-11-to-12).
| [1.1](https://github.com/Azure/azure-iotedge/releases/tag/1.1.0) | Uzun süreli destek (LTS) | Şubat 2021 | [Uzun süreli destek planı ve desteklenen sistem güncelleştirmeleri](support.md) |
| [1,2-RC1](https://github.com/Azure/azure-iotedge/releases/tag/1.2.0-rc1) | Önizleme | Kasım 2020 | [Ağ geçitlerinin arkasındaki Cihazları IoT Edge](how-to-connect-downstream-iot-edge-device.md?view=iotedge-2020-11&preserve-view=true)<br>[IoT Edge MQTT Aracısı](how-to-publish-subscribe.md?view=iotedge-2020-11&preserve-view=true) |
| [1.0.10](https://github.com/Azure/azure-iotedge/releases/tag/1.0.10) | Dengeli | Ekim 2020 | [Uploadsupportdemeti doğrudan yöntemi](how-to-retrieve-iot-edge-logs.md#upload-support-bundle-diagnostics)<br>[Çalışma zamanı ölçümlerini karşıya yükle](how-to-access-built-in-metrics.md)<br>[Yönlendirme önceliği ve yaşam süresi](module-composition.md#priority-and-time-to-live)<br>[Modül başlangıç sırası](module-composition.md#configure-modules)<br>[X. 509.440 el ile sağlama](how-to-register-device.md) |
| [1.0.9](https://github.com/Azure/azure-iotedge/releases/tag/1.0.9) | Dengeli | Mart 2020 | [DPS ile X. 509.440 otomatik sağlama](how-to-auto-provision-x509-certs.md)<br>[RestartModule Direct yöntemi](how-to-edgeagent-direct-method.md#restart-module)<br>[support-demeti komutu](troubleshoot.md#gather-debug-information-with-support-bundle-command) |

## <a name="next-steps"></a>Sonraki adımlar

* [Tüm Azure IoT Edge sürümlerini görüntüle](https://github.com/Azure/azure-iotedge/releases)
* [Geri bildirim forumundaki özellik isteklerini oluşturun veya gözden geçirin](https://feedback.azure.com/forums/907045-azure-iot-edge)