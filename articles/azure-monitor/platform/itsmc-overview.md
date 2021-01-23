---
title: BT Hizmet Yönetimi Bağlayıcısı genel bakış
description: Bu makalede BT Hizmet Yönetimi Bağlayıcısı (ıSMC) genel bakışı sunulmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/16/2020
ms.custom: references_regions
ms.openlocfilehash: 6d9ad775f91778f95380a19fbe253e2cbbebd3fc
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2021
ms.locfileid: "98736465"
---
# <a name="it-service-management-connector-overview"></a>BT Hizmet Yönetimi Bağlayıcısı genel bakış

:::image type="icon" source="media/itsmc-overview/itsmc-symbol.png":::

BT Hizmet Yönetimi Bağlayıcısı (ITSMC), Azure 'un desteklenen bir BT hizmet yönetimi (ıTSM) ürününe veya hizmetine bağlanmasını sağlar.

Azure Log Analytics ve Azure Izleyici gibi Azure Hizmetleri, Azure ve Azure dışı kaynaklarla ilgili sorunları tespit etmek, çözümlemek ve sorunlarını gidermek için araçlar sağlar. Ancak bir sorunla ilgili iş öğeleri genellikle ıTSM ürünü veya hizmetinde bulunur. ISMC, sorunları daha hızlı çözmenize yardımcı olmak için Azure ile ıTSM araçları arasında çift yönlü bir bağlantı sağlar.

## <a name="configuration-steps"></a>Yapılandırma adımları

ISMC aşağıdaki ıTSM araçlarıyla bağlantıları destekler:

-   ServiceNow
-   System Center Service Manager
-   Provance
-   Cherwell

   >[!NOTE]
> Çeriyi ve Provance müşterilerinin, tümleştirmede başka bir çözüm olarak, Cherwell ve Provance uç noktasına [Web kancası eylemi](./action-groups.md#webhook) kullanmasını sağlıyoruz.

ISMC ile şunları yapabilirsiniz:

-  ITSM aracınız için Azure uyarılarınıza (ölçüm uyarıları, etkinlik günlüğü uyarıları ve Log Analytics Uyarıları) göre iş öğeleri oluşturun.
-  İsteğe bağlı olarak, ıTSM aracınızdan olay ve değişiklik isteği verilerini Azure Log Analytics çalışma alanına eşitleyebilirsiniz.

Yasal koşullar ve Gizlilik ilkesi hakkında bilgi için bkz. [Microsoft gizlilik bildirimi](https://go.microsoft.com/fwLink/?LinkID=522330&clcid=0x9).

Aşağıdaki adımları tamamlayarak ıSMC kullanmaya başlayabilirsiniz:

1. [Azure 'dan gelen uyarıları kabul etmek için ıTSM ortamınızı ayarlayın.](./itsmc-connections.md)
1. [Azure ıTSSM çözümünü yapılandırma](./itsmc-definition.md#add-it-service-management-connector)
1. [ITSSM ortamınız için Azure ıTSM bağlayıcısını yapılandırın.](./itsmc-definition.md#create-an-itsm-connection)
1. [ITSM bağlayıcısından yararlanmak için eylem grubunu yapılandırın.](./itsmc-definition.md#use-itsmc)

## <a name="next-steps"></a>Sonraki adımlar

* [ITSM Bağlayıcısındaki sorunları giderme](./itsmc-resync-servicenow.md)