---
title: Azure IoT Hub tanılama ayarlarına geçiş | Microsoft Dokümanlar
description: IoT hub'ınızdaki işlemlerin durumunu gerçek zamanlı olarak izlemek için işlem izleme yerine Azure tanılama ayarlarını kullanmak üzere Azure IoT Hub'ı nasıl güncelleştirebilirsiniz?
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: kgremban
ms.openlocfilehash: ab07da38c01b052a4220274fb059683a22950a3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75750713"
---
# <a name="migrate-your-iot-hub-from-operations-monitoring-to-diagnostics-settings"></a>IoT Hub'ınızı operasyon izlemeden tanılama ayarlarına geçirin

IoT Hub'daki işlemlerin durumunu izlemek için [operasyon izleme](iot-hub-operations-monitoring.md) özelliğini kullanan müşteriler, bu iş akışını Azure Monitor özelliği olan [Azure tanılama ayarlarına](../azure-monitor/platform/platform-logs-overview.md)geçirebilir. Tanılama ayarları, birçok Azure hizmeti için kaynak düzeyinde tanılama bilgileri sağlar.

**IoT Hub'ın operasyon izleme işlevi küçümsenir**ve portaldan kaldırıldı. Bu makalede, iş yüklerinizi operasyon izlemeden tanılama ayarlarına taşımak için adımlar sağlar. Amortisman zaman çizelgesi hakkında daha fazla bilgi için Azure [IoT çözümlerinizi Azure Monitor ve Azure Kaynak Durumu ile izleyin'e](https://azure.microsoft.com/blog/monitor-your-azure-iot-solutions-with-azure-monitor-and-azure-resource-health/)bakın.

## <a name="update-iot-hub"></a>IoT Hub'ı güncelleştir

Azure portalındaki IoT Hub'ınızı güncellemek için önce tanılama ayarlarını açın, ardından işlem izlemeyi kapatın.  

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="turn-off-operations-monitoring"></a>Operasyon izlemeyi kapatma

> [!NOTE]
> 11 Mart 2019 itibariyle, işlemleri izleme özelliği IoT Hub'ın Azure portal arabiriminden kaldırılır. Aşağıdaki adımlar artık geçerli değildir. Geçiş yapmak için, yukarıdaki Azure Monitor tanı ayarlarında doğru kategorilerin açık olduğundan emin olun.

İş akışınızdaki yeni tanılama ayarlarını test ettikten sonra, işlem izleme özelliğini kapatabilirsiniz. 

1. IoT Hub menüsünde **Operasyonlar izleme'yi**seçin.

2. Her izleme kategorisi altında **Yok'u**seçin.

3. Değişiklikleri izleme işlemlerini kaydedin.

## <a name="update-applications-that-use-operations-monitoring"></a>Operasyon izleme kullanan uygulamaları güncelleştirme

Operasyon izleme ve tanılama ayarları için şemalar biraz farklılık gösterir. Tanılama ayarları tarafından kullanılan şemaya eşlemek için bugün operasyon izleme kullanan uygulamaları güncelleştirmeniz önemlidir. 

Ayrıca, tanılama ayarları izleme için beş yeni kategori sunar. Varolan şema uygulamalarını güncelledikten sonra, yeni kategorileri de ekleyin:

* Bulut-aygıtikiz işlemleri
* Aygıt-buluta ikiz işlemleri
* İkiz sorgular
* İş işlemleri
* Doğrudan Yöntemler

Belirli şema yapıları için [tanılama ayarları için şema anlayın'](iot-hub-monitor-resource-health.md#understand-the-logs)a bakın.

## <a name="monitoring-device-connect-and-disconnect-events-with-low-latency"></a>Düşük gecikme gecikmesi ile olayları bağlama ve kesme

Cihaz bağlantısını izlemek ve üretimdeki olayları kesmek için, uyarı almak ve aygıt bağlantı durumunu izlemek için Event Grid'deki [ **aygıt bağlantısı kesilmiş** olaya](iot-hub-event-grid.md#event-types) abone olmanızı öneririz. IoT Hub'ından Aygıt Bağlantısı ve Aygıt Bağlantısı Kesilen olayları IoT çözümünüze nasıl entegre acağınızı öğrenmek için bu [öğreticiyi](iot-hub-how-to-order-connection-state-events.md) kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[Azure IoT Hub durumunu izleyin ve sorunları hızla tanılayın](iot-hub-monitor-resource-health.md)
