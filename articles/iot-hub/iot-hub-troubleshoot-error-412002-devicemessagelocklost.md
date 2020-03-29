---
title: Sorun Giderme Azure IoT Hub hatası 412002 DeviceMessageLockLost
description: Hata 412002 DeviceMessageLockLost nasıl düzeltilir anlayın
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 66461b23432a3e8b7ae4ad1fdc078fba9ca05646
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76960769"
---
# <a name="412002-devicemessagelocklost"></a>412002 DeviceMessageLockLost

Bu **makalede, 412002 DeviceMessageLockLost** hatalarının nedenleri ve çözümleri açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

Bir buluttan cihaza ileti göndermeye çalışırken, istek **hata 412002 DeviceMessageLockLost**ile başarısız olur.

## <a name="cause"></a>Nedeni

Bir aygıt kuyruktan buluttan aygıta ileti aldığında (örneğin, kullanma) [`ReceiveAsync()`](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.receiveasync?view=azure-dotnet)ileti, bir dakikalık kilit zaman ayarı için IoT Hub tarafından kilitlenir. Aygıt kilit süresi dolduktan sonra iletiyi tamamlamaya çalışırsa, IoT Hub bu özel durumu atar.

## <a name="solution"></a>Çözüm

IoT Hub bildirimi bir dakikalık kilit zaman süresi içinde alamazsa, iletiyi *Enqueued* durumuna geri ayarlar. Aygıt iletiyi yeniden almayı deneyebilir. Hatanın gelecekte oluşmasını önlemek için, iletiyi aldıktan sonraki bir dakika içinde tamamlamak için aygıt yan mantığını uygulayın. Bu bir dakikalık zaman dilimi değiştirilemez.