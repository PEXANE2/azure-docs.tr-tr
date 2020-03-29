---
title: Sorun Giderme Azure IoT Hub hatası 400027 ConnectionForcefullyClosedOnNewConnection
description: Hata 400027 ConnectionForcefullyClosedOnNewConnection nasıl düzeltilir anlayın
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: f4949816f516c6a6b60cfda0602f458256370d40
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76960535"
---
# <a name="400027-connectionforcefullyclosedonnewconnection"></a>400027 ConnectionForcefullyClosedOnNewConnection

Bu makalede, **400027 ConnectionForcefullyClosedOnNewConnection** hatalarının nedenleri ve çözümleri açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

Cihazdan buluta ikiz işleminiz (okuma veya yama bildirilen özellikler gibi) veya doğrudan yöntem çağırma hata kodu **400027**ile başarısız olur.

## <a name="cause"></a>Nedeni

Başka bir istemci aynı kimlik bilgilerini kullanarak IoT Hub'a yeni bir bağlantı oluşturdu, bu nedenle IoT Hub önceki bağlantıyı kapattı. IoT Hub, aynı kimlik bilgilerini kullanarak birden fazla istemcinin bağlanmasına izin vermez.

## <a name="solution"></a>Çözüm

Her istemcinin kendi kimliğini kullanarak IoT Hub'a bağlandığından emin olun.