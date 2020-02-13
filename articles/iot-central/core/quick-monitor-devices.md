---
title: Hızlı başlangıç-Azure IoT Central cihazlarınızı Izleyin
description: Bir operatör olarak, cihazlarınızı izlemek için Azure IoT Central uygulamanızı nasıl kullanacağınızı öğrenin.
author: dominicbetts
ms.author: dobett
ms.date: 02/12/2020
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: e56b733a567c706c2a15f2c30ed93a47c244db11
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77168716"
---
# <a name="quickstart-use-azure-iot-central-to-monitor-your-devices"></a>Hızlı başlangıç: cihazlarınızı izlemek için Azure IoT Central kullanma

*Bu makale, işleçler, oluşturucular ve yöneticiler için geçerlidir.*

Bu hızlı başlangıçta, cihazlarınızı izlemek ve ayarları değiştirmek için Microsoft Azure IoT Central uygulamanızı nasıl kullanacağınızı bir operatör olarak gösterilir.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce, önceki üç hızlı başlangıç [Oluştur Azure IoT Central uygulaması oluşturma](./quick-deploy-iot-central.md), [IoT Central uygulamanıza sanal bir cihaz ekleme](./quick-create-pnp-device.md) ve [cihazınız için kuralları ve eylemleri yapılandırma işlemleri](quick-configure-rules.md)gerçekleştirmeniz gerekir.

## <a name="receive-a-notification"></a>Bildirim alma

Azure IoT Central, e-posta iletileri halinde cihazlara ilişkin bildirimler gönderir. Oluşturucu, bağlı bir cihaz algılayıcısındaki sıcaklık bir eşiği aştığında bildirim göndermek için bir kural ekledi. Oluşturucunun bildirim almayı seçtiği hesaba gönderilen e-postaları kontrol edin.

Cihaz hızlı başlangıç [için kuralları yapılandırma ve eylemlerin](quick-configure-rules.md) sonunda aldığınız e-posta iletisini açın. E-postada, cihazın bağlantısını seçin:

![Uyarı bildirimi e-postası](media/quick-monitor-devices/email.png)

Önceki hızlı başlangıçlarda oluşturduğunuz sanal cihaz için **genel bakış** görünümü tarayıcınızda açılır:

![Bildirim e-posta iletisini tetikleyen cihaz](media/quick-monitor-devices/dashboard.png)

## <a name="investigate-an-issue"></a>Sorun araştırma

Bir operatör olarak, **genel bakış**, **hakkında**ve **Komutlar** görünümlerinde cihaz hakkındaki bilgileri görüntüleyebilirsiniz. Oluşturucu cihaz bilgilerini düzenlemeniz ve cihaz özelliklerini ayarlamanız için bir **cihaz yönetme** görünümü oluşturdu.

Panodaki grafik, cihaz sıcaklığının bir çizimini gösterir. Cihaz sıcaklığının çok yüksek olduğuna karar verirsiniz.

## <a name="remediate-an-issue"></a>Sorunu düzeltme

Cihazda bir değişiklik yapmak için **Cihazı yönet** sayfasını kullanın.

Cihaz soğuk olan **fan hızını** 500 olarak değiştirin. Cihazı güncelleştirmek için **Kaydet** ' i seçin. Cihaz, ayarlar değişikliğini onayladığında, özelliğin durumu **eşitlenmiş**olarak değişir:

![Ayarları güncelleştirme](media/quick-monitor-devices/change-settings.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta şunları öğrendiniz:

* Bildirim alma
* Sorun araştırma
* Sorunu düzeltme

Artık cihazınızı izlemeyi bildiğinize göre, önerilen sonraki adım şunları yapmanız gerekir:

> [!div class="nextstepaction"]
> [Bir cihaz şablonu oluşturun ve yönetin](howto-set-up-template.md).
