---
title: Sorun Giderme Azure IoT Hub hatası 404001 DeviceNotFound
description: Hata 404001 DeviceNotFound nasıl düzeltilir anlayın
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 15aa21c2ec2c11bb251f7208fd22c92ceb859d6d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76960834"
---
# <a name="404001-devicenotfound"></a>404001 DeviceNotFound

Bu makalede, **404001 DeviceNotFound** hatalarının nedenleri ve çözümleri açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

C2D iletisi, ikiz güncelleme veya doğrudan yöntem gibi bir bulut-to-aygıt (C2D) iletişim sırasında, işlem hata **404001 DeviceNotFound**ile başarısız olur.

## <a name="cause"></a>Nedeni

Aygıt IoT Hub tarafından bulunamadığı için işlem başarısız oldu. Aygıt kayıtlı veya devre dışı bırakılmış.

## <a name="solution"></a>Çözüm

Kullandığınız aygıt kimliğini kaydedin ve yeniden deneyin.