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
ms.openlocfilehash: 6f43bbcd83861f7d39de2aa89bbe035c2ff5b809
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "70050460"
---
<!-- This tells how to create a custom shared access policy for your IoT hub and get the connection string for it-->

**Hizmet bağlantısı** ve kayıt **defteri okuma** izinleri veren paylaşılan bir erişim ilkesi oluşturmak ve bu ilke için bir bağlantı dizesi almak için aşağıdaki adımları izleyin:

1. Azure [portalında](https://portal.azure.com) **Kaynak gruplarını**seçin. Hub'ınızın bulunduğu kaynak grubunu seçin ve ardından kaynak listesinden hub'ınızı seçin.

1. Hub'ınızın sol tarafındaki bölmede **Paylaşılan erişim ilkeleri'ni**seçin.

1. İlkeler listesinin üst menüsünden **Ekle'yi**seçin.

1. **Paylaşılan erişim ilkesi ekle**altında, *hizmetAndRegistryRead*gibi ilkeniz için açıklayıcı bir ad girin. **İzinler**altında, **Kayıt Defteri okuma** ve Hizmet **bağlantısı'nı**seçin ve ardından **Oluştur'u**seçin.

    ![Yeni bir paylaşılan erişim ilkesinin nasıl ekleyeceğini göster](./media/iot-hub-include-find-custom-connection-string/iot-hub-add-custom-policy.png)

1. İlkeler listesinden yeni ilkenizi seçin.

1. **Paylaşılan erişim tuşları**altında, Bağlantı **dizesi** için kopya simgesini seçin -- birincil anahtar ve değeri kaydedin.

    ![Bağlantı dizesinin nasıl alınır göster](./media/iot-hub-include-find-custom-connection-string/iot-hub-get-connection-string.png)

IoT Hub paylaşılan erişim ilkeleri ve izinleri hakkında daha fazla bilgi için [Erişim denetimi ve izinleri](../articles/iot-hub/iot-hub-devguide-security.md#access-control-and-permissions)bkz.
