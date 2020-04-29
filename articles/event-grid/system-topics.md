---
title: Azure Event Grid sistem konuları
description: Azure Event Grid içindeki Sistem konularını açıklar.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: spelluru
ms.openlocfilehash: 46bceeb31fa38068c6c4f9f3a86ed556ad39effb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81393162"
---
# <a name="system-topics-in-azure-event-grid"></a>Azure Event Grid sistem konuları
Azure Event Grid hizmeti, bir Azure Olay kaynağı için ilk olay aboneliği oluştururken sistem konuları oluşturur. Şu anda, Event Grid 11, 15, 2020 tarihinden önce oluşturulan konu kaynakları için sistem konuları oluşturmaz. Bu tarihte veya bu tarihten sonra oluşturduğunuz tüm konu kaynakları için Event Grid otomatik olarak sistem konuları oluşturur. Bu makalede Azure Event Grid içindeki **sistem konuları** açıklanmaktadır.

> [!NOTE]
> Bu özellik şu anda Azure Kamu Bulutu için etkinleştirilmemiş. 

## <a name="overview"></a>Genel Bakış
Azure depolama hesabı gibi bir Azure Olay kaynağı için ilk olay aboneliği oluşturduğunuzda, abonelik için sağlama işlemi **Microsoft. EventGrid/Systemkonuları**türünde ek bir kaynak oluşturur. Azure olay kaynağına yönelik son olay aboneliği silindiğinde, sistem konusu otomatik olarak silinir.

Sistem konusu özel konu senaryoları, diğer bir deyişle Event Grid konular ve Event Grid etki alanları için geçerli değildir. 

## <a name="location"></a>Konum
Belirli bir bölgedeki/konumdaki Azure olay kaynakları için sistem konusu, Azure Olay kaynağıyla aynı konumda oluşturulur. Örneğin, Doğu ABD bir Azure Blob depolama alanı için bir olay aboneliği oluşturursanız, sistem konusu Doğu ABD oluşturulur. Azure abonelikleri, kaynak grupları veya Azure haritaları gibi genel Azure olay kaynakları için Event Grid, **genel** konumdaki Sistem konusunu oluşturur. 

## <a name="resource-group"></a>Kaynak grubu 
Genel olarak, sistem konusu Azure olay kaynağının bulunduğu kaynak grubunda oluşturulur. Azure abonelik kapsamında oluşturulan olay abonelikleri için, sistem konusu **varsayılan-EventGrid**kaynak grubu altında oluşturulur. Kaynak grubu yoksa, Azure Event Grid Sistem konusunu oluşturmadan önce onu oluşturur. 

Kaynak grubunu depolama hesabıyla silmeye çalıştığınızda, etkilenen kaynaklar listesinde Sistem konusunu görürsünüz.  

![Kaynak grubunu silme](./media/system-topics/delete-resource-group.png)

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın: 

- [Özel konular](custom-topics.md)
- [Etki alanları](event-domains.md)