---
title: 'Tutorial: Monitor your devices in Azure IoT Central'
description: 'Tutorial: As an operator, use your Azure IoT Central application to monitor your devices.'
author: dominicbetts
ms.author: dobett
ms.date: 11/13/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: a07a822e863c5ff5d695a20c02bd8d295721efda
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74480247"
---
# <a name="tutorial-use-azure-iot-central-to-monitor-your-devices"></a>Öğretici: Azure IoT Central kullanarak cihazlarınızı izleme

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

Bu öğreticide, bir operatör olarak cihazlarınızı izlemek ve ayarlarınızı değiştirmek için Microsoft Azure IoT Central uygulamasını nasıl kullanacağınız gösterilmektedir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bildirim alma
> * Sorun araştırma
> * Sorunu düzeltme

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce, oluşturucunun Azure IoT Central uygulamasını oluşturmaya yönelik üç oluşturucu öğreticisini tamamlaması gerekir:

* [Yeni bir cihaz türü belirleme](tutorial-define-device-type.md)
* [Cihazınız için kurallar ve eylemler yapılandırma](tutorial-configure-rules.md)
* [Operatörün görünümlerini özelleştirme](tutorial-customize-operator.md)

## <a name="receive-a-notification"></a>Bildirim alma

Azure IoT Central, e-posta iletileri halinde cihazlara ilişkin bildirimler gönderir. Oluşturucu, bağlı bir klima cihazındaki sıcaklık belirli bir eşiği aştığında bildirim gönderecek şekilde bir kural eklemiştir. Oluşturucunun bildirim almayı seçtiği hesaba gönderilen e-postaları kontrol edin.

[Cihazınız için kurallar ve eylemler yapılandırma](tutorial-configure-rules.md) öğreticisinin sonunda aldığınız e-posta iletisini açın. In the email, select the link to the device next to **Device Name** in the **Details** section:

![Alert notification email](media/tutorial-monitor-devices/email.png)

Önceki öğreticilerde oluşturduğunuz **Bağlı Klima-1** sanal cihazının **Cihaz** sayfası tarayıcıda açılır:

![Bildirim e-posta iletisini tetikleyen cihaz](media/tutorial-monitor-devices/sourcedevice.png)

## <a name="investigate-an-issue"></a>Sorun araştırma

Operatör olarak, bir cihaza ilişkin bilgileri **Ölçümler**, **Ayarlar**, **Özellikler**, **Kurallar** ve **Pano** sayfalarında görüntüleyebilirsiniz. Oluşturucu, **Pano**’yu bağlı bir klima cihazına ilişkin önemli bilgileri gösterecek şekilde özelleştirmiştir.

Cihaza ilişkin bilgileri görmek için **Pano** görünümünü seçin.

![Cihaz panosu](media/tutorial-monitor-devices/initial_screen.png)

Panodaki grafik, cihaz sıcaklığının bir çizimini gösterir. You can also see the current target temperature for the device in the **Device properties** tile. Hedef sıcaklığın çok yüksek olmasına karar verdiniz.

## <a name="remediate-an-issue"></a>Sorunu düzeltme

Cihazın hedef sıcaklığını değiştirmek için **Ayarlar** sayfasını kullanın:

1. **Ayarlar**'ı seçin. **Sıcaklığı Ayarla**’yı 75 olarak değiştirin. Yeni hedef sıcaklığı cihaza göndermek için **Güncelleştir**’i seçin. When the device confirms the settings change, the status of the setting changes to **synced**:

    ![Ayarları güncelleştirme](media/tutorial-monitor-devices/change_settings.png)

2. **Pano**’yu seçin ve yeni ayar değerini doğrulayın:

    ![Güncelleştirilmiş cihaz panosu](media/tutorial-monitor-devices/new_settings.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="nextstepaction"]
> * Bildirim alma
> * Sorun araştırma
> * Sorunu düzeltme

Artık cihazınızı nasıl izleyeceğinizi bildiğinize göre [Cihaz ekleme](tutorial-add-device.md) adımını uygulamanız önerilir.
