---
author: wesmc7777
ms.author: wesmc
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: 239ea2eb4d5bc8d326d5ca503a18b149252dc1be
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69558780"
---
> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-device-management-get-started.md)
> * [.NET](../articles/iot-hub/iot-hub-csharp-csharp-device-management-get-started.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-device-management-getstarted.md)
> * [Python](../articles/iot-hub/iot-hub-python-python-device-management-get-started.md)

Arka uç uygulamaları, aygıtlar üzerindeki aygıt yönetimi eylemlerini uzaktan başlatmak ve izlemek için [aygıt ikizi][lnk-devtwin] ve [doğrudan yöntemler][lnk-c2dmethod]gibi Azure IoT Hub ilkellerini kullanabilir. Bu öğretici, bir arka uç uygulaması ve bir aygıt uygulamasının IoT Hub'ı kullanarak uzak bir aygıtıyeniden başlatmayı başlatmak ve izlemek için birlikte nasıl çalışabileceğini gösterir.

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Buluttaki bir arka uç uygulamasından aygıt yönetimi eylemlerini (yeniden başlatma, fabrika sıfırlama ve firmware güncelleştirmesi gibi) başlatmak için doğrudan bir yöntem kullanın. Cihaz aşağıdakilerden sorumludur:

* IoT Hub'dan gönderilen yöntem isteğini işleme.

* Aygıtta ilgili aygıta özgü eylemi başlatma.

* Bildirilen *özellikler* aracılığıyla IoT Hub'a durum güncelleştirmeleri sağlar.

Aygıt yönetimi eylemlerinizin ilerlemesini bildirmek için aygıt ikiz sorgularını çalıştırmak için bulutta bir arka uç uygulaması kullanabilirsiniz.

[lnk-devtwin]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
