---
author: wesmc7777
ms.author: wesmc
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: 60c0c700dde5afda2ca93a92b334cbc4f0134043
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68667969"
---
> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-device-management-get-started.md)
> * [.NET](../articles/iot-hub/iot-hub-csharp-csharp-device-management-get-started.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-device-management-getstarted.md)
> * [Python](../articles/iot-hub/iot-hub-python-python-device-management-get-started.md)

Arka uç uygulamalar cihaz [ikizi][lnk-devtwin] ve [doğrudan yöntemler][lnk-c2dmethod]gibi Azure IoT Hub temel öğelerini kullanarak cihazlarda cihaz yönetimi eylemlerini uzaktan başlatabilir ve izleyebilir. Bu öğreticide, bir arka uç uygulamasının ve bir cihaz uygulamasının IoT Hub kullanarak uzak cihaz yeniden başlatma işlemini başlatmak ve izlemek için birlikte nasıl çalıştığı gösterilmektedir.

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Bulut içindeki bir arka uç uygulamasından cihaz yönetim eylemleri (yeniden başlatma, fabrika sıfırlaması ve bellenim güncelleştirmesi gibi) başlatmak için doğrudan bir yöntem kullanın. Cihazın sorumludur:

* IoT Hub gönderilen Yöntem isteği işleniyor.
* Cihazda cihaza özgü ilgili eylem başlatılıyor.
* *Bildirilen özellikler* aracılığıyla IoT Hub durum güncelleştirmeleri sağlama.

Cihaz yönetimi eylemlerinizin ilerlemesini raporlamak üzere cihaz ikizi sorgularını çalıştırmak için bulutta bir arka uç uygulaması kullanabilirsiniz.

[lnk-devtwin]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
