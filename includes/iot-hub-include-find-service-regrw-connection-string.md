---
title: include dosyası
description: include dosyası
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 04/03/2020
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 1a0502568c1673bcd7f57d3e9bc9c95ed90bbefa
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80756937"
---
<!-- This tells how to create a custom shared access policy that has service connect and registry RW permissions for your IoT hub and get the connection string for it-->

**Hizmet bağlantısı** ve kayıt **defteri yazma** izinleri veren paylaşılan bir erişim ilkesi oluşturmak ve bu ilke için bağlantı dizesi almak için aşağıdaki adımları izleyin:

1. Azure [portalında](https://portal.azure.com) **Kaynak gruplarını**seçin. Hub'ınızın bulunduğu kaynak grubunu seçin ve ardından kaynak listesinden hub'ınızı seçin.

1. Hub'ınızın sol tarafındaki bölmede **Paylaşılan erişim ilkeleri'ni**seçin.

1. İlkeler listesinin üst menüsünden **Ekle'yi**seçin.

1. **Paylaşılan erişim ilkesi ekle**altında, *hizmetAndRegistryReadWrite*gibi ilkeniz için açıklayıcı bir ad girin. **İzinler** **altında, Kayıt Defteri yaz** ve Hizmet **bağlan'ı**seçin ve sonra **Oluştur'u**seçin. (Kayıt Defteri **yazma**seçeneğini seçtiğinizde **Kayıt Defteri okuma** izni otomatik olarak dahil edilir.)

    ![Yeni bir paylaşılan erişim ilkesinin nasıl ekleyeceğini göster](./media/iot-hub-include-find-service-regrw-connection-string/iot-hub-add-svc-regrw-policy.png)

1. İlkeler listesinden yeni ilkenizi seçin.

1. **Paylaşılan erişim tuşları**altında, Bağlantı **dizesi** için kopya simgesini seçin -- birincil anahtar ve değeri kaydedin.

    ![Bağlantı dizesinin nasıl alınır göster](./media/iot-hub-include-find-service-regrw-connection-string/iot-hub-get-connection-string.png)

IoT Hub paylaşılan erişim ilkeleri ve izinleri hakkında daha fazla bilgi için [Erişim denetimi ve izinleri](../articles/iot-hub/iot-hub-devguide-security.md#access-control-and-permissions)bkz.
