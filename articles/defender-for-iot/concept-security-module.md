---
title: Güvenlik modülü ve cihaz ikları
description: Güvenlik modülü TWINS kavramı ve bunların IoT için Defender 'da nasıl kullanıldığı hakkında bilgi edinin.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2019
ms.author: mlottner
ms.openlocfilehash: feb84a1261e37600c68d8e372162033d4021f564
ms.sourcegitcommit: 27d616319a4f57eb8188d1b9d9d793a14baadbc3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2021
ms.locfileid: "100522929"
---
# <a name="security-module"></a>Güvenlik modülü

Bu makalede, IoT için Defender 'ın cihaz TWINS ve modülleri nasıl kullandığı açıklanmaktadır.

## <a name="device-twins"></a>Cihaz ikikesi

Azure 'da oluşturulan IoT çözümleri için, cihaz ikis, hem cihaz yönetimi hem de süreç otomasyonu 'nda anahtar rol oynar.

IoT için Defender, mevcut IoT cihaz yönetimi Platformunuzla tam tümleştirme sunarak cihaz güvenlik durumunuzu yönetmenize ve mevcut cihaz denetimi yeteneklerini kullanmanıza olanak sağlar. Tümleştirme, IoT Hub ikizi mekanizması kullanılarak elde edilir.

Azure IoT Hub 'de [cihaz](../iot-hub/iot-hub-devguide-device-twins.md) ikseponları kavramı hakkında daha fazla bilgi edinin.

## <a name="security-module-twins"></a>Güvenlik modülü TWINS

IoT için Defender, hizmette bulunan her cihaz için bir güvenlik modülü ikizi tutar.
İkizi güvenlik modülü, çözümünüzde her belirli cihaz için cihaz güvenliğiyle ilgili tüm bilgileri tutar.
Cihaz güvenlik özellikleri, daha güvenli iletişim için ikizi ve daha az kaynak gerektiren güncelleştirmeleri ve bakımı etkinleştirmek üzere özel bir güvenlik modülünde saklanır.

İkizi oluşturmayı, özelleştirmeyi ve yapılandırmayı öğrenmek için bkz. [Güvenlik modülü oluşturma ikizi](quickstart-create-security-twin.md) ve [güvenlik aracılarını yapılandırma](how-to-agent-configuration.md) . IoT Hub modül TWINS kavramı hakkında daha fazla bilgi edinmek için bkz. [Modül TWINS 'ı anlama](../iot-hub/iot-hub-devguide-module-twins.md) .

## <a name="next-steps"></a>Sonraki adımlar

- [IoT için Defender genel bakış](overview.md)
- [Güvenlik aracısı dağıtma](how-to-deploy-agent.md)
- [Güvenlik Aracısı kimlik doğrulama yöntemleri](concept-security-agent-authentication-methods.md)