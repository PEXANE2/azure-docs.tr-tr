---
title: Azure IoT Central cihazlarınızı izleyin | Microsoft Docs
description: Bir operatör olarak, cihazlarınızı izlemek için Azure IoT Central uygulamanızı kullanın.
author: dominicbetts
ms.author: dobett
ms.date: 06/09/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: f68bc211be3cffb61b3381390ae2eeacaffa4213
ms.sourcegitcommit: 4d177e6d273bba8af03a00e8bb9fe51a447196d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71960417"
---
# <a name="tutorial-use-azure-iot-central-to-monitor-your-devices"></a>Öğretici: cihazlarınızı izlemek için Azure IoT Central kullanma

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

Bu öğreticide, bir operatör olarak, cihazlarınızı izlemek ve ayarları değiştirmek için Microsoft Azure IoT Central uygulamanızı nasıl kullanacağınız gösterilmektedir.

Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:

> [!div class="checklist"]
> * Bildirim alın
> * Bir sorunu araştırın
> * Bir sorunu düzeltin

## <a name="prerequisites"></a>Prerequisites

Başlamadan önce, oluşturucunun Azure IoT Central uygulamasını oluşturmak için üç Oluşturucu öğreticilerini tamamlaması gerekir:

* [Yeni bir cihaz türü tanımlayın](tutorial-define-device-type.md)
* [Cihazınız için kuralları ve eylemleri yapılandırma](tutorial-configure-rules.md)
* [İşlecin görünümlerini özelleştirme](tutorial-customize-operator.md)

## <a name="receive-a-notification"></a>Bildirim alın

Azure IoT Central, cihazlar hakkında e-posta iletileri olarak bildirimler gönderir. Oluşturucu, bağlı bir AIR klimaları cihazındaki sıcaklık bir eşiği aştığında bildirim göndermek için bir kural ekledi. Oluşturucunun bildirimleri almak için seçtiği hesaba gönderilen e-postaları denetleyin.

[Cihaz öğreticinizi yapılandırma kuralları ve eylemlerinin](tutorial-configure-rules.md) sonunda aldığınız e-posta iletisini açın. E-postada, **Ayrıntılar** bölümünde **Cihaz adı** ' nın yanındaki cihaza bağlantıyı seçin:

![Uyarı bildirimi e-postası](media/tutorial-monitor-devices/email.png)

Önceki öğreticilerde oluşturduğunuz sanal cihaz **1** ' in **cihaz** sayfası tarayıcınızda açılır:

![Bildirim e-posta iletisini tetikleyen cihaz](media/tutorial-monitor-devices/sourcedevice.png)

## <a name="investigate-an-issue"></a>Bir sorunu araştırın

Bir operatör olarak **ölçümler**, **Ayarlar**, **Özellikler**, **kurallar**ve **Pano** sayfalarında cihaz hakkındaki bilgileri görüntüleyebilirsiniz. Oluşturucu, bağlantılı bir AIR klimaları cihazı hakkında önemli bilgileri göstermek için **panoyu** özelleştirdi.

Cihaz hakkındaki bilgileri görmek için **Pano** görünümünü seçin.

![Cihaz panosu](media/tutorial-monitor-devices/initial_screen.png)

Panodaki grafik, cihaz sıcaklığının bir grafiğini gösterir. Cihaz **özellikleri** kutucuğunda cihaz için geçerli hedef sıcaklığını da görebilirsiniz. Hedef sıcaklığın çok yüksek olduğuna karar verirsiniz.

## <a name="remediate-an-issue"></a>Bir sorunu düzeltin

Cihazın hedef sıcaklığını değiştirmek için **Ayarlar** sayfasını kullanın:

1. **Ayarlar**' ı seçin. **Küme sıcaklığını** 75 olarak değiştirin. Yeni hedef sıcaklığını cihaza göndermek için **Güncelleştir** ' i seçin. Cihaz, ayarlar değişikliğini onayladığında, ayarın durumu **eşitlenmiş**olarak değişir:

    ![Güncelleştirme ayarları](media/tutorial-monitor-devices/change_settings.png)

2. **Pano** ' yı seçin ve yeni ayar değerini doğrulayın:

    ![Güncelleştirilmiş cihaz panosu](media/tutorial-monitor-devices/new_settings.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, nasıl yapılacağını öğrendiniz:

> [!div class="nextstepaction"]
> * Bildirim alın
> * Bir sorunu araştırın
> * Bir sorunu düzeltin

Artık cihazınızı izleyeceğinizi bildiğinize göre, önerilen sonraki adım [bir cihaz eklemektir](tutorial-add-device.md).