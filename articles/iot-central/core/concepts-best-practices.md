---
title: Azure IoT Central cihaz geliştirme en iyi uygulamaları | Microsoft Docs
description: Bu makalede, Azure IoT Central cihaz bağlantısı için en iyi yöntemler özetlenmektedir
author: dominicbetts
ms.author: dobett
ms.date: 03/03/2021
ms.topic: conceptual
ms.service: iot-central
services: iot-central
ms.custom:
- device-developer
ms.openlocfilehash: e8ae8b0173e53c0a46ded1a2690175e367997c9f
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102054951"
---
# <a name="best-practices-for-device-development"></a>Cihaz geliştirme için en iyi uygulamalar

*Bu makale, cihaz geliştiricileri için geçerlidir.*

Bu öneriler, IoT Central yerleşik olağanüstü durum kurtarma ve otomatik ölçeklendirmenin avantajlarından yararlanmak için cihazların nasıl uygulanacağını gösterir.

Aşağıdaki listede bir cihaz IoT Central bağlandığı zaman üst düzey akış gösterilmektedir:

1. Cihaz sağlamak ve bir cihaz bağlantı dizesi almak için DPS kullanın.

1. IoT Central iç IoT Hub uç noktasına bağlanmak için bağlantı dizesini kullanın. IoT Central uygulamanızdan veri gönderme ve verileri alma.

1. Cihaz bağlantı hataları alırsa, hata türüne bağlı olarak, bağlantıyı yeniden deneyin veya cihazı yeniden sağlayın.

## <a name="use-dps-to-provision-the-device"></a>Cihaz sağlamak için DPS kullanın

Bir cihaz için DPS sağlamak üzere IoT Central uygulamanızdan kapsam KIMLIĞI, kimlik bilgileri ve cihaz KIMLIĞINI kullanın. Kimlik bilgisi türleri hakkında daha fazla bilgi edinmek için bkz. [X. 509.440 grup kaydı](concepts-get-connected.md#x509-group-enrollment) ve [SAS grubu kaydı](concepts-get-connected.md#sas-group-enrollment). Cihaz kimlikleri hakkında daha fazla bilgi için bkz. [cihaz kaydı](concepts-get-connected.md#device-registration).

Başarılı olduğunda, DPS cihazın IoT Central uygulamanıza bağlanmak için kullanabileceği bir bağlantı dizesi döndürür. Sağlama hatalarını gidermek için, bkz. [cihazınızın sağlama durumunu denetleme](troubleshoot-connection.md#check-the-provisioning-status-of-your-device).

Cihaz, daha sonraki bağlantılarda kullanılacak bağlantı dizesini önbelleğe alabilir. Ancak, cihazın [bağlantı başarısızlıklarını işleyecek](#handle-connection-failures)şekilde hazırlanmalıdır.

## <a name="connect-to-iot-central"></a>IoT Central Bağlan

IoT Central iç IoT Hub uç noktasına bağlanmak için bağlantı dizesini kullanın. Bağlantı, IoT Central uygulamanıza telemetri göndermenizi, özellik değerlerini IoT Central uygulamanızla eşitlemeniz ve IoT Central uygulamanız tarafından gönderilen komutlara yanıt vermenizi sağlar.

## <a name="handle-connection-failures"></a>Bağlantı başarısızlıklarını işle

Ölçeklendirme veya olağanüstü durum kurtarma amaçları için IoT Central temel alınan IoT Hub 'ını güncelleştirebilir. Bağlantıyı sürdürmek için, cihaz kodunuz yeni IoT Hub uç noktası ile bağlantı kurarak belirli bağlantı hatalarını işlemelidir.

Cihaz, bağlandığı sırada aşağıdaki hatalardan birini alırsa, yeni bir bağlantı dizesi almak için sağlama adımını DPS ile yinelemelidir. Bu hatalar, cihazın kullandığı bağlantı dizesinin artık geçerli olmadığı anlamına gelir:

- IoT Hub uç noktası ulaşılamaz.
- Süre dolma güvenlik belirteci.
- IoT Hub cihaz devre dışı bırakıldı.

Cihaz, bağlandığı sırada aşağıdaki hatalardan birini alırsa bağlantıyı yeniden denemek için bir geri dönüş stratejisi kullanmalıdır. Bu hatalar, cihazın kullandığı bağlantı dizesinin hala geçerli olduğu, ancak geçici koşulların cihazın bağlanmasını durdurmakta olduğu anlamına gelir:

- İşleç engellenen cihaz.
- Hizmetten iç hata 500.

Cihaz hata kodları hakkında daha fazla bilgi edinmek için bkz. [cihaz bağlantılarında sorun giderme](troubleshoot-connection.md).

## <a name="next-steps"></a>Sonraki adımlar

Bir cihaz geliştiricisiyseniz, önerilen bazı sonraki adımlar şunlardır:

- Eğitim bölümünde SAS belirteçlerinin nasıl kullanılacağını gösteren örnek kodları gözden geçirin [: Azure IoT Central uygulamanızda bir istemci uygulaması oluşturma ve bağlama](tutorial-connect-device.md)
- [IoT Central uygulama için Node.js cihaz SDK 'sını kullanarak X. 509.952 sertifikalarıyla cihazları bağlamayı](how-to-connect-devices-x509.md) öğrenin
- [Azure CLI kullanarak cihaz bağlantısını izlemeyi](./howto-monitor-devices-azure-cli.md) öğrenin
- [Azure IoT Central uygulamanızda yeni bir IoT cihaz türü tanımlama](./howto-set-up-template.md) hakkında bilgi edinin
- [Azure IoT Edge cihazlar ve Azure IoT Central](./concepts-iot-edge.md) hakkında bilgi edinin
