---
author: wesmc7777
ms.author: wesmc
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: eef073f5f4d1eb39fd5ccd8dafacd7074158fa37
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68667837"
---
> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
> * [.NET](../articles/iot-hub/iot-hub-csharp-csharp-twin-getstarted.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-twin-getstarted.md)
> * [Python](../articles/iot-hub/iot-hub-python-twin-getstarted.md)

Cihaz çiftleri, cihaz durumu bilgilerini (meta veriler, yapılandırmalar ve koşullar) depolayan JSON belgelerdir. IoT Hub, kendisine bağlanan her cihaz için bir cihaz ikizi devam ettirir.

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Cihaz ikizlerini şu şekilde kullan:

* Çözüm arka ucundan cihaz meta verilerini depolayın.

* Kullanılabilir yetenekler ve koşullar (örneğin, kullanılan bağlantı yöntemi) gibi geçerli durum bilgilerini cihaz uygulamanızdan bildirin.

* Uzun süre çalışan iş akışlarının (bellenim ve yapılandırma güncelleştirmeleri gibi) durumunu bir cihaz uygulaması ile arka uç uygulaması arasında eşitler.

* Cihazınızın meta verilerini, yapılandırmasını veya durumunu sorgulayın.

Cihaz TWINS, eşitleme için ve cihaz yapılandırmalarının ve koşullarının sorgulanması için tasarlanmıştır. Cihaz TWINS 'in ne zaman kullanılacağı hakkında daha fazla bilgi için bkz. cihaz ikizlerini [anlama](../articles/iot-hub/iot-hub-devguide-device-twins.md).

Cihaz ikizleri bir IoT Hub 'ında depolanır ve şunları içerir:

* *Etiketler*, cihaz meta verileri yalnızca çözüm arka ucu tarafından erişilebilir;

* *istenen özellikler*, JSON nesneleri çözüm arka ucu tarafından değiştirilebilir ve cihaz uygulaması tarafından observable; '

* *bildirilen özellikler*, JSON nesneleri cihaz uygulaması tarafından değiştirilebilir ve çözüm arka ucu tarafından okunabilir. Etiketler ve Özellikler diziler içeremez, ancak nesneler iç içe olabilir.

![İşlevselliği gösteren cihaz ikizi resmi](./media/iot-hub-selector-twin-get-started/twin.png)

Ayrıca, çözüm arka ucu, yukarıdaki tüm verileri temel alan cihaz TWINS 'i sorgulayabilir.
Cihaz TWINS hakkında daha fazla bilgi için bkz. [cihaz TWINS](../articles/iot-hub/iot-hub-devguide-device-twins.md) ve sorgulama için [IoT Hub sorgu dili](../articles/iot-hub/iot-hub-devguide-query-language.md) başvurusu.


Bu öğretici şunların nasıl yapıldığını gösterir:

* Bir cihaz ikizi 'e *Etiketler* ekleyen bir arka uç uygulaması oluşturun ve bağlantı kanalını cihazda *bildirilen bir özellik* olarak raporlayan bir sanal cihaz uygulamasına ikizi.

* Daha önce oluşturulan Etiketler ve özelliklerde filtreler kullanarak arka uç uygulamanızdaki cihazları sorgulayın.
