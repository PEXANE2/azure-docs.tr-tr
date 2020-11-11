---
title: Azure IoT Hub hatası 400027 ConnectionForcefullyClosedOnNewConnection sorunlarını giderme
description: 400027 ConnectionForcefullyClosedOnNewConnection hatasını nasıl düzelteceğinizi öğrenin
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.custom:
- mqtt
- fasttrack-edit
- iot
ms.openlocfilehash: e5d1dc345c72d77be6172fb9c3a10eb2f38d186a
ms.sourcegitcommit: 4bee52a3601b226cfc4e6eac71c1cb3b4b0eafe2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2020
ms.locfileid: "94506337"
---
# <a name="400027-connectionforcefullyclosedonnewconnection"></a>400027 ConnectionForcefullyClosedOnNewConnection

Bu makalede, **400027 ConnectionForcefullyClosedOnNewConnection** hatalarına yönelik nedenler ve çözümler açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

Cihazınız, .NET SDK ve MQTT aktarım türü kullanan **Connectionstatuschangereason** olarak **Communication_Error** bağlantısı kesildi.

Cihazdan buluta ikizi işlemi (okuma veya düzeltme eki bildiren özellikler gibi) veya doğrudan yöntem çağırma, **400027** hata koduyla başarısız olur.

## <a name="cause"></a>Nedeni

Başka bir istemci aynı kimlik bilgilerini kullanarak IoT Hub yeni bir bağlantı oluşturdu, bu nedenle önceki bağlantıyı kapattı IoT Hub. IoT Hub, aynı kimlik bilgileri kümesi kullanılarak birden fazla istemcinin bağlanmasına izin vermez.

## <a name="solution"></a>Çözüm

Her istemcinin kendi kimliğini kullanarak IoT Hub bağlandığından emin olun.