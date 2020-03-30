---
title: Sorun Giderme Azure IoT Hub hatası 403006 DeviceMaximumActiveFileUploadLimitExceeded
description: Hata 403006 DeviceMaximumActiveFileUploadLimitExceeded nasıl düzeltilir anlayın
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 1e3c05e4cc3ccf34573b55d3729aded16e26d66e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76960847"
---
# <a name="403006-devicemaximumactivefileuploadlimitexceeded"></a>403006 DeviceMaximumActiveFileUploadLimitExceeded

Bu **makalede, 403006 DeviceMaximumActiveFileUploadLimitExceeded** hatalarının nedenleri ve çözümleri açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

Dosya yükleme isteğiniz **403006** hata koduyla başarısız olur ve "Etkin dosya yükleme isteklerinin sayısı 10'u geçemez" iletisi ile başarısız olur.

## <a name="cause"></a>Nedeni

Her aygıt istemcisi [10 eşzamanlı dosya yüklemesi](./iot-hub-devguide-quotas-throttling.md#other-limits)ile sınırlıdır. 

Dosya yüklemeleri tamamlandığında aygıtınız IoT Hub'a bildirimde bulunmazsa sınırı kolayca aşabilirsiniz. Bu sorun genellikle güvenilir olmayan bir aygıt yan ağ neden olur.

## <a name="solution"></a>Çözüm

Aygıtın [IoT Hub dosya yükleme tamamlamayı](./iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload)derhal bilgilendirebilmesini sağlayın. Ardından, [dosya yükleme yapılandırması için SAS belirteci TTL'yi azaltmayı](iot-hub-configure-file-upload.md)deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Dosya yüklemeleri hakkında daha fazla bilgi edinmek [için, IoT Hub ile Dosya Yükle](./iot-hub-devguide-file-upload.md) ve Azure [portalını kullanarak IoT Hub dosya yüklemelerini yapılandırın'](./iot-hub-configure-file-upload.md)a bakın.