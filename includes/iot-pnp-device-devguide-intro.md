---
author: dominicbetts
ms.author: dobett
ms.service: iot-pnp
ms.topic: include
ms.date: 09/24/2020
ms.openlocfilehash: 706e936590f5f47ee2989bf77b3caad67a8f28fc
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2020
ms.locfileid: "91580551"
---
IoT Tak ve Kullan, yeteneklerini Azure IoT uygulamalarına duyuran akıllı cihazlar oluşturmanızı sağlar. IoT Tak ve Kullan cihazları, bir müşteri bunları IoT Tak ve Kullan özellikli uygulamalara bağladığında el ile yapılandırma gerektirmez.

Akıllı bir cihaz doğrudan uygulanabilir, [modüller](../articles/iot-hub/iot-hub-devguide-module-twins.md)kullanabilir veya [IoT Edge modüller](../articles/iot-edge/about-iot-edge.md)kullanabilir.

Bu kılavuzda, [ıot Tak ve kullan kurallarını](../articles/iot-pnp/concepts-convention.md)izleyen bir cihaz, modül veya IoT Edge modülü oluşturmak için gereken temel adımlar açıklanmaktadır.

IoT Tak ve Kullan cihazı, modülü veya IoT Edge modülü oluşturmak için aşağıdaki adımları izleyin:

1. Cihazınızın Azure IoT Hub bağlanmak için MQTT veya MQTT Over WebSockets protokolünü kullandığından emin olun.
1. Cihazınızı anlatmak için [dijital bir TWINS tanım dili (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl) modeli oluşturun. Daha fazla bilgi edinmek için bkz. [ıot Tak ve kullan modellerdeki bileşenleri anlama](../articles/iot-pnp/concepts-components.md).
1. Cihaz bağlantısının bir parçası olarak duyurmak için cihazınızı veya modülünüzü güncelleştirin `model-id` .
1. [Iot Tak ve kullan kurallarını](../articles/iot-pnp/concepts-convention.md) kullanarak telemetri, özellik ve komutları uygulama

Cihazınız veya modül uygulamanız hazırsanız, cihazın IoT Tak ve Kullan kurallarını izlediğini doğrulamak için [Azure IoT Gezginini](../articles/iot-pnp/howto-use-iot-explorer.md) kullanın.
