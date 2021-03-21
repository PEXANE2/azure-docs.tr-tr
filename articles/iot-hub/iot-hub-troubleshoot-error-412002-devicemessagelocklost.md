---
title: Azure IoT Hub sorunlarını giderme hatası 412002 Devicemessagelockkaybedildi
description: 412002 Devicemessagelockkayıp hatasını nasıl düzelteceğinizi öğrenin
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 53364009f9b9c041c39728e438c3e24eacfd1665
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102435486"
---
# <a name="412002-devicemessagelocklost"></a>412002 DeviceMessageLockLost

Bu makalede, **412002 Devicemessagelockkayıp** hataları için nedenler ve çözümler açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

Buluttan cihaza ileti gönderilmeye çalışırken, istek **412002 Devicemessagelockkaybedildi** hatasıyla başarısız olur.

## <a name="cause"></a>Nedeni

Bir cihaz kuyruktan cihaza bir ileti aldığında (örneğin, kullanarak [`ReceiveAsync()`](/dotnet/api/microsoft.azure.devices.client.deviceclient.receiveasync) ), bir dakikalık kilit zaman aşımı süresi için IoT Hub ileti kilitlenir. Kilit zaman aşımı süresi dolduktan sonra cihaz iletiyi tamamlamaya çalışırsa IoT Hub bu özel durumu oluşturur.

## <a name="solution"></a>Çözüm

IoT Hub bir dakikalık kilit zaman aşımı süresi içinde bildirimi almadıysanız, iletiyi *sıraya alınan* duruma geri ayarlar. Cihaz iletiyi yeniden almayı deneyebilir. Hatanın gelecekte oluşmasını engellemek için, iletiyi alırken bir dakika içinde iletiyi tamamlamaya yönelik cihaz tarafı mantığını uygulayın. Bu bir dakikalık zaman aşımı değiştirilemez.