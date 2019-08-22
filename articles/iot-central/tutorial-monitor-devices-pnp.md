---
title: Azure IoT Central'da cihazlarınızı izleme | Microsoft Docs
description: Bir operatör olarak, Azure IoT Central uygulamanızı cihazlarınızı izlemek için kullanın.
author: dominicbetts
ms.author: dobett
ms.date: 08/06/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: cf50fd80bdbce5895bd1da39700d1c6e4cdcc230
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69878120"
---
# <a name="tutorial-use-azure-iot-central-to-monitor-your-devices-preview-features"></a>Öğretici: Cihazlarınızı izlemek için Azure IoT Central kullanma (Önizleme özellikleri)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Bu öğreticide, bir operatör olarak cihazlarınızı izlemek ve ayarlarınızı değiştirmek için Microsoft Azure IoT Central uygulamasını nasıl kullanacağınız gösterilmektedir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bildirim alma
> * Sorun araştırma
> * Sorunu düzeltme

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce, oluşturucunun Azure IoT Central uygulamasını oluşturmaya yönelik üç oluşturucu öğreticisini tamamlaması gerekir:

* [Yeni bir cihaz türü belirleme](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
* [Cihaz ekleme](tutorial-add-device-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
* [Cihazınız için kurallar ve eylemler yapılandırma](tutorial-configure-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)

## <a name="receive-a-notification"></a>Bildirim alma

Azure IoT Central, e-posta iletileri halinde cihazlara ilişkin bildirimler gönderir. Oluşturucu, bağlı bir ortam algılayıcısı cihazındaki sıcaklık bir eşiği aştığında bildirim göndermek için bir kural ekledi. Oluşturucunun bildirim almayı seçtiği hesaba gönderilen e-postaları kontrol edin.

[Cihazınız için kurallar ve eylemler yapılandırma](tutorial-configure-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) öğreticisinin sonunda aldığınız e-posta iletisini açın. E-postada, cihazın bağlantısını seçin:

![Uyarı bildirimi e-postası](media/tutorial-monitor-devices-pnp/email.png)

Önceki öğreticilerde oluşturduğunuz ortam algılayıcısı sanal cihazının **Pano** görünümü tarayıcınızda açılır:

![Bildirim e-posta iletisini tetikleyen cihaz](media/tutorial-monitor-devices-pnp/dashboard.png)

## <a name="investigate-an-issue"></a>Sorun araştırma

Bir operatör olarak, **Pano**, **ortam algılayıcısı özellikleri**ve **komut** sayfalarında cihaz hakkındaki bilgileri görüntüleyebilirsiniz. Oluşturucu, bağlı bir ortam algılayıcısı aygıtı hakkında önemli bilgileri göstermek için **Pano** ve **ortam algılayıcısı Özellikler** sayfalarını özelleştirdi.

Cihaza ilişkin bilgileri görmek için **Pano** görünümünü seçin.

Panodaki grafik, cihaz sıcaklığının bir çizimini gösterir. Cihaz **özellikleri** kutucuğunda cihaz için geçerli hedef sıcaklığını da görebilirsiniz. Hedef sıcaklığın çok yüksek olmasına karar verdiniz.

## <a name="remediate-an-issue"></a>Sorunu düzeltme

Cihazda bir değişiklik yapmak için **ortam algılayıcısı Özellikler** sayfasını kullanın:

1. **Çevresel algılayıcı özelliklerini**seçin. **Parlaklık düzeyini** 10 ' a değiştirin. Cihazı güncelleştirmek için **Kaydet** ' i seçin. Cihaz, ayarlar değişikliğini onayladığında, özelliğin durumu **eşitlenmiş**olarak değişir:

    ![Ayarları güncelleştir](media/tutorial-monitor-devices-pnp/change-settings.png)

2. **Pano** ' yı seçin ve yeni özellik değerini doğrulayın:

    ![Güncelleştirilmiş cihaz panosu](media/tutorial-monitor-devices-pnp/new-settings.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

* Bildirim alma
* Sorun araştırma
* Sorunu düzeltme

Artık cihazınızı izlemeyi bildiğinize göre, önerilen sonraki adım şunları yapmanız gerekir:

> [!div class="nextstepaction"]
> [Cihazınız için kurallar ve eylemler yapılandırma](tutorial-configure-rules.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).