---
author: wesmc7777
ms.author: wesmc
ms.service: iot-hub
ms.topic: include
ms.date: 08/20/2019
ms.openlocfilehash: c412f7f3e20e4d04083e457bfb245b850b65e126
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "70050445"
---
> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
> * [.NET](../articles/iot-hub/iot-hub-csharp-csharp-twin-getstarted.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-twin-getstarted.md)
> * [Python](../articles/iot-hub/iot-hub-python-twin-getstarted.md)

Cihaz ikizleri; meta veriler, yapılandırmalar ve koşullar gibi cihaz durumu bilgilerini depolayan JSON belgelerdir. IoT Hub, bağlanan her aygıt için bir aygıt ikizini kalıcı hale getirir.

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Aygıt ikizleri kullanarak şunları kullanın:

* Çözüm arka uçunuzdan aygıt meta verilerini depolayın.

* Aygıt uygulamanızdan kullanılabilir özellikler ve koşullar (örneğin, kullanılan bağlantı yöntemi gibi geçerli durum bilgilerini) bildirin.

* Bir aygıt uygulaması ve arka uç uygulaması arasında firmware ve yapılandırma güncelleştirmeleri gibi uzun süren iş akışlarının durumunu eşitleyin.

* Aygıt meta verilerinizi, yapılandırmanızı veya durumunuzu sorgulayın.

Aygıt ikizleri senkronizasyon ve aygıt yapılandırmalarını ve koşullarını sorgulamak için tasarlanmıştır. Cihaz ikizleri ne zaman kullanılacağı hakkında daha fazla bilgi [understand aygıt ikizler](../articles/iot-hub/iot-hub-devguide-device-twins.md)bulunabilir.

Aygıt ikizleri bir IoT hub'ında saklanır ve aşağıdaki öğeleri içerir:

* **Etiketler**. Aygıt meta verilerine yalnızca çözüm arka uçtarafından erişilebilir.

* **İstenilen özellikler**. JSON nesneleri çözüm arka uç tarafından değiştirilebilir ve cihaz uygulaması tarafından gözlemlenebilir.

* **Bildirilen özellikler.** JSON nesneleri cihaz uygulaması tarafından değiştirilebilir ve çözüm arka uç tarafından okunabilir.

Etiketler ve özellikler dizileri içeremez, ancak nesneler iç içe olabilir.

Aşağıdaki resimde aygıt ikiz organizasyonu gösterilmektedir:

![İşlevselliği gösteren aygıt ikiz görüntüsü](./media/iot-hub-selector-twin-get-started/twin.png)

Ayrıca, çözüm arka uç yukarıdaki tüm verilere dayalı aygıt ikiz sorgulayabilirsiniz.
Aygıt ikizleri hakkında daha fazla bilgi için [bkz.](../articles/iot-hub/iot-hub-devguide-device-twins.md) Sorgulama hakkında daha fazla bilgi için [IoT Hub sorgu dili'ne](../articles/iot-hub/iot-hub-devguide-query-language.md)bakın.


Bu öğretici şunların nasıl yapıldığını gösterir:

* Aygıt ikizine etiket ekleyen bir arka uç uygulaması ve bağlantı kanalını aygıt ikizinde bildirilen bir özellik olarak bildiren simüle edilmiş bir aygıt uygulaması oluşturun.

* Daha önce oluşturulan etiketlerdeki ve özelliklerdeki filtreleri kullanarak arka uç uygulamanızdaki cihazları sorgula.
