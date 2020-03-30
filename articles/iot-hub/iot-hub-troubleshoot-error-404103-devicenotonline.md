---
title: Sorun Giderme Azure IoT Hub hatası 404103 DeviceNotOnline
description: Hata 404103 DeviceNotOnline nasıl düzeltilir anlayın
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: e648428f924cfc33421c8591c41f7ac85b05a033
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76960821"
---
# <a name="404103-devicenotonline"></a>404103 DeviceNotOnline

Bu makalede, **404103 DeviceNotOnline** hatalarının nedenleri ve çözümleri açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

Aygıta doğrudan bir yöntem, aygıt çevrimiçi olsa bile **404103 DeviceNotOnline** hatasıyla başarısız olur. 

## <a name="cause"></a>Nedeni

Aygıtın çevrimiçi olduğunu biliyorsanız ve yine de hata yıkıyorsanız, bunun nedeni doğrudan yöntem geri aramasının aygıtta kayıtlı olmamasıdır.

## <a name="solution"></a>Çözüm

Doğrudan yöntem geri aramaları için aygıtınızı düzgün yapılandırmak için, [bir aygıtta doğrudan bir yöntemi ele alın'a](iot-hub-devguide-direct-methods.md#handle-a-direct-method-on-a-device)bakın.