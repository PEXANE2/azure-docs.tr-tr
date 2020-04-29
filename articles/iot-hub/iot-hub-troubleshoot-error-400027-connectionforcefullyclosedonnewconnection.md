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
ms.openlocfilehash: f4949816f516c6a6b60cfda0602f458256370d40
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76960535"
---
# <a name="400027-connectionforcefullyclosedonnewconnection"></a>400027 ConnectionForcefullyClosedOnNewConnection

Bu makalede, **400027 ConnectionForcefullyClosedOnNewConnection** hatalarına yönelik nedenler ve çözümler açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

Cihazdan buluta ikizi işlemi (okuma veya düzeltme eki bildiren özellikler gibi) veya doğrudan yöntem çağırma, **400027**hata koduyla başarısız olur.

## <a name="cause"></a>Nedeni

Başka bir istemci aynı kimlik bilgilerini kullanarak IoT Hub yeni bir bağlantı oluşturdu, bu nedenle önceki bağlantıyı kapattı IoT Hub. IoT Hub, aynı kimlik bilgileri kümesi kullanılarak birden fazla istemcinin bağlanmasına izin vermez.

## <a name="solution"></a>Çözüm

Her istemcinin kendi kimliğini kullanarak IoT Hub bağlandığından emin olun.