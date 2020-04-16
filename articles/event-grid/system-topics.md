---
title: Azure Etkinlik Grid'inde sistem konuları
description: Azure Olay Grid'inde sistem konularını açıklar.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: spelluru
ms.openlocfilehash: 46bceeb31fa38068c6c4f9f3a86ed556ad39effb
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393162"
---
# <a name="system-topics-in-azure-event-grid"></a>Azure Etkinlik Grid'inde sistem konuları
Azure Olay Ağı hizmeti, bir Azure etkinlik kaynağı için ilk etkinlik aboneliği oluşturduğunuzda sistem konuları oluşturur. Şu anda, Olay Izgara Mar, 15, 2020 önce oluşturulan konu kaynakları için sistem konuları oluşturmaz. Bu tarihte veya bu tarihten sonra oluşturduğunuz tüm konu kaynakları için Olay Idamı otomatik olarak sistem konuları oluşturur. Bu makalede, Azure Olay **Grid'indeki sistem konuları** açıklanmaktadır.

> [!NOTE]
> Bu özellik şu anda Azure Kamu bulutu için etkinleştirildi. 

## <a name="overview"></a>Genel Bakış
Azure Depolama hesabı gibi bir Azure etkinlik kaynağı için ilk olay aboneliği oluşturduğunuzda, abonelik için sağlama işlemi **Microsoft.EventGrid/systemTopics**türünden ek bir kaynak oluşturur. Azure olay kaynağının son olay aboneliği silindiğinde, sistem konusu otomatik olarak silinir.

Sistem konusu özel konu senaryoları, yani Olay Izgara konuları ve Olay Izgara etki alanları için geçerli değildir. 

## <a name="location"></a>Konum
Belirli bir bölgede/konumda bulunan Azure olay kaynakları için sistem konusu, Azure etkinlik kaynağıyla aynı konumda oluşturulur. Örneğin, Doğu ABD'deki bir Azure blob depolama alanı için etkinlik aboneliği oluşturursanız, sistem konusu Doğu ABD'de oluşturulur. Azure abonelikleri, kaynak grupları veya Azure Haritalar gibi genel Azure etkinlik kaynakları için Olay Ağı, sistem konusunu **genel** konumda oluşturur. 

## <a name="resource-group"></a>Kaynak grubu 
Genel olarak, sistem konusu Azure olay kaynağının içinde olduğu kaynak grubunda oluşturulur. Azure abonelik kapsamında oluşturulan etkinlik abonelikleri için **varsayılan-eventgrid**kaynak grubu altında sistem konusu oluşturulur. Kaynak grubu yoksa, sistem konusunu oluşturmadan önce Azure Olay Ağıt'ı oluşturur. 

Depolama hesabıyla kaynak grubunu silmeye çalıştığınızda, etkilenen kaynaklar listesinde sistem konusunu görürsünüz.  

![Kaynak grubunu silme](./media/system-topics/delete-resource-group.png)

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın: 

- [Özel konular](custom-topics.md)
- [Etki alanları](event-domains.md)