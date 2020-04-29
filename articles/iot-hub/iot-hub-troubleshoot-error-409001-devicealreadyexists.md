---
title: Azure IoT Hub sorun giderme hatası 409001 DeviceAlreadyExists
description: 409001 DeviceAlreadyExists hatasını nasıl düzelteceğinizi öğrenin
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 93ab2ecc8e820c461a7c79082ac1d50c24f0ba8b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76960795"
---
# <a name="409001-devicealreadyexists"></a>409001 DeviceAlreadyExists

Bu makalede, **409001 DeviceAlreadyExists** hatalarına yönelik nedenler ve çözümler açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

IoT Hub bir cihazı kaydetmeye çalışırken, istek **409001 DeviceAlreadyExists**hatasıyla başarısız olur.

## <a name="cause"></a>Nedeni

IoT Hub 'ında aynı cihaz KIMLIĞINE sahip bir cihaz zaten var. 

## <a name="solution"></a>Çözüm

Farklı bir cihaz KIMLIĞI kullanıp yeniden deneyin.