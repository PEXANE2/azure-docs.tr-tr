---
title: include dosyası
description: include dosyası
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 11/02/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 55dc712d323c43849fa3de23c83e29fc348a0f1e
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68404000"
---
<!-- This tells how to create a custom shared access policy for your IoT hub and get the connection string for it-->

**Hizmet Connect** ve **kayıt defteri okuma** izinleri veren ve bu ilke için bir bağlantı dizesi alan bir paylaşılan erişim ilkesi oluşturmak için şu adımları izleyin:

1. IoT Hub 'ınızı [Azure Portal](https://portal.azure.com)açın. IoT Hub 'ınıza almanın en kolay yolu **kaynak grupları**seçmek, IoT Hub 'ınızın bulunduğu kaynak grubunu seçmek ve ardından kaynak listesinden IoT Hub 'ınızı seçmeniz gerekir.

2. IoT Hub 'ınızın sol tarafındaki bölmede **paylaşılan erişim ilkeleri**' ni seçin.

3. İlke listesinin üstündeki en üstteki menüden **Ekle**' yi seçin.

4. **Paylaşılan erişim Ilkesi Ekle** bölmesinde, ilkeniz için açıklayıcı bir ad girin; Örneğin: *Serviceandregistryread*. **İzinler**altında **kayıt defteri okuma** ve **hizmet Connect**' i seçin ve ardından **Oluştur**' u seçin.

    ![Yeni bir paylaşılan erişim ilkesinin nasıl ekleneceğini göster](./media/iot-hub-include-find-custom-connection-string/iot-hub-add-custom-policy.png)

5. **Paylaşılan erişim ilkeleri** bölmesine geri döndüğünüzde, ilkeler listesinden yeni ilkenizi seçin.

6. **Paylaşılan erişim anahtarları**altında, **bağlantı dizesi--birincil anahtar** için Kopyala simgesini seçin ve değeri kaydedin.

    ![Bağlantı dizesinin nasıl alınacağını göster](./media/iot-hub-include-find-custom-connection-string/iot-hub-get-connection-string.png)

IoT Hub paylaşılan erişim ilkeleri ve izinleri hakkında daha fazla bilgi için bkz. [erişim denetimi ve izinleri](../articles/iot-hub/iot-hub-devguide-security.md#access-control-and-permissions).
