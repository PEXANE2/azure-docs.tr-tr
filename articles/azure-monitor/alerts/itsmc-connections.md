---
title: Azure Izleyici 'de BT Hizmet Yönetimi Bağlayıcısı
description: Bu makalede, ITSM iş öğelerini merkezi olarak izlemek ve yönetmek için ITSM ürünlerinizi/hizmetlerinizi Azure Izleyici 'de BT Hizmet Yönetimi Bağlayıcısı (ITSMC) ile bağlama hakkında bilgi sağlanır.
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 05/12/2020
ms.openlocfilehash: 046fcb9d7473de5666b3acb25cbcaa1f9549e679
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102039504"
---
# <a name="connect-itsm-productsservices-with-it-service-management-connector"></a>BT Hizmet Yönetimi Bağlayıcısı ile ITSM ürünlerine/hizmetlerine bağlanma
Bu makalede, çalışma öğelerinizi merkezi olarak yönetmek için ıTSM ürününüz/hizmetiniz ile Log Analytics BT Hizmet Yönetimi Bağlayıcısı (ITSMC) arasındaki bağlantının nasıl yapılandırılacağı hakkında bilgi sağlanır. ISMC hakkında daha fazla bilgi için bkz. [genel bakış](./itsmc-overview.md).

Aşağıdaki ıTSM ürünleri/hizmetleri desteklenir. Ürünün ıSMC 'a nasıl bağlanacağı hakkında ayrıntılı bilgileri görüntülemek için ürünü seçin.

- [ServiceNow](./itsmc-connections-servicenow.md)
- [System Center Service Manager](./itsmc-connections-scsm.md)
- [Cherwell](./itsmc-connections-cherwell.md)
- [Provance](./itsmc-connections-provance.md)

> [!NOTE]
> Çeriyi ve Provance müşterilerinin, tümleştirmede başka bir çözüm olarak, Cherwell ve Provance uç noktasına [Web kancası eylemi](./action-groups.md#webhook) kullanmasını sağlıyoruz.

## <a name="next-steps"></a>Sonraki adımlar

* [ITSM Bağlayıcısındaki sorunları giderme](./itsmc-resync-servicenow.md)