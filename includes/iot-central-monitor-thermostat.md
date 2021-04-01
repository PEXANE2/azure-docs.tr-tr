---
title: include dosyası
description: include dosyası
services: iot-central
author: dominicbetts
ms.service: iot-central
ms.topic: include
ms.date: 03/12/2020
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 28f676892967abbd0da63d7a75ea3d164b87ce97
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "96017513"
---
Azure IoT Central uygulamanızda bir operatör olarak şunları yapabilirsiniz:

* **Genel bakış** sayfasında cihaz tarafından gönderilen Telemetriyi görüntüleyin:

    :::image type="content" source="media/iot-central-monitor-thermostat/view-telemetry.png" alt-text="Cihaz telemetrisini görüntüleme":::

* **Hakkında** sayfasında cihaz özelliklerini görüntüleyin:

    :::image type="content" source="media/iot-central-monitor-thermostat/about-properties.png" alt-text="Cihaz özelliklerini görüntüle":::

## <a name="customize-the-device-template"></a>Cihaz şablonunu özelleştirme

Çözüm geliştiricisi olarak, termostat cihazı bağlandığında IoT Central otomatik olarak oluşturulan cihaz şablonunu özelleştirebilirsiniz.

Cihazla ilişkili müşteri adını depolamak üzere bir bulut özelliği eklemek için:

1. IoT Central uygulamanızda, **cihaz şablonları** sayfasında **termostat** cihaz şablonuna gidin.

1. **Termostat** cihaz şablonunda, **bulut özellikleri**' ni seçin.

1. **Bulut özelliği Ekle**' yi seçin. **Görünen ad** olarak *müşteri adı* ' nı girin ve **şema** olarak **dize** ' yi seçin. Sonra **Kaydet**'i seçin.

**Max-Min raporu alma** komutunun IoT Central uygulamanızda nasıl görüntüleneceğini özelleştirmek için, cihaz şablonunda **Özelleştir** ' i seçin. **Max-Min al raporunu değiştirin.** *durumunu Al raporuyla*. Sonra **Kaydet**'i seçin.

**Termostat** modeli **hedef sıcaklık** yazılabilir özelliğini Içerir, cihaz şablonu **müşteri adı** bulutu özelliğini içerir. Bir işlecin bu özellikleri düzenlemek için kullanabileceği bir görünüm oluşturun:

1. **Görünümler** ' i seçin ve ardından **cihazı ve bulut verilerini Düzenle** kutucuğunu seçin.

1. _Özellikleri_ form adı olarak girin.

1. **Hedef sıcaklık** ve **müşteri adı** özelliklerini seçin. Ardından **Bölüm Ekle**' yi seçin.

1. Yaptığınız değişiklikleri kaydedin.

:::image type="content" source="media/iot-central-monitor-thermostat/properties-view.png" alt-text="Özellik değerlerini güncelleştirmek için görüntüleme":::

## <a name="publish-the-device-template"></a>Cihaz şablonunu yayımlama

Bir işlecin yaptığınız özelleştirmeleri görüp kullanabilmesi için önce cihaz şablonunu yayımlamanız gerekir.

**Termostat** cihaz şablonundan **Yayımla**' yı seçin. **Bu cihaz şablonunu uygulama panelinde Yayımla** ' da, **Yayımla**' yı seçin.

Bir işleç artık özellik değerlerini güncelleştirmek için **Özellikler** görünümünü kullanabilir ve cihaz komutları sayfasında **durum raporu al** adlı bir komut çağırabilir:

* **Özellikler** sayfasında yazılabilir özellik değerlerini güncelleştir:

    :::image type="content" source="media/iot-central-monitor-thermostat/update-properties.png" alt-text="Cihaz özelliklerini güncelleştirme":::

* **Komutlar sayfasından komutları** çağırın:

    :::image type="content" source="media/iot-central-monitor-thermostat/call-command.png" alt-text="Komutu çağırın":::

    :::image type="content" source="media/iot-central-monitor-thermostat/command-response.png" alt-text="Komut yanıtını görüntüleme":::
