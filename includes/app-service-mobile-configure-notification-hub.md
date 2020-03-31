---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: da3793c428c624ce3a224cbd7606ab26c4a50803
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188874"
---
Azure Uygulama Hizmeti'nin Mobil Uygulamalar özelliği, itme göndermek için [Azure Bildirim Hub'larını] kullanır, böylece mobil uygulamanız için bir bildirim hub'ı yapılandırırsınız.

1. Azure [portalında,] **Uygulama Hizmetleri'ne**gidin ve ardından uygulamanızı arka uca seçin. **Ayarlar**altında **İtme'yi**seçin.
2. Uygulamaya bir bildirim hub kaynağı eklemek için **Bağlan'ı**seçin. Hub oluşturabilir veya varolan bir hub'a bağlanabilirsiniz.

    ![Hub'ı yapılandırma](./media/app-service-mobile-create-notification-hub/configure-hub-flow.png)

Artık Mobil Uygulamalar arka uç projenize bir bildirim hub'ı bağladınız. Daha sonra bu bildirim merkezini aygıtlara itmek için bir platform bildirim sistemine (PNS) bağlanmak üzere yapılandırırsınız.

[Azure portalında]: https://portal.azure.com/
[Azure Notification Hubs]: https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/
