---
title: Azure IoT Hub sorunlarını giderme hatası 403006 Devicemaximumactivefileuploadlimitexceıbaşında
description: 403006 Devicemaximumactivefileuploadlimitexceıda hatasını nasıl düzelteceğinizi öğrenin
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 1e3c05e4cc3ccf34573b55d3729aded16e26d66e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76960847"
---
# <a name="403006-devicemaximumactivefileuploadlimitexceeded"></a>403006 DeviceMaximumActiveFileUploadLimitExceeded

Bu makalede, **403006 Devicemaximumactivefileuploadlimitexceıo** hatalarına yönelik nedenler ve çözümler açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

Dosya yükleme isteğiniz **403006** hata koduyla başarısız oluyor ve "etkin dosya yükleme isteklerinin sayısı 10 ' dan fazla olamaz.

## <a name="cause"></a>Nedeni

Her cihaz istemcisi, [10 eşzamanlı dosya yükleme](./iot-hub-devguide-quotas-throttling.md#other-limits)ile sınırlıdır. 

Dosya karşıya yükleme tamamlandığında cihazınız IoT Hub bildirimde yoksa, sınırı kolayca aşabilirsiniz. Bu sorun genellikle güvenilmez bir cihaz tarafı ağından kaynaklanır.

## <a name="solution"></a>Çözüm

Cihazın [karşıya dosya yükleme tamamlamayı IoT Hub derhal bilgilendirdiğinden](./iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload)emin olun. Daha sonra, [dosya yükleme yapılandırması IÇIN SAS BELIRTECI TTL 'sini azaltmayı](iot-hub-configure-file-upload.md)deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Karşıya dosya yükleme hakkında daha fazla bilgi edinmek için bkz. [IoT Hub dosyaları karşıya yükleme](./iot-hub-devguide-file-upload.md) ve [Azure Portal kullanarak IoT Hub dosya yüklemelerini yapılandırma](./iot-hub-configure-file-upload.md).