---
title: Sorun Giderme Azure IoT Hub hatası 409001 DeviceAlreadyExists
description: Hata 409001 DeviceAlreadyExists nasıl düzeltilir anlayın
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 93ab2ecc8e820c461a7c79082ac1d50c24f0ba8b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76960795"
---
# <a name="409001-devicealreadyexists"></a>409001 DeviceAlreadyExists

Bu **makalede, 409001 DeviceAlreadyExists** hatalarının nedenleri ve çözümleri açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

IoT Hub'da bir aygıtı kaydetmeye çalışırken, istek **hata 409001 DeviceAlreadyExists**ile başarısız olur.

## <a name="cause"></a>Nedeni

IoT hub'ında zaten aynı aygıt kimliğine sahip bir aygıt var. 

## <a name="solution"></a>Çözüm

Farklı bir aygıt kimliği kullanın ve yeniden deneyin.