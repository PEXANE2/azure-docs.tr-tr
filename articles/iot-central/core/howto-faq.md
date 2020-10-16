---
title: Azure IoT Central sık sorulan sorular | Microsoft Docs
description: Azure IoT Central sık sorulan sorular (SSS) ve yanıtlar
author: dominicbetts
ms.author: dobett
ms.date: 09/23/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: f9c7412afcc191470902cc256586f9db21f8e78c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91852151"
---
# <a name="frequently-asked-questions-for-iot-central"></a>IoT Central için sık sorulan sorular

**Bir cihaz IoT Central uygulamama bağlanmazsa kimlik bilgisi sorunları olup olmadığını denetleyin Nasıl yaparım??**

[Cihazlarınızdaki verilerin Azure 'da neden gösterilmediğini sorun giderme IoT Central](troubleshoot-connection.md) cihazların bağlantı sorunlarını tanılamaya yönelik adımları içerir.

**Müşteri desteği ile bir bilet dosyası Nasıl yaparım?.**

Yardıma ihtiyacınız varsa bir [Azure destek bileti](https://portal.azure.com/#create/Microsoft.Support)dosyası oluşturabilirsiniz.

Diğer destek seçenekleri de dahil olmak üzere daha fazla bilgi için bkz. [Azure IoT desteği ve yardım seçenekleri](../../iot-fundamentals/iot-support-help.md).

**Cihazın engellemesini kaldırmak Nasıl yaparım??**

Bir cihaz engellendiğinde, IoT Central uygulamanıza veri gönderemeyecektir. Engellenen cihazların, uygulamanızdaki **cihazlar** sayfasında **engellenmiş** durumu vardır. Bir işlecin, veri göndermeye çalışmaya başlamadan önce cihazın engellemesini kaldırmanız gerekir:

:::image type="content" source="media/howto-faq/blocked.png" alt-text="Engellenen cihazı gösteren ekran görüntüsü":::

Bir operatör bir cihazı engellerse, durum önceki değerine döner, **kaydedilir** **veya sağlanır**.

**Nasıl yaparım? bir cihaz onaylansın mı?**

Cihaz durumu **cihazlar** sayfasında **onay bekliyor** ise, **otomatik onaylama** seçeneğinin devre dışı olduğu anlamına gelir:

:::image type="content" source="media/howto-faq/auto-approve.png" alt-text="Engellenen cihazı gösteren ekran görüntüsü":::

Bir işlecin, verileri göndermeye başlamadan önce açıkça bir cihaz onaylaması gerekir. **Cihazlar sayfasında el** ile kayıtlı değil, ancak geçerli kimlik bilgileriyle bağlantılı olarak cihaz durumu **onay bekliyor**olacaktır. İşleçler **onay** düğmesini kullanarak bu cihazları **aygıtlar** sayfasından onaylayabilir:

:::image type="content" source="media/howto-faq/approve-device.png" alt-text="Engellenen cihazı gösteren ekran görüntüsü":::

**Nasıl yaparım? cihazı bir cihaz şablonuyla ilişkilendirsin mi?**

Cihaz durumu **ilişkilendirilmemiş**ise, IoT Central bağlanan cihazın ilişkili bir cihaz şablonu olmadığı anlamına gelir. Bu durum genellikle aşağıdaki senaryolarda meydana gelir:

- Cihaz şablonunu belirtmeden **cihazlar** sayfasında **içeri aktar** kullanılarak bir cihaz kümesi eklenir.
- Cihaz şablonunu belirtmeden **cihazlar** sayfasında el ile bir cihaz kaydedildi. Ardından cihaz geçerli kimlik bilgileriyle bağlanır.  

İşleci, **geçirme** düğmesini kullanarak **cihazları cihazlar** sayfasından bir cihaz şablonuyla ilişkilendirebilir. Daha fazla bilgi edinmek için bkz. [Azure IoT Central uygulamanızdaki cihazları yönetme > cihazları bir şablona geçirme](howto-manage-devices.md).

**IoT Hub hakkında nereden daha fazla bilgi edinebilirim?**

Azure IoT Central, Azure IoT Hub cihaz bağlantısı sağlayan bir bulut ağ geçidi olarak kullanır. IoT Hub şunları sunar:

- Bulutta ölçekte veri alımı.
- Cihaz yönetimi.
- Güvenli cihaz bağlantısı.

IoT Hub hakkında daha fazla bilgi edinmek için bkz. [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/).

**Cihaz sağlama hizmeti (DPS) hakkında nereden daha fazla bilgi edinebilirim?**

Azure IoT Central, cihazların uygulamanıza bağlanmasını sağlamak için DPS kullanır. Cihazların IoT Central 'e bağlanması sırasında DPS 'in oynadığı rol hakkında daha fazla bilgi edinmek için bkz. [Azure 'a bağlanma IoT Central alma](concepts-get-connected.md). DPS hakkında daha fazla bilgi edinmek için bkz. [Azure IoT Hub cihaz sağlama hizmeti ile cihazları sağlama](../../iot-dps/about-iot-dps.md).