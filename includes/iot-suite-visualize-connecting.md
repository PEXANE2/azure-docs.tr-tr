---
title: include dosyası
description: include dosyası
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 04/24/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 9b9e28f18208674609d0842b0e3a54e3fc661c9f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188805"
---
## <a name="view-device-telemetry"></a>Cihaz telemetrisini görüntüleme

Cihazınızdan gönderilen telemetriyi aygıt **explorer** sayfasında niçin çözümde görüntüleyebilirsiniz.

1. **Aygıt Gezgini** sayfasındaki aygıtlar listesinde sizin hükmettiğiniz aygıtı seçin. Bir panel, aygıt telemetrisinin çizimi de dahil olmak üzere cihazınızla ilgili bilgileri görüntüler:

    ![Aygıt ayrıntılarını görün](media/iot-suite-visualize-connecting/devicesdetail.png)

1. Telemetri ekranını değiştirmek için **Basınç'ı** seçin:

    ![Basınç telemetrisini görüntüle](media/iot-suite-visualize-connecting/devicespressure.png)

1. Cihazınızla ilgili tanılama bilgilerini görüntülemek için **Tanılama'ya**gidin:

    ![Cihaz tanılama bilgilerini görüntüleme](media/iot-suite-visualize-connecting/devicesdiagnostics.png)

## <a name="act-on-your-device"></a>Cihazınızda hareket edin

Aygıtlarınızda yöntemleri çağırmak için Uzaktan İzleme çözümündeki **Aygıt Gezgini** sayfasını kullanın. Örneğin, Uzaktan İzleme çözümünde **Chiller** aygıtları yeniden **başlatma** yöntemi uygular.

1. Çözümdeki **Aygıt Gezgini** sayfasına gitmek için **Aygıtlar'ı** seçin.

1. **Aygıt Gezgini** sayfasındaki aygıtlar listesinde sizin hükmettiğiniz aygıtı seçin:

    ![Gerçek cihazınızı seçin](media/iot-suite-visualize-connecting/devicesselect.png)

1. Cihazınızda arayabilirsiniz yöntemlerin listesini görüntülemek **için, İşler,** sonra **Yöntemleri**seçin. Birden çok aygıtta çalışacak bir iş zamanlamak için, listede birden çok aygıt seçebilirsiniz. **İşler** paneli seçtiğiniz tüm aygıtlar için ortak yöntem türlerini gösterir.

1. **Yeniden Başlat'ı**seçin, iş adını **RebootPhysicalChiller** olarak ayarlayın ve sonra **Uygula'yı**seçin:

    ![Firmware güncelleştirmesini zamanlama](media/iot-suite-visualize-connecting/deviceschedule.png)

1. Simüle edilen aygıt yöntemi işlerken konsolda aygıt kodunuzu çalıştıran bir dizi ileti görüntülenir.

> [!NOTE]
> Çözümdeki işin durumunu izlemek için **İş Durumunu Görüntüle'yi**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Makale, [Çözüm Hızlandırıcısını Özelleştirmek](../articles/iot-accelerators/iot-accelerators-remote-monitoring-customize.md) için çözüm hızlandırıcısını özelleştirmenin bazı yollarını açıklar.