---
title: dosya dahil etme
description: dosya dahil etme
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 04/03/2020
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 1a0502568c1673bcd7f57d3e9bc9c95ed90bbefa
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80756937"
---
<!-- This tells how to create a custom shared access policy that has service connect and registry RW permissions for your IoT hub and get the connection string for it-->

**Hizmet Connect** ve **kayıt defteri yazma** izinleri veren bir paylaşılan erişim ilkesi oluşturmak ve bu ilke için bir bağlantı dizesi almak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) **kaynak grupları**' nı seçin. Hub 'ınızın bulunduğu kaynak grubunu seçin ve ardından kaynak listesinden hub 'ınızı seçin.

1. Hub 'ınızın sol tarafındaki bölmede **paylaşılan erişim ilkeleri**' ni seçin.

1. İlke listesinin üstündeki en üstteki menüden **Ekle**' yi seçin.

1. **Paylaşılan erişim Ilkesi Ekle**' nin altında, Ilkeniz Için *Serviceandregistryreadwrite*gibi açıklayıcı bir ad girin. **İzinler**altında, **kayıt defteri yazma** ve **hizmet Connect**' i seçin ve ardından **Oluştur**' u seçin. (Kayıt defteri **okuma** Izni, **kayıt defteri yazma**seçeneğini belirlediğinizde otomatik olarak eklenir.)

    ![Yeni bir paylaşılan erişim ilkesinin nasıl ekleneceğini göster](./media/iot-hub-include-find-service-regrw-connection-string/iot-hub-add-svc-regrw-policy.png)

1. İlke listesinden yeni ilkenizi seçin.

1. **Paylaşılan erişim anahtarları**altında, **bağlantı dizesi--birincil anahtar** için Kopyala simgesini seçin ve değeri kaydedin.

    ![Bağlantı dizesinin nasıl alınacağını göster](./media/iot-hub-include-find-service-regrw-connection-string/iot-hub-get-connection-string.png)

IoT Hub paylaşılan erişim ilkeleri ve izinleri hakkında daha fazla bilgi için bkz. [erişim denetimi ve izinleri](../articles/iot-hub/iot-hub-devguide-security.md#access-control-and-permissions).
