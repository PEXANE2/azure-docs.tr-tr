---
title: include dosyası
description: include dosyası
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 07/16/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 55cdd864d73ce084d994c64233e79d5a58b17def
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "69558464"
---
<!-- This tells how to get the connection string for the service shared access policy of your IoT hub -->

**Hizmet** ilkesi için IoT Hub bağlantı dizesini almak için aşağıdaki adımları izleyin:

1. Azure [portalında](https://portal.azure.com) **Kaynak gruplarını**seçin. Hub'ınızın bulunduğu kaynak grubunu seçin ve ardından kaynak listesinden hub'ınızı seçin.

1. IoT hub'ınızın sol tarafındaki bölmede **Paylaşılan erişim ilkeleri'ni**seçin.

1. İlkeler listesinden **hizmet** ilkesini seçin.

1. **Paylaşılan erişim tuşları**altında, Bağlantı **dizesi** için kopya simgesini seçin -- birincil anahtar ve değeri kaydedin.

    ![Bağlantı dizesinin nasıl alınır göster](./media/iot-hub-include-find-service-connection-string/iot-hub-get-connection-string.png)

IoT Hub paylaşılan erişim ilkeleri ve izinleri hakkında daha fazla bilgi için [Erişim denetimi ve izinleri](../articles/iot-hub/iot-hub-devguide-security.md#access-control-and-permissions)bkz.
