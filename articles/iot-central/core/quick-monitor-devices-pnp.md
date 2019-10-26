---
title: Azure IoT Central'da cihazlarınızı izleme | Microsoft Docs
description: Bir operatör olarak, Azure IoT Central uygulamanızı cihazlarınızı izlemek için kullanın.
author: dominicbetts
ms.author: dobett
ms.date: 10/24/2019
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: db229a6c6d8fded8dc2434653f34ec0c727cc761
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72958080"
---
# <a name="quickstart-use-azure-iot-central-to-monitor-your-devices-preview-features"></a>Hızlı başlangıç: cihazlarınızı izlemek için Azure IoT Central kullanma (Önizleme özellikleri)

*Bu makale, işleçler, oluşturucular ve yöneticiler için geçerlidir.*

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Bu hızlı başlangıçta, cihazlarınızı izlemek ve ayarları değiştirmek için Microsoft Azure IoT Central uygulamanızı nasıl kullanacağınızı bir operatör olarak gösterilir.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce, önceki üç hızlı başlangıç [Oluştur Azure IoT Central uygulaması oluşturma](./quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json), [IoT Central uygulamanıza sanal bir cihaz ekleme](./quick-create-pnp-device-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) ve [cihazınız için kuralları ve eylemleri yapılandırma işlemleri](quick-configure-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)gerçekleştirmeniz gerekir.

## <a name="receive-a-notification"></a>Bildirim alma

Azure IoT Central, e-posta iletileri halinde cihazlara ilişkin bildirimler gönderir. Oluşturucu, bağlı bir ortam algılayıcısı cihazındaki sıcaklık bir eşiği aştığında bildirim göndermek için bir kural ekledi. Oluşturucunun bildirim almayı seçtiği hesaba gönderilen e-postaları kontrol edin.

Cihaz hızlı başlangıç [için kuralları yapılandırma ve eylemlerin](quick-configure-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) sonunda aldığınız e-posta iletisini açın. E-postada, cihazın bağlantısını seçin:

![Uyarı bildirimi e-postası](media/quick-monitor-devices-pnp/email.png)

Önceki hızlı başlangıçlarda oluşturduğunuz ortam algılayıcısı sanal cihazının **Pano** görünümü tarayıcınızda açılır:

![Bildirim e-posta iletisini tetikleyen cihaz](media/quick-monitor-devices-pnp/dashboard.png)

## <a name="investigate-an-issue"></a>Sorun araştırma

Bir operatör olarak, **genel bakış**, **ortam algılayıcısı özellikleri**ve **komut** sayfalarında cihaz hakkındaki bilgileri görüntüleyebilirsiniz. Oluşturucu, bağlı bir ortam algılayıcısı aygıtı hakkında önemli bilgileri göstermek için **Pano** ve **ortam algılayıcısı Özellikler** sayfalarını özelleştirdi.

Cihaz hakkındaki bilgileri görmek için **genel bakış** görünümünü seçin.

Panodaki grafik, cihaz sıcaklığının bir çizimini gösterir. Cihaz sıcaklığının çok yüksek olduğuna karar verirsiniz.

## <a name="remediate-an-issue"></a>Sorunu düzeltme

Cihazda bir değişiklik yapmak için **ortam algılayıcısı Özellikler** sayfasını kullanın.

**Çevresel algılayıcı özelliklerini**seçin. **Parlaklık düzeyini** 10 ' a değiştirin. Cihazı güncelleştirmek için **Kaydet** ' i seçin. Cihaz, ayarlar değişikliğini onayladığında, özelliğin durumu **eşitlenmiş**olarak değişir:

![Ayarları güncelleştirme](media/quick-monitor-devices-pnp/change-settings.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta şunları öğrendiniz:

* Bildirim alma
* Sorun araştırma
* Sorunu düzeltme

Artık cihazınızı izlemeyi bildiğinize göre, önerilen sonraki adım şunları yapmanız gerekir:

> [!div class="nextstepaction"]
> [Bir cihaz şablonu oluşturun ve yönetin](howto-set-up-template-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).
