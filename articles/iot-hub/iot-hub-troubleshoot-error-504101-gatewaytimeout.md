---
title: Azure IoT Hub sorunlarını giderme hata 504101 GatewayTimeout
description: 504101 GatewayTimeout hatasını nasıl düzelteceğinizi öğrenin
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.custom: amqp
ms.openlocfilehash: 373acc30ed652a7f540e840dfad5eeeda65ca179
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81759560"
---
# <a name="504101-gatewaytimeout"></a>504101 GatewayTimeout

Bu makalede, **504101 GatewayTimeout** hatalarının nedenleri ve çözümleri açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

IoT Hub bir cihaza doğrudan bir yöntemi çağırmaya çalışırken, istek **504101 GatewayTimeout**hatasıyla başarısız olur.

## <a name="cause"></a>Nedeni

### <a name="cause-1"></a>Neden 1

IoT Hub bir hatayla karşılaştı ve zaman aşımından önce doğrudan yöntemin tamamlanıp tamamlanmadığını doğrulayamadık.

### <a name="cause-2"></a>Neden 2

Azure IoT C# SDK 'SıNıN (<1.19.0) önceki bir sürümünü kullanırken, bir hata nedeniyle cihaz ve IoT Hub arasındaki AMQP bağlantısı sessizce bırakılabilir.

## <a name="solution"></a>Çözüm

### <a name="solution-1"></a>Çözüm 1

Yeniden deneme sorunu.

### <a name="solution-2"></a>Çözüm 2

Azure ıOT C# SDK 'SıNıN en son sürümüne yükseltin.