---
title: include dosyası
description: include dosyası
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 08/07/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 3386cb51a8a728576f6615002d6154d89ca662c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68883830"
---
<!-- This tells how to get the connection string for the registryReadWrite shared access policy of your IoT hub -->

**Kayıt defteriReadWrite** ilkesi için IoT Hub bağlantı dizesini almak için aşağıdaki adımları izleyin:

1. Azure [portalında](https://portal.azure.com) **Kaynak gruplarını**seçin. Hub'ınızın bulunduğu kaynak grubunu seçin ve ardından kaynak listesinden hub'ınızı seçin.

2. Hub'ınızın sol tarafındaki bölmede **Paylaşılan erişim ilkeleri'ni**seçin.

3. İlkeler listesinden **kayıt defteriReadWrite** ilkesini seçin.

4. **Paylaşılan erişim tuşları**altında, Bağlantı **dizesi** için kopya simgesini seçin -- birincil anahtar ve değeri kaydedin.

    ![Bağlantı dizesinin nasıl alınır göster](./media/iot-hub-include-find-registryrw-connection-string/iot-hub-get-connection-vs2019.png)

IoT Hub paylaşılan erişim ilkeleri ve izinleri hakkında daha fazla bilgi için [Erişim denetimi ve izinleri](../articles/iot-hub/iot-hub-devguide-security.md#access-control-and-permissions)bkz.
