---
title: dosya dahil etme
description: dosya dahil etme
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 04/24/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 9b9e28f18208674609d0842b0e3a54e3fc661c9f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188805"
---
## <a name="view-device-telemetry"></a>Cihaz telemetrisini görüntüleme

Cihazınızdaki **Device Explorer** sayfasında, cihazınızdan gönderilen Telemetriyi görüntüleyebilirsiniz.

1. **Device Explorer** sayfasındaki cihaz listesinde sağladığınız cihazı seçin. Bir panel cihaz telemetrinin bir çizimi dahil olmak üzere cihazınız hakkındaki bilgileri görüntüler:

    ![Bkz. cihaz ayrıntısı](media/iot-suite-visualize-connecting/devicesdetail.png)

1. Telemetri ekranını değiştirmek için **basınç** ' ı seçin:

    ![Basınç telemetrisini görüntüle](media/iot-suite-visualize-connecting/devicespressure.png)

1. Aygıtınızla ilgili tanılama bilgilerini görüntülemek için, **tanılamayı**aşağı kaydırın:

    ![Cihaz tanılama bilgilerini görüntüleme](media/iot-suite-visualize-connecting/devicesdiagnostics.png)

## <a name="act-on-your-device"></a>Cihazınızda işlem yapın

Cihazlarınızda yöntemleri çağırmak için, uzaktan Izleme çözümünde **Device Explorer** sayfasını kullanın. Örneğin, uzaktan Izleme çözümünde **Chiller** cihazlarında bir **yeniden başlatma** yöntemi uygular.

1. Çözümdeki **Device Explorer** sayfasına gitmek için **cihazları** seçin.

1. **Device Explorer** sayfasındaki cihaz listesinde sağladığınız cihazı seçin:

    ![Gerçek cihazınızı seçin](media/iot-suite-visualize-connecting/devicesselect.png)

1. Cihazınızda çağırabilmeniz için yöntemler listesini göstermek için **işler**' i ve sonra **Yöntemler**' i seçin. Bir işi birden çok cihazda çalışacak şekilde zamanlamak için, listede birden çok cihaz seçebilirsiniz. **İşler** paneli, seçtiğiniz tüm cihazlarda ortak olan yöntem türlerini gösterir.

1. **Yeniden başlatma**' yı seçin, Iş adını **Rebootphysicalchilolarak** ayarlayın ve **Uygula**' yı seçin:

    ![Üretici yazılımı güncelleştirmesini zamanlama](media/iot-suite-visualize-connecting/deviceschedule.png)

1. Bir dizi ileti, sanal cihaz Yöntemi işlerken cihaz kodunuzu çalıştıran konsolda görüntülenir.

> [!NOTE]
> Çözümdeki işin durumunu izlemek için **Iş durumunu görüntüle**' yi seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Uzaktan izleme çözüm hızlandırıcısının özelleştirilmesi](../articles/iot-accelerators/iot-accelerators-remote-monitoring-customize.md) başlıklı makalede çözüm hızlandırıcıyı özelleştirmenin bazı yolları açıklanmaktadır.