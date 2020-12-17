---
title: BT Hizmet Yönetimi Bağlayıcısı genel bakış
description: Bu makalede BT Hizmet Yönetimi Bağlayıcısı (ıSMC) genel bakışı sunulmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/16/2020
ms.custom: references_regions
ms.openlocfilehash: aaaeb23ef24f0d8a0fa4b38139fed57cda6fa63d
ms.sourcegitcommit: ad677fdb81f1a2a83ce72fa4f8a3a871f712599f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97657048"
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
> Çeriyi ve Provance müşterilerinin, tümleştirmede başka bir çözüm olarak, Cherwell ve Provance uç noktasına [Web kancası eylemi](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups#webhook) kullanmasını sağlıyoruz.

ISMC ile şunları yapabilirsiniz:

-  ITSM aracınız için Azure uyarılarınıza (ölçüm uyarıları, etkinlik günlüğü uyarıları ve Log Analytics Uyarıları) göre iş öğeleri oluşturun.
-  İsteğe bağlı olarak, ıTSM aracınızdan olay ve değişiklik isteği verilerini Azure Log Analytics çalışma alanına eşitleyebilirsiniz.

Yasal koşullar ve Gizlilik ilkesi hakkında bilgi için bkz. [Microsoft gizlilik bildirimi](https://go.microsoft.com/fwLink/?LinkID=522330&clcid=0x9).

Aşağıdaki adımları tamamlayarak ıSMC kullanmaya başlayabilirsiniz:

1. [BT Hizmet Yönetimi Bağlayıcısı ile ıSM ürünlerini/hizmetlerini bağlayın.](./itsmc-connections.md)
1. [ISMC ekleyin.](/.itsmc-definition.md#add-it-service-management-connector)
1. [ITSM bağlantısı oluşturun.](./itsmc-definition.md#create-an-itsm-connection)
1. [Bağlantıyı kullanın.](./itsmc-definition.md#use-itsmc)

## <a name="next-steps"></a>Sonraki adımlar

BT Hizmet Yönetimi Bağlayıcısı için ıTSM [ürünleri/hizmetleri ekleyin](./itsmc-connections.md) 
 [ITSM Bağlayıcısı Ekle](./itsmc-definition.md) 
 [ITSM Bağlayıcısı sorunlarını giderme](./itsmc-resync-servicenow.md)