---
title: Azure IoT Hub sorunlarını giderme hatası 404103 DeviceNotOnline
description: 404103 DeviceNotOnline hatasını nasıl düzelteceğinizi öğrenin
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: e648428f924cfc33421c8591c41f7ac85b05a033
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "76960821"
---
# <a name="404103-devicenotonline"></a>404103 DeviceNotOnline

Bu makalede **404103 DeviceNotOnline** hatalarına yönelik nedenler ve çözümler açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

Cihaz çevrimiçi olsa bile bir cihaza doğrudan yöntem **404103 DeviceNotOnline** hatasıyla başarısız olur. 

## <a name="cause"></a>Nedeni

Cihazın çevrimiçi olduğunu ve yine de hatayı almaya başladıysanız, bunun nedeni, doğrudan yöntem geri çağrısının cihazda kayıtlı olmaması olabilir.

## <a name="solution"></a>Çözüm

Cihazınızı doğrudan yöntem geri çağırmaları için doğru şekilde yapılandırmak için, bkz. [bir cihazda doğrudan yöntem işleme](iot-hub-devguide-direct-methods.md#handle-a-direct-method-on-a-device).