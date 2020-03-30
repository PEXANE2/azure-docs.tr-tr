---
title: Quickstart - Azure IoT Central'da cihazlarınızı izleyin
description: Operatör olarak, cihazlarınızı izlemek için Azure IoT Central uygulamanızı nasıl kullanacağınızı öğrenin.
author: dominicbetts
ms.author: dobett
ms.date: 02/12/2020
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: e56b733a567c706c2a15f2c30ed93a47c244db11
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77168716"
---
# <a name="quickstart-use-azure-iot-central-to-monitor-your-devices"></a>Hızlı başlatma: Cihazlarınızı izlemek için Azure IoT Central'ı kullanın

*Bu makale, işleçler, oluşturucular ve yöneticiler için geçerlidir.*

Bu hızlı başlangıç, bir operatör olarak cihazlarınızı izlemek ve ayarlarınızı değiştirmek için Microsoft Azure IoT Merkezi uygulamanızı nasıl kullanacağınızı gösterir.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, önceki üç hızlı başlangıcı tamamlamanız gereken [bir Azure IoT Merkezi uygulaması oluşturma](./quick-deploy-iot-central.md), [IoT Central uygulamanıza simüle edilmiş bir aygıt ekleyin](./quick-create-pnp-device.md) ve cihazınız için kuralları ve eylemleri [yapılandırın.](quick-configure-rules.md)

## <a name="receive-a-notification"></a>Bildirim alma

Azure IoT Central, e-posta iletileri halinde cihazlara ilişkin bildirimler gönderir. Oluşturucu, bağlı bir aygıt sensöründeki sıcaklık bir eşiği aştığında bildirim göndermek için bir kural ekledi. Oluşturucunun bildirim almayı seçtiği hesaba gönderilen e-postaları kontrol edin.

Aygıtınızın hızlı başlatılması için Yapılandırma [kuralları nın ve eylemlerinin](quick-configure-rules.md) sonunda aldığınız e-posta iletisini açın. E-postada, aygıt bağlantısını seçin:

![Uyarı bildirimi e-postası](media/quick-monitor-devices/email.png)

Önceki hızlı başlangıçlarda oluşturduğunuz simüle edilmiş aygıtın **Genel Bakış** görünümü tarayıcınızda açılır:

![Bildirim e-posta iletisini tetikleyen cihaz](media/quick-monitor-devices/dashboard.png)

## <a name="investigate-an-issue"></a>Sorun araştırma

Bir operatör olarak, aygıt la ilgili bilgileri **Genel Bakış**, **Hakkında**ve **Komutlar görünümlerinde** görüntüleyebilirsiniz. Oluşturucu, aygıt bilgilerini ve aygıt özelliklerini ayarlamanız için bir **Yönet aygıtı** görünümü oluşturdu.

Panodaki grafik, cihaz sıcaklığının bir çizimini gösterir. Cihaz sıcaklığının çok yüksek olduğuna karar verirsiniz.

## <a name="remediate-an-issue"></a>Sorunu düzeltme

Aygıtta değişiklik yapmak için **Aygıtı Yönet** sayfasını kullanın.

Cihazı soğutmak için **Fan Hızını** 500 olarak değiştirin. Aygıtı güncelleştirmek için **Kaydet'i** seçin. Aygıt ayarların değiştiğini onayladığında, özelliğin durumu **eşitlenmiş**olarak değişir:

![Ayarları güncelleştirme](media/quick-monitor-devices/change-settings.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta şunları öğrendiniz:

* Bildirim alma
* Sorun araştırma
* Sorunu düzeltme

Artık cihazınızı izleyeceğinizi bildiğinize göre, önerilen bir sonraki adım şudur:

> [!div class="nextstepaction"]
> [Aygıt şablonu oluşturun ve yönetin.](howto-set-up-template.md)
