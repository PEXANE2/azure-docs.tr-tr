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
ms.openlocfilehash: b6ca43616a2e7e7611b122bce5c95084e1fd5012
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68402383"
---
<!-- This tells how to get the connection string for the service shared access policy of your IoT hub -->

**Hizmet** ilkesi için IoT Hub bağlantı dizesi almak için aşağıdaki adımları izleyin:

1. IoT Hub 'ınızı [Azure Portal](https://portal.azure.com)açın.  IoT Hub 'ınıza almanın en kolay yolu **kaynak grupları**seçmek, IoT Hub 'ınızın bulunduğu kaynak grubunu seçmek ve ardından kaynak listesinden IoT Hub 'ınızı seçmeniz gerekir.

2. IoT Hub 'ınızın sol tarafındaki bölmede **paylaşılan erişim ilkeleri**' ni seçin.

3. İlke listesinden **hizmet** İlkesi ' ni seçin.

4. **Paylaşılan erişim anahtarları**altında, **bağlantı dizesi--birincil anahtar** için Kopyala simgesini seçin ve değeri kaydedin.

    ![Bağlantı dizesinin nasıl alınacağını göster](./media/iot-hub-include-find-service-connection-string/iot-hub-get-connection-string.png)

IoT Hub paylaşılan erişim ilkeleri ve izinleri hakkında daha fazla bilgi için bkz. [erişim denetimi ve izinleri](../articles/iot-hub/iot-hub-devguide-security.md#access-control-and-permissions).
