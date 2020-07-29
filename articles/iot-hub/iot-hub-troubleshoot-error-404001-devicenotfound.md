---
title: Azure IoT Hub sorunlarını giderme hatası 404001 DeviceNotFound
description: 404001 DeviceNotFound hatasını nasıl düzelteceğinizi öğrenin
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 15aa21c2ec2c11bb251f7208fd22c92ceb859d6d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76960834"
---
# <a name="404001-devicenotfound"></a>404001 DeviceNotFound

Bu makalede, **404001 DeviceNotFound** hatalarına yönelik nedenler ve çözümler açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

C2D Message, ikizi Update veya Direct yöntemi gibi buluttan cihaza (C2D) iletişim sırasında, işlem **404001 DeviceNotFound**hatasıyla başarısız olur.

## <a name="cause"></a>Nedeni

Cihaz IoT Hub tarafından bulunamadığı için işlem başarısız oldu. Cihaz kayıtlı değil veya devre dışı.

## <a name="solution"></a>Çözüm

Kullandığınız cihaz KIMLIĞINI kaydedin, sonra yeniden deneyin.