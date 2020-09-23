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
ms.openlocfilehash: a5382313c837482f116f498f3a05c36447062b0a
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90942198"
---
# <a name="security-module"></a>Güvenlik modülü

Bu makalede, IoT için Defender 'ın cihaz TWINS ve modülleri nasıl kullandığı açıklanmaktadır.

## <a name="device-twins"></a>Cihaz ikikesi

Azure 'da oluşturulan IoT çözümleri için, cihaz ikis, hem cihaz yönetimi hem de süreç otomasyonu 'nda anahtar rol oynar.

IoT için Defender, mevcut IoT cihaz yönetimi Platformunuzla tam tümleştirme sunarak cihaz güvenlik durumunuzu yönetmenize ve mevcut cihaz denetimi yeteneklerini kullanmanıza olanak sağlar. Tümleştirme, IoT Hub ikizi mekanizması kullanılarak elde edilir.

Azure IoT Hub 'de [cihaz](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins) ikseponları kavramı hakkında daha fazla bilgi edinin.

## <a name="security-module-twins"></a>Güvenlik modülü TWINS

IoT için Defender, hizmette bulunan her cihaz için bir güvenlik modülü ikizi tutar.
İkizi güvenlik modülü, çözümünüzde her belirli cihaz için cihaz güvenliğiyle ilgili tüm bilgileri tutar.
Cihaz güvenlik özellikleri, daha güvenli iletişim için ikizi ve daha az kaynak gerektiren güncelleştirmeleri ve bakımı etkinleştirmek üzere özel bir güvenlik modülünde saklanır.

İkizi oluşturmayı, özelleştirmeyi ve yapılandırmayı öğrenmek için bkz. [Güvenlik modülü oluşturma ikizi](quickstart-create-security-twin.md) ve [güvenlik aracılarını yapılandırma](how-to-agent-configuration.md) . IoT Hub modül TWINS kavramı hakkında daha fazla bilgi edinmek için bkz. [Modül TWINS 'ı anlama](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-module-twins) .

## <a name="see-also"></a>Ayrıca bkz.

- [IoT için Defender genel bakış](overview.md)
- [Güvenlik aracısı dağıtma](how-to-deploy-agent.md)
- [Güvenlik Aracısı kimlik doğrulama yöntemleri](concept-security-agent-authentication-methods.md)
