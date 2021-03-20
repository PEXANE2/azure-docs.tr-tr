---
title: include dosyası
description: include dosyası
services: iot-central
author: dominicbetts
ms.service: iot-central
ms.topic: include
ms.date: 11/03/2020
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 2b7881e7fa1ccbcec1325b2af0a570c86b0585a8
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96017528"
---
Bu öğreticide daha sonra örnek cihaz uygulamasını çalıştırdığınızda aşağıdaki yapılandırma değerleri gerekir:

* KIMLIK kapsamı: IoT Central uygulamanızda **yönetim > cihaz bağlantısı**' na gidin. **Kimlik kapsamı** değerini bir yere getirin.
* Birincil anahtarı grupla: IoT Central uygulamanızda **yönetim > cihaz bağlantısı > SAS-IoT-cihazlar**' a gidin. Paylaşılan erişim imzası **birincil anahtar** değerini bir yere getirin.

Yeni aldığınız grup SAS anahtarından bir cihaz anahtarı oluşturmak için Cloud Shell kullanın:

```azurecli-interactive
az extension add --name azure-iot
az iot central device compute-device-key  --device-id sample-device-01 --pk <the group SAS primary key value>
```

Oluşturulan Cihaz anahtarını bir yere getirin, bu öğreticide daha sonra kullanın.
